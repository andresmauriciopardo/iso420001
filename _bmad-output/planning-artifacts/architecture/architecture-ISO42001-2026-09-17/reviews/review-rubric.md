# Revisión rubric-walker — ARCHITECTURE-SPINE.md (2026-09-17)

- **Artefacto revisado:** `_bmad-output/planning-artifacts/architecture/architecture-ISO42001-2026-09-17/ARCHITECTURE-SPINE.md`
- **Insumos contrastados:** `docs/product/DOMAIN_MAP.md` §3-§7, `docs/product/ROADMAP.md` §3 (24 cambios), PRD §7 (NFR-001..024) y §9 (PR-DR-001..030), `CLAUDE.md`, listado de `docs/decisions/` y `docs/architecture/`.
- **Lista de comprobación:** good-spine checklist (8 puntos). Revisión de solo lectura; no se modificó la espina ni ningún insumo.

## Veredicto

La espina fija bien el paradigma, la tenencia, el bus de eventos y los contratos de API/puntuación, pero **no está lista como sustrato de construcción**: el orden real de los cambios 2-5 contradice AD-4/AD-6, y quedan sin propietario ni mecanismo cuatro puntos donde cambios construidos por separado divergirían con seguridad (canal íntegro del outbox hacia el registro de auditoría, rol de sistema frente a RLS, `ScoreSnapshot`, proyección de búsqueda).

## Resumen por punto de la checklist

| # | Punto | Resultado |
| --- | --- | --- |
| 1 | Fija los puntos de divergencia de los 24 cambios sin omitir ninguno | Parcial: los 24 aparecen en el mapa, pero el mapa contradice el ROADMAP en los cambios 1-5 (H-1) y deja sin fijar tres tablas transversales (H-4, H-5, M-5) |
| 2 | Rules exigibles y que previenen lo declarado | Parcial: AD-3 e AD-12 tienen mecanismo; AD-4, AD-8 (alcances), AD-11 y AD-14 no (M-3, M-4, M-5, M-6) |
| 3 | Nada en Deferred permite divergencia incompatible | Parcial: la fila "búsqueda" difiere el motor pero no la propiedad de la proyección (H-5); falta diferir o decidir copias/DR (M-1) |
| 4 | Stack coherente internamente | Mayormente sí; faltan filas para herramientas nombradas en AD (M-7) y los compañeros citan ADR-001..015 cuando existe ADR-016 (L-1) |
| 5 | Cobertura de FR, PR-DR y NFR | Parcial: NFR-001 (rate limiting, CSRF, cabeceras), NFR-004 (100 MB), NFR-008 (copias/DR) y FR-093 (auditor externo) sin AD (M-1, M-2, M-4) |
| 6 | Dimensiones estructurales decididas/diferidas/abiertas | Silenciadas: operaciones (copias, DR, runbooks) y roles de base de datos para procesos de sistema (H-3, M-1) |
| 7 | Coherencia interna de nombres | Fallos menores: `AuditLogEntry`/`audit_entries`, `Audit report`, `RiskMethodology`, `Job`, `CREDENTIAL_*` (M-5, M-6, L-2) |
| 8 | Sin placeholders, Mermaid válido | Correcto: los tres diagramas son sintácticamente válidos; sin comentarios de plantilla. Única referencia externa no resoluble en el repo: "memlog" (L-4) |

## Hallazgos

### C-1 (critical) — Mapa Capacidad→Arquitectura y ROADMAP incompatibles en los cambios 1-5: el outbox llega después de quien lo necesita

- **Sección afectada:** Capability → Architecture Map (filas 1, 2, 4, 5); AD-4; AD-6.
- **Problema:** AD-4 (`Binds: all`) exige que toda mutación anexe al outbox en la misma transacción, y AD-6 define el `audit-trail-writer` como consumidor pg-boss del outbox. Sin embargo, el mapa sitúa `packages/events` y `packages/jobs` en el cambio 5 `domain-events-outbox-jobs`, mientras el ROADMAP exige `TENANT_CREATED` en outbox como criterio de salida del cambio 2, y el cambio 4 `immutable-audit-trail` (que precede al 5) exige "entrada por cada mutación de los cambios 2-3". Construidos por separado, los cambios 2-4 tendrían que inventar su propio mecanismo de escritura de outbox y de alimentación del registro de auditoría (sin pg-boss), y el cambio 5 lo sustituiría después: divergencia garantizada y retrabajo.
- **Corrección propuesta:** reescribir las filas 1 y 5 del mapa y añadir una frase a AD-6:
  - Fila 1: `foundation-project-bootstrap` | `apps/*`, `packages/kernel, db, i18n, ui, observability`, **`packages/events` (tabla `outbox_events` + `appendOutbox()` dentro de `withTenantTransaction`)**, **`packages/jobs` (puerto `JobScheduler` + adaptador pg-boss, sin consumidores)** | AD-1, AD-3, AD-4, AD-5 (envelope), AD-12, AD-19, AD-20, AD-22, AD-23.
  - Fila 5: `domain-events-outbox-jobs` | `packages/events` (relay, `processed_events`, reintentos, `dead_letter`, catálogo de eventos DOMAIN_MAP §5, `object_versions` FR-009), `domain-workflow` (Approval mínimo) | AD-5, AD-10, AD-11, AD-19.
  - AD-6, Rule (añadir): "El `audit-trail-writer` **no consume colas `events.<TYPE>`**: lee `outbox_events` íntegro por `(tenant_id, id)` con cursor persistido `audit_cursors(tenant_id, last_event_id)`, ejecutado como job singleton por tenant. Este modo existe desde el cambio 4 (polling por `JobScheduler`) y no cambia al llegar el relay en el cambio 5."

### H-1 (high) — AD-5 y AD-6 se contradicen sobre el canal y el orden

- **Sección afectada:** AD-5, AD-6.
- **Problema:** AD-5 hace fan-out del outbox a colas por tipo (`events.<TYPE>`); AD-6 exige que Administration procese "el outbox completo serializado por tenant" con `sequence` por tenant y una "clave singleton pg-boss por tenant". Las claves singleton de pg-boss se aplican por cola; con N colas por tipo se pierde el orden total por tenant y no hay un único consumidor que vea el flujo íntegro. Además `ENTITY_MUTATED` (AD-4) no está en DOMAIN_MAP §5 y nadie declara quién lo consume. Dos equipos (cambio 4 y cambio 5) elegirían mecanismos distintos.
- **Corrección propuesta:** AD-5, Rule (añadir): "Además del fan-out por tipo, todo evento queda disponible para los **consumidores íntegros** (`audit-trail-writer`, `search-indexer`) que leen `outbox_events` por cursor y nunca por colas de tipo. `sequence` de `audit_entries` es el orden de inserción por tenant del propio registro, no del evento. `ENTITY_MUTATED` no se enruta a colas de tipo; solo lo leen los consumidores íntegros." Registrar `ENTITY_MUTATED` en DOMAIN_MAP §5 (propagación hacia arriba según la jerarquía de CLAUDE.md).

### H-2 (high) — AD-2 no decide cómo operan los procesos de sistema bajo RLS forzado

- **Sección afectada:** AD-2, AD-20, AD-7.
- **Problema:** AD-2 define un único rol de aplicación sin `BYPASSRLS` y `SET LOCAL app.tenant_id` por transacción, y declara el catálogo global "de solo lectura para el rol de aplicación". Pero el relay del outbox, `audit-trail-writer`, `retention-sweeper`, `REPORT_SCHEDULE_DUE`, el cargador de `standards-seed` (escribe el catálogo) y el script del tenant demo operan sobre varios tenants o escriben el catálogo. Sin decisión, el cambio 5 (relay), el 6 (loader), el 13/21 (sweeper, expiración) y el 23 divergirían: unos con superusuario, otros iterando tenants, otros exceptuando tablas de la política.
- **Corrección propuesta:** AD-2, Rule (añadir): "Tres roles PostgreSQL: `app_rw` (web y consumidores de dominio; RLS forzado, sin `BYPASSRLS`), `app_system` (solo `apps/worker`; sin `BYPASSRLS`; las tablas de infraestructura `outbox_events`, `processed_events`, `audit_cursors`, `idempotency_keys` y el esquema `pgboss` llevan `tenant_id` pero **no** política RLS y solo las tocan `packages/events|jobs`; todo job que lea tablas de negocio itera tenants y fija `SET LOCAL app.tenant_id` por iteración) y `app_migrator` (migraciones y carga de `standards-seed`; único con `INSERT/UPDATE` en el catálogo global; nunca en runtime). `MODE=migrate` ejecuta `prisma migrate deploy` y `seed:standards` con `app_migrator`."

### H-3 (high) — `ScoreSnapshot` sin propietario ni forma persistida compartida

- **Sección afectada:** AD-16, AD-3, mapa filas 10-11, 18-19, 20.
- **Problema:** AD-16 obliga a que toda puntuación sea un `ScoreSnapshot`, y la calculan al menos tres contextos en cambios distintos: Risk (residual, cambio 10), Objectives & KPIs (6 puntuaciones, cambio 18) y Reporting/AIMS (preparación, cambio 20). AD-3 exige un único propietario por tabla, pero DOMAIN_MAP §4 deja `ScoreSnapshot` "a confirmar en la arquitectura" y la espina no lo confirma. Resultado probable: tres tablas con formas distintas o tres escritores sobre la misma.
- **Corrección propuesta:** AD-16, Rule (añadir): "`ScoreSnapshot` es un tipo del kernel (`packages/kernel/scoring.ts`) con `score_type` en el enum canónico `ScoreType { requirement_coverage, control_implementation, control_effectiveness, evidence_exposure, risk_residual, readiness }` (addendum §C). La tabla `score_snapshots` pertenece a **Objectives & KPIs**; los demás contextos no la escriben: implementan `ScoreCalculator` (puerto público, registrado por `score_type`) y `domain-objectives-kpis` lo invoca, persiste y publica `SCORE_RECALCULATED`. El valor residual operativo vive en `RiskAssessment` (Risk); su desglose transparente es el `ScoreSnapshot` `risk_residual`."

### H-4 (high) — Proyección de búsqueda asignada a Reporting contra DOMAIN_MAP (Administration)

- **Sección afectada:** mapa fila 22; Deferred "Búsqueda semántica"; AD-3.
- **Problema:** la fila 22 sitúa la proyección de búsqueda en `domain-reporting [ASSUMPTION]`, pero DOMAIN_MAP §3.20 asigna `SearchDocument` a Administration, que además "consume todos los eventos para el registro de auditoría **y la indexación de búsqueda**". AD-3 obliga a respetar DOMAIN_MAP §4 como fuente de propiedad. El cambio 5 (Administration consume todo) y el cambio 22 construirían la proyección en sitios distintos. La fila Deferred difiere el motor semántico, no la propiedad, así que no cubre esta divergencia.
- **Corrección propuesta:** fila 22 → `search-traceability-navigation` | `apps/web` (paneles), **`domain-administration` (`search_documents`, consumidor íntegro `search-indexer`, mismo mecanismo de cursor que `audit-trail-writer`)**, puertos de lectura de cada contexto para trazabilidad | AD-3, AD-15, AD-5 (consumidores íntegros). Eliminar la etiqueta `[ASSUMPTION]` o, si se prefiere Reporting, corregir primero DOMAIN_MAP (jerarquía de CLAUDE.md).

### M-1 (medium) — Dimensión operativa silenciada: copias, DR y runbooks (NFR-008)

- **Sección afectada:** AD-20; Deferred.
- **Problema:** NFR-008 fija RPO ≤ 1 h, RTO ≤ 8 h, copias diarias cifradas en la región del tenant y prueba de restauración semestral. Ni un AD, ni una convención, ni una fila de Deferred lo gobierna. Las copias del bucket y de PostgreSQL deben ser coherentes entre sí (una fila `Evidence` sin objeto es evidencia perdida): decisión estructural, no de despliegue.
- **Corrección propuesta:** fila nueva en Deferred: "Proveedor de copias y DR (NFR-008: RPO ≤ 1 h, RTO ≤ 8 h) | El MVP corre en Docker; el contrato de consistencia ya se fija en AD-20 | Antes de `staging`, junto a Q6". Y AD-20, Rule (añadir): "PostgreSQL con PITR y copia diaria cifrada; el bucket con versionado de objetos y copia diaria en la misma región; la restauración se prueba en `staging` con un job de CI y su runbook vive en `docs/operations/RUNBOOKS.md` (NFR-022). La purga de un objeto de storage nunca precede a la de su fila."

### M-2 (medium) — NFR-001/NFR-004 sin AD: rate limiting, CSRF, cabeceras, tamaño máximo de archivo

- **Sección afectada:** AD-15, AD-13.
- **Problema:** NFR-001 exige rate limiting por usuario y tenant (100 req/min), protección CSRF, cabeceras seguras y escaneo ZAP en CI; NFR-004 fija ≤ 100 MB por archivo. CLAUDE.md lo lista como estándar de ingeniería. Ningún AD lo fija, así que cada route handler (cambios 2-24) podría resolverlo distinto o no resolverlo.
- **Corrección propuesta:** AD-15, Rule (añadir): "Un único `apps/web/middleware.ts` aplica: rate limit por `(tenant_id, user_id | api_key_id)` con ventana en PostgreSQL vía `packages/db` (sin Redis en el MVP `[ASSUMPTION]`) y respuesta 429 `problem+json` con `code = RATE_LIMITED`; CSRF por cookie `SameSite=Lax` **más** verificación de `Origin`/`Sec-Fetch-Site` en toda mutación de sesión; cabeceras (CSP, HSTS, `X-Content-Type-Options`, `Referrer-Policy`) definidas en `packages/kernel/security-headers.ts`." AD-13, Rule (añadir): "`ObjectStorage.ingest` rechaza archivos > 100 MB (`[ASSUMPTION NFR-004]`) y tipos fuera de la lista con `code = FILE_REJECTED` y evento `EVIDENCE_REJECTED`."

### M-3 (medium) — AD-4 no es exigible: la "secuencia fija" no tiene mecanismo

- **Sección afectada:** AD-4; AD-15 (`Idempotency-Key`).
- **Problema:** AD-3 se hace cumplir con `dependency-cruiser`; AD-4 solo enuncia `authorize → validate → transaction { mutate; append outbox }` sin nada que impida a un cambio omitir el `authorize` o el `append`. Tampoco se dice dónde se persisten las `Idempotency-Key` que AD-15 declara obligatorias (tabla y propietario).
- **Corrección propuesta:** AD-4, Rule (añadir): "Todo command se declara con `defineCommand({ permission, schema, handler })` de `packages/kernel`; el helper ejecuta la secuencia y es el único que recibe `withTenantTransaction`. `packages/db` **no exporta** el `PrismaClient` crudo: solo `withTenantTransaction(ctx, fn)` y los repositorios. La prueba de arquitectura falla ante un export de `src/commands/**` que no sea un `defineCommand` y ante cualquier import de `@prisma/client` fuera de `packages/db`." Convención nueva: "`Idempotency-Key` se persiste en `idempotency_keys(tenant_id, key, request_hash, response, expires_at)` propiedad de `packages/db` (infra), TTL 24 h `[ASSUMPTION]`."

### M-4 (medium) — Alcances de autorización de AD-8 no cubren al auditor externo (FR-093, cambio 15)

- **Sección afectada:** AD-8.
- **Problema:** los alcances `tenant | business_unit | owned_object` no expresan "solo lectura sobre los objetos vinculados a la auditoría X, con caducidad", que es lo que FR-093 y el cambio 15 exigen. El cambio 15 tendría que inventar un alcance o un rol ad hoc.
- **Corrección propuesta:** AD-8, Rule (añadir): "Cuarto alcance `audit_scoped(audit_id)`: concede permisos `*.read` solo sobre objetos alcanzables desde `Audit → AuditEvidence/AuditFinding → Evidence/Control/Requirement` de esa auditoría; se materializa como `Membership { role = external_auditor, scope_type = audit, scope_id, expires_at }` y el `Authorizer` lo evalúa por un puerto `AuditScopeReadPort` de Audit. Rol `external_auditor` incluido en la matriz por defecto (addendum §B, D-6)."

### M-5 (medium) — Entidades y tablas de AD-11 sin propietario o con nombre no canónico

- **Sección afectada:** AD-11; AD-3; DOMAIN_MAP §4.
- **Problema:** AD-11 cita `Audit report` (no es entidad; Audit tiene `AuditConclusion`) y `RiskMethodology` (no aparece en DOMAIN_MAP §3.5/§4 aunque FR-041 la exige). `object_versions` (snapshot por mutación de todas las entidades) no tiene propietario, en contra de AD-3. El cambio 10 y el cambio 15 podrían nombrar distinto lo mismo; el cambio 5 y cualquier otro podrían escribir `object_versions` cada uno a su manera.
- **Corrección propuesta:** AD-11, Rule (sustituir la lista): "`AIMSScope`, `Policy`, `Document`, `StatementOfApplicability`, `ImpactAssessment`, `RiskMethodology` (Risk), `AuditConclusion` (Audit)". Añadir: "`object_versions` pertenece a `packages/events` (infra, sin RLS de política pero con `tenant_id`) y lo escribe únicamente `withTenantTransaction` al confirmar un command; ningún contexto lo escribe ni lo lee salvo por `HistoryReadPort`." Propagar `RiskMethodology` a DOMAIN_MAP §3.5 y §4.

### M-6 (medium) — AD-14: emisor de `CREDENTIAL_*` indeterminado y L1 no puede emitir eventos

- **Sección afectada:** AD-14; AD-3; DOMAIN_MAP §5.
- **Problema:** DOMAIN_MAP §5 asigna `CREDENTIAL_CREATED|…|ACCESSED|REVOKED` a Integrations, pero AD-14 vincula también AI Assistance, Identity (secreto de cliente OIDC) y Notifications (SMTP). `packages/secrets` es L1 y, según la tabla de capas, solo depende de L0: no puede anexar al outbox. Sin regla, unos contextos emitirán el evento y otros supondrán que lo emite el adaptador.
- **Corrección propuesta:** AD-14, Rule (añadir): "`packages/secrets` nunca escribe en BD ni emite eventos; recibe `TenantContext` y devuelve el valor. El evento `CREDENTIAL_*` lo emite el **contexto propietario del `secret_ref`** (Integrations para conectores, Identity para OIDC, Notifications para SMTP, AI Assistance para LLM) con `aggregate_type = 'Credential'`, `payload.secret_ref` y `payload.purpose`. Actualizar DOMAIN_MAP §5 para reflejar los cuatro emisores."

### M-7 (medium) — Stack incompleto respecto a lo que los AD nombran

- **Sección afectada:** Stack; AD-3, AD-6, AD-16, AD-20, AD-22.
- **Problema:** los AD nombran herramientas que no tienen fila en Stack y por tanto no están "verificadas": `dependency-cruiser` (AD-3), motor de render HTML→PDF (AD-16: ¿Chromium de Playwright o `puppeteer`?), librería JSON canónico RFC 8785 (AD-6/AD-22), `mailpit`, `otel-collector` (AD-20), logger estructurado (AD-19). Dos cambios (1 y 19; 4 y 13) podrían elegir librerías distintas para el mismo hash canónico.
- **Corrección propuesta:** filas nuevas: `dependency-cruiser | <versión verificada>`; `Render PDF | Chromium de @playwright/test 1.63 reutilizado en apps/worker [ASSUMPTION]`; `JSON canónico RFC 8785 | <librería> (o implementación propia en packages/kernel/canonical-json.ts con vectores de prueba del RFC)`; `Mailpit | <versión>`; `OTel Collector (contrib) | <versión>`; `Logger | API de logs de @opentelemetry/sdk-logs <versión>`. Marcar cualquier fila no verificada con `[ASSUMPTION]` en lugar de omitirla.

### M-8 (medium) — Migraciones: NFR-019 no reflejado y una migración por cambio sin regla de reversibilidad

- **Sección afectada:** Consistency Conventions ("Commits y ramas"); AD-20.
- **Problema:** NFR-019 exige migraciones reversibles cuando sea práctico y aprobación humana para las irreversibles (también CLAUDE.md). La convención solo fija el nombre del directorio. Con esquema Prisma multi-archivo por contexto en `packages/db`, dos cambios que toquen tablas distintas generan migraciones que Prisma no fusiona automáticamente.
- **Corrección propuesta:** convención nueva "Migraciones": "una migración por cambio OpenSpec como máximo, generada al final del cambio; cada migración lleva `-- reversible: yes|no` y `-- down:` documentado en `docs/operations/`; `no` exige aprobación humana registrada en `design.md`; `prisma.config.ts` con `schemaFolder` apuntando a `packages/db/prisma/schema/<context>.prisma`; los índices de NFR-007 (`tenant_id, status, owner_id, *_id, created_at, due_date`) se declaran en el mismo archivo que la tabla."

### M-9 (medium) — Envelope de AD-5: `tenant_id` obligatorio no vale para eventos del catálogo global

- **Sección afectada:** AD-5, AD-7.
- **Problema:** `STANDARD_VERSION_PUBLISHED` y `REGULATORY_REQUIREMENT_CHANGED` (global) no tienen tenant; el envelope los exige. El cambio 6 tendría que inventar un tenant ficticio o violar el envelope; el `audit-trail-writer` (por tenant) no sabría bajo qué cadena registrarlos.
- **Corrección propuesta:** AD-5, Rule (añadir): "`tenant_id` es `NULL` solo cuando `aggregate_type` pertenece al catálogo global (AD-7). Los eventos globales se registran en la cadena del **tenant plataforma** (el mismo que inventaría la plataforma como `AISystem`, A9) y se abanican a todos los tenants suscritos por el relay como `events.<TYPE>` con `tenant_id` del suscriptor y `causation_id` del evento global."

### L-1 (low) — Compañeros desactualizados: existe ADR-016

- **Sección afectada:** front matter `companions`.
- **Problema:** se cita `docs/decisions/ADR-001..ADR-015`; en el repositorio existe `ADR-016-sdk-de-conectores.md`, que gobierna AD-18.
- **Corrección propuesta:** `docs/decisions/ADR-001..ADR-016` y referenciar ADR-016 en AD-18.

### L-2 (low) — Nombres no alineados con DOMAIN_MAP

- **Sección afectada:** AD-6, ERD, DOMAIN_MAP §3.18/§3.20.
- **Problema:** DOMAIN_MAP nombra `AuditLogEntry`; la espina usa `audit_entries` / `AUDIT_ENTRY` (entidad implícita `AuditEntry`). DOMAIN_MAP declara `Job` [A] como entidad de Workflow ("cola pg-boss"), pero la espina la hace infraestructura (`packages/jobs`), y AD-5 dice que pg-boss solo se usa por `JobScheduler`.
- **Corrección propuesta:** fijar en la espina "entidad `AuditEntry`, tabla `audit_entries`" y "pg-boss no es entidad de dominio; `Job` se elimina de Workflow", y propagar ambos a DOMAIN_MAP §3/§4 (jerarquía de CLAUDE.md).

### L-3 (low) — `SoAReadPort.isControlActive` no fija sobre qué versión de la SoA responde

- **Sección afectada:** AD-9; AD-16 (`ReportRun` guarda "versión de SoA").
- **Problema:** la SoA es versionada y aprobada (FR-063). Un consumidor podría leer el borrador en edición y otro la última aprobada. El cambio 13 (evidencia) y el 18 (métricas) divergirían en qué controles cuentan.
- **Corrección propuesta:** AD-9, Rule (añadir): "`SoAReadPort` responde siempre sobre la última `StatementOfApplicability` con `status = Approved`; expone además `currentApprovedSoaVersion(tenantId)` que `ReportRun` y `ScoreSnapshot` registran. Un borrador no activa ni desactiva nada hasta `SOA_APPROVED`."

### L-4 (low) — Cobertura F2 en el mapa y referencia externa "memlog"

- **Sección afectada:** mapa fila F2; Stack (encabezado).
- **Problema:** la fila F2 nombra conectores, IA asistida, lifecycle e incidentes, pero no importación/exportación (FR-013, PR-DR-016), calidad de datos (FR-014) ni feature flags, que DOMAIN_MAP asigna a Administration; sin fila, un futuro cambio podría ubicarlos en otro paquete. El encabezado de Stack remite a un "memlog" que no está en el repositorio ni en `companions`.
- **Corrección propuesta:** fila F2 (añadir): "`domain-administration` (ImportJob, ImportMapping, DataQualityCheck, FeatureFlag) | AD-3, AD-4, AD-21, PR-DR-016". Sustituir "memlog" por la ruta del registro de verificación (p. ej., `docs/decisions/ADR-001-stack-tecnologico.md §Verificación`) o eliminarlo.

## Lo que sí está bien resuelto (para no reabrirlo)

- Paradigma, capas y dirección de dependencias con mecanismo de CI (AD-1, AD-3).
- RLS forzado + prefijo de storage por tenant (AD-2), salvo el vacío de roles de sistema (H-2).
- Catálogo global inmutable frente a estado del tenant, con `SOURCE_DETAIL_REQUIRED` y cita a `resumenes/` (AD-7).
- SoA como único emisor de activación y prohibición de leer `soa_items` (AD-9), `Approval` único con separación de funciones (AD-10), enumeraciones únicas y prueba de términos prohibidos (AD-12), contrato de API y errores RFC 9457 (AD-15), puntuaciones e informes declarativos (AD-16), IA solo con `*.suggest` (AD-17), SDK de conectores y estados honestos (AD-18).
- Los tres diagramas Mermaid son válidos (`flowchart` con subgrafos y auto-arco punteado, `erDiagram` con etiquetas vacías permitidas); no hay placeholders ni comentarios de plantilla.

## Recuento

1 critical · 4 high · 9 medium · 4 low = 18 hallazgos.
