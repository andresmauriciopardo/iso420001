---
titulo: "ADR-008: Almacenamiento de objetos S3-compatible, hash en servidor y escaneo antivirus"
fecha: 2026-09-17
estado: "propuesto, requiere aprobación humana"
nivel: 3
decisiones_spine: [AD-13, AD-2, AD-20]
inputs:
  - _bmad-output/planning-artifacts/architecture/architecture-ISO42001-2026-09-17/ARCHITECTURE-SPINE.md
  - CLAUDE.md
  - _bmad-output/planning-artifacts/prds/prd-ISO42001-2026-09-17/prd.md (§17 D-7, Q6)
  - docs/product/ROADMAP.md (§3)
sustituye: []
---
# ADR-008: Almacenamiento de objetos S3-compatible, hash en servidor y escaneo antivirus

## Contexto

Evidence, Documents & Policies, Reporting, Audit y Administration manejan binarios: evidencias subidas, políticas exportadas, informes PDF/CSV, exportaciones del registro de auditoría. AD-2 obliga a aislar por tenant también en almacenamiento; AD-13 fija que los binarios nunca van a la base de datos, que el hash se calcula en servidor y que nada pasa a `Collected` sin escaneo. MinIO Community Edition fue archivado en 2026-04, y Garage tiene licencia AGPLv3, que CLAUDE.md clasifica como copyleft sujeto a aprobación humana. SeaweedFS 4.47 (Apache-2.0) ofrece pasarela S3 y queda como semilla local.

## Decisión

1. **Puerto `ObjectStorage`** en `packages/storage` con operaciones `put(stream, key, contentType) → { sha256, size, etag }`, `getSignedDownloadUrl(key, ttl)`, `head(key)`, `copy(from, to)` y `remove(key)`. `remove` solo lo invoca el job de retención de Administration (ADR-015); ningún contexto lo llama directamente.
2. **Adaptador S3** único para todos los entornos. Local y CI: SeaweedFS 4.47 en `docker-compose` `[ASSUMPTION]`. Staging y producción: S3 gestionado del proveedor de nube (pendiente Q6). El adaptador se prueba por contrato contra ambos.
3. **Clave** `tenants/<tenant_id>/<domain>/<object_id>/<version>`; el prefijo lo impone el adaptador a partir de `TenantContext`, nunca lo compone el llamador. Bucket privado, sin ACL públicas, cifrado en reposo del proveedor `[ASSUMPTION]`.
4. **Hash en servidor.** El route handler entrega el stream al adaptador, que calcula SHA-256 mientras escribe y lo devuelve; el dominio lo persiste en la entidad (`Evidence.sha256`, `ReportRun.sha256`). Cualquier hash enviado por el cliente se ignora.
5. **Higiene de archivos.** Lista blanca de tipos MIME por dominio, validada por cabecera declarada y por bytes mágicos `[ASSUMPTION]`; tamaño máximo configurable por tenant (D-7). El objeto entra en estado `Pending Scan`; el job `object-scan` (pg-boss) lo envía a ClamAV 1.4 LTS (`clamd` INSTREAM) y emite `OBJECT_SCAN_COMPLETED { verdict: clean | infected | error }` `[ASSUMPTION nombres]`. Solo con `clean` la evidencia pasa a `Collected`; con `infected` se mueve al prefijo `quarantine/`, se emite evento y notificación al propietario; con `error` se reintenta (AD-5).
6. **Descarga** solo por URL firmada de vida ≤ 15 min `[ASSUMPTION]`, emitida por un servicio de aplicación tras `authorize(evidence.read, object)`; la emisión genera evento `EVIDENCE_DOWNLOADED` `[ASSUMPTION nombre]` para el registro de auditoría.

## Alternativas consideradas

| Alternativa | Por qué se descarta |
| --- | --- |
| Binarios en PostgreSQL (`bytea` / large objects) | Infla copias de seguridad y RLS sobre blobs; no escala con 7 años de retención; AD-13 lo prohíbe. |
| MinIO Community Edition | Archivado en 2026-04; sin mantenimiento de seguridad. |
| Garage | Funcional, pero AGPLv3: dependencia copyleft que exige aprobación humana explícita (CLAUDE.md); se prefiere una opción permisiva. |
| Escaneo sincrónico en la petición | Bloquea la subida y agota el pool; ClamAV puede tardar segundos en archivos grandes. |
| Sin escaneo, confiando en el proveedor | Los proveedores S3 no escanean por defecto; la evidencia circula entre auditores. |

## Consecuencias

**Positivas.** Un solo puerto para cinco contextos; aislamiento de tenant por prefijo comprobable; integridad demostrable (`sha256` reproducible); evidencia nunca visible antes de escanear; sustitución del servidor S3 sin tocar dominio.

**Negativas.** Dos contenedores más en local (`seaweedfs`, `clamav`); ClamAV consume memoria significativa en arranque `[ASSUMPTION]`; la compatibilidad S3 de SeaweedFS no es total, por eso la prueba de contrato corre contra el proveedor real antes de `staging`.

**Cambios OpenSpec.** `foundation-project-bootstrap` (paquete `storage`, `docker-compose` con `seaweedfs` y `clamav`); `evidence-library` (puerto en uso, estados `Pending Scan`/`Collected`, job `object-scan`, URLs firmadas); `reporting-essentials` (salida de `ReportRun`); `internal-audit-core` (exportaciones); `immutable-audit-trail` (exportación del registro); `mvp-hardening` (pruebas de seguridad y rendimiento de subida).

## Verificación

- Prueba de contrato de `ObjectStorage` contra SeaweedFS en CI (Testcontainers `[ASSUMPTION]`) y, antes de `staging`, contra el S3 gestionado elegido.
- `tests/security`: un principal del tenant A solicita una clave `tenants/<B>/...` → `TENANT_MISMATCH`; una URL firmada caducada devuelve error; un objeto sin `OBJECT_SCAN_COMPLETED` nunca alcanza `Collected`.
- Prueba con el archivo de prueba EICAR: veredicto `infected`, objeto en `quarantine/`, evento emitido.
- `dependency-cruiser` `[ASSUMPTION]`: el SDK de S3 solo se importa en `packages/storage`.
- Escaneo de licencias en CI: falla ante dependencias AGPL o GPL no aprobadas.

## Estado y aprobación

Propuesto. Requiere aprobación humana por tres motivos: (a) fija infraestructura local y el puerto que usarán cinco contextos (bloqueo de arquitectura); (b) el descarte de Garage equivale a una política "sin copyleft salvo aprobación" que debe ratificarse; (c) el escaneo antivirus, las URLs firmadas y el TTL de 15 minutos son arquitectura de seguridad. El proveedor S3 de producción depende de Q6 (checkpoint 3).
