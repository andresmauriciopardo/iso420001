---
titulo: "Arquitectura de seguridad — Plataforma SaaS de gobernanza de IA ISO/IEC 42001"
fecha: 2026-09-17
estado: "borrador para revisión"
nivel: 3
inputs:
  - CLAUDE.md
  - docs/product/MASTER_ISO42001_PLATFORM_REQUIREMENTS.md (§4, §40-§43)
  - _bmad-output/planning-artifacts/prds/prd-ISO42001-2026-09-17/prd.md (FR-001..FR-014, NFR-001..NFR-005, NFR-017, §17)
  - _bmad-output/planning-artifacts/prds/prd-ISO42001-2026-09-17/addendum.md (§B, D.6)
  - docs/architecture/API_ARCHITECTURE.md
spine: _bmad-output/planning-artifacts/architecture/architecture-ISO42001-2026-09-17/ARCHITECTURE-SPINE.md
decisiones_relacionadas: [ADR-001, ADR-002, ADR-003, ADR-004, ADR-005, ADR-008, ADR-011, ADR-013, ADR-014]
---

# Arquitectura de seguridad

La plataforma se trata como un producto de seguridad empresarial (P2 §40): custodia decisiones de cumplimiento, evidencia y credenciales de terceros de muchos tenants. Este documento desarrolla las decisiones del spine que tocan seguridad (AD-2, AD-4, AD-6, AD-8, AD-11, AD-13, AD-14, AD-15, AD-17, AD-19, AD-23) y las convierte en controles verificables. **Toda decisión de seguridad que no está fijada literalmente en el spine se marca `[ASSUMPTION]` y queda pendiente de aprobación humana** (CLAUDE.md: "cambios de arquitectura de seguridad"); se recogen en §14.

Principios: mínimo privilegio; defensa en profundidad (la BD aísla aunque el código falle); nada de confianza en el cliente; todo cambio de estado atribuible e inmutable; secretos fuera del modelo de datos; la IA sugiere y una persona autorizada aprueba.

## 1. Autenticación (AD-15, ADR-011; FR-003, FR-005)

**Proveedor.** better-auth 1.7 `[ASSUMPTION, pendiente de aprobación humana: arquitectura de seguridad]` integrado en `apps/web`, con almacenamiento en PostgreSQL mediante el adaptador de Prisma. Tablas de autenticación (`users`, `sessions`, `accounts`, `verifications`, `two_factor`) son tablas de plataforma con `tenant_id` allí donde el dato pertenece a un tenant (`memberships`, `invitations`, `api_keys`) y RLS como el resto (AD-2); el usuario global (`users`) puede pertenecer a varios tenants vía `Membership` (FR-001).

**Métodos en el MVP.**

- Cuenta local: correo + contraseña + **TOTP MFA** (RFC 6238, 6 dígitos, ventana ±1 paso `[ASSUMPTION]`). Contraseñas con `argon2id` `[ASSUMPTION]`, política configurable con mínimo 12 caracteres (FR-003), comprobación contra listas de contraseñas comprometidas `[ASSUMPTION]`. Códigos de recuperación de un solo uso, almacenados hasheados.
- **OIDC genérico** (Authorization Code + PKCE) contra el proveedor de identidad del tenant, configurado por el `Organization Administrator`; `client_secret` almacenado solo como `secret_ref` (AD-14). El vínculo cuenta externa ↔ usuario se hace por `sub` + `iss`, nunca solo por correo `[ASSUMPTION]`.
- **SAML diferido** (spine, tabla Deferred): la arquitectura es OIDC-ready y SAML llegará como adaptador cuando lo exija un piloto.

**MFA obligatoria.** Configurable por tenant y por rol; por defecto obligatoria para `Executive`, `Organization Administrator` y `Platform Administrator` `[ASSUMPTION D-7]`. Una sesión sin segundo factor de un rol que lo exige solo puede completar el enrolamiento de MFA; cualquier otra ruta responde `401 MFA_REQUIRED`. Cuando el tenant delega en OIDC, la MFA se acepta del IdP si la aserción trae `amr` con un factor fuerte `[ASSUMPTION]`; si no, se exige TOTP local.

**Sesiones.** Cookie `httpOnly; Secure; SameSite=Lax; Path=/` con identificador opaco; el registro en BD guarda `user_id`, `active_tenant_id`, `mfa_verified`, `ip`, `user_agent`, `last_seen_at`, `expires_at`. Caducidad absoluta 12 h y de inactividad 60 min `[ASSUMPTION]`. Cada petición valida la sesión contra la BD (sin JWT sin estado en la UI `[ASSUMPTION]`), por lo que la desactivación (FR-005) revoca sesiones y claves de API personales en ≤ 60 s.

**Invitaciones y ciclo de vida.** `Invitation { tenant_id, email, roles[], token_hash, expires_at, invited_by }` con caducidad de 7 días (FR-005); token enviado por correo y almacenado hasheado; aceptar exige vincular la cuenta y, si el rol lo requiere, enrolar MFA antes de la primera sesión completa. Eventos: `USER_INVITED`, `USER_ACTIVATED`, `USER_DEACTIVATED`, `USER_PERMISSION_CHANGED` (motivo obligatorio, FR-004). Ningún tenant queda sin `Organization Administrator` activo (invariante de `deactivateUser`).

**Endpoints de autenticación.** Rate limiting por IP (§7), bloqueo progresivo tras 10 intentos fallidos `[ASSUMPTION]`, errores uniformes que no revelan si el correo existe, verificación de correo obligatoria.

**API keys.** Definidas en `API_ARCHITECTURE.md` §3: prefijo visible, `SHA-256` del valor en BD, `scopes ⊆` permisos del creador, caducidad opcional, rotación con solapamiento, eventos `API_KEY_*` `[ASSUMPTION]`.

## 2. Autorización: RBAC, alcance y nivel de objeto (AD-8, AD-17; FR-004)

El catálogo de permisos `<resource>.<action>` vive en `packages/kernel/permissions.ts`. Cada *command* y *query* declara su permiso; el `Authorizer` lo evalúa contra las asignaciones de rol del actor en el tenant activo con tres alcances:

| Alcance | Significado | Fase |
| --- | --- | --- |
| `tenant` | El permiso vale sobre cualquier objeto del tenant | MVP |
| `business_unit` | Solo sobre objetos cuya `business_unit_id` esté en la asignación del rol | F2 (FR-004) |
| `owned_object` `(p)` | Solo si `owner_id` (o `created_by`, según la entidad) es el actor | MVP |

**Autorización a nivel de objeto (BOLA).** Ningún servicio autoriza por recurso y luego carga el objeto: el patrón es `repo.findById(ctx, id)` (tenant fijado por RLS) → si no existe o no pertenece al tenant → `NOT_FOUND` → `authorizer.check(permission, object)`. La propiedad `(p)` se comprueba con el objeto ya cargado. Las referencias cruzadas en el cuerpo de una petición (p. ej., `control_implementation_id` al enlazar evidencia) se resuelven también por repositorio con tenant fijado; una referencia que no resuelve produce `TENANT_MISMATCH` internamente, se registra como evento de seguridad y se responde como `NOT_FOUND`.

**Matriz rol × permiso por defecto** (addendum §B, D-6; `[ASSUMPTION]`, **pendiente de aprobación humana**). Leyenda: **X** concedido; **(p)** solo sobre objetos propios; **(F2)** en Fase 2; vacío = no concedido. PA Platform Administrator · OA Organization Administrator · GM AI Governance Manager · RM Risk Manager · CM Compliance Manager · AU Auditor · SO AI System Owner · CO Control Owner · EO Evidence Owner · RV Reviewer · EX Executive · RO Read Only.

| Permiso | PA | OA | GM | RM | CM | AU | SO | CO | EO | RV | EX | RO |
|---|---|---|---|---|---|---|---|---|---|---|---|---|
| organization.read | X | X | X | X | X | X | X | X | X | X | X | X |
| organization.manage | X | X | | | | | | | | | | |
| users.read | X | X | X | | X | | | | | | | |
| users.manage | X | X | | | | | | | | | | |
| roles.manage | X | X | | | | | | | | | | |
| standards.read | X | X | X | X | X | X | X | X | X | X | X | X |
| standards.manage (catálogo global y semilla) | X | | | | | | | | | | | |
| requirements.read | X | X | X | X | X | X | X | X | X | X | X | X |
| requirements.manage | | | X | | X | | | | | | | |
| controls.read | X | X | X | X | X | X | X | X | X | X | X | X |
| controls.manage | | | X | | X | | | (p) | | | | |
| controls.activate / controls.deactivate | | | X | | X | | | | | | | |
| soa.read | X | X | X | X | X | X | X | X | X | X | X | X |
| soa.manage | | | X | | X | | | | | | | |
| soa.approve | | | X | | X | | | | | | | |
| risks.read | X | X | X | X | X | X | X | X | X | X | X | X |
| risks.manage | | | X | X | | | (p) | | | | | |
| risks.approve (plan de tratamiento) | | | | X | | | | | | | X | |
| risks.accept (riesgo residual) | | | | X | | | | | | | X | |
| assets.read / ai_systems.read | X | X | X | X | X | X | X | X | X | X | X | X |
| assets.manage / ai_systems.manage | | | X | | | | (p) | | | | | |
| assessments.read | X | X | X | X | X | X | X | X | X | X | X | X |
| assessments.manage | | | X | X | | | (p) | | | | | |
| assessments.approve | | | X | X | | | | | | | | |
| evidence.read | X | X | X | X | X | X | X | X | X | X | X | X (paquete) |
| evidence.upload | | | X | | X | | X | X | X | | | |
| evidence.approve | | | X | | X | | | | | X | | |
| evidence.delete (archivar; borrado gobernado F2) | | X | | | X | | | | | | | |
| audits.read | X | X | X | X | X | X | X | X | X | X | X | X |
| audits.manage (programa) | | | | | X | X | | | | | | |
| audits.execute | | | | | | X | | | | | | |
| reports.read | X | X | X | X | X | X | X | X | X | X | X | X |
| reports.manage | | X | X | | X | | | | | | | |
| integrations.read | X | X | X | | X | | | | | | | |
| integrations.manage / integrations.sync | | X | X | | | | | | | | | |
| policies.read | X | X | X | X | X | X | X | X | X | X | X | X |
| policies.manage | | | X | | X | | | | | | | |
| policies.generate (F2) | | | X | | X | | | | | | | |
| policies.approve | | | | | | | | | | | X | |
| management_review.read | X | X | X | X | X | X | | | | | X | X |
| management_review.manage | | | X | | | | | | | | X | |
| management_review.approve | | | | | | | | | | | X | |

**Reglas transversales** (addendum §B), implementadas como invariantes del `Authorizer` y del *command* `assignRoles`, no como convención:

1. `Platform Administrator` administra tenants y catálogo global pero **no** ve datos de negocio de los tenants por defecto. El acceso de soporte exige un `SupportAccessGrant { tenant_id, granted_by (OA), platform_admin_id, scope, reason, expires_at }` `[ASSUMPTION: entidad nueva; pendiente de aprobación humana]` aceptado por un `Organization Administrator`, con caducidad ≤ 72 h `[ASSUMPTION]`; cada lectura bajo el grant emite `SUPPORT_ACCESS_USED` `[ASSUMPTION]` al registro de auditoría del tenant. Técnicamente, el PA no tiene `Membership` en el tenant; el grant crea una membresía temporal de solo lectura que el job `support-access-expirer` revoca.
2. `Auditor` no puede tener a la vez `controls.manage`, `evidence.upload` ni `evidence.approve` sobre el alcance que audita (independencia). `assignRoles` rechaza la combinación con `INVARIANT_VIOLATION`.
3. `Reviewer` nunca aprueba evidencia que subió (PR-DR-011): `reviewEvidence` rechaza `actor_id = uploaded_by` con `SEGREGATION_OF_DUTIES`.
4. `Read Only` invitado como auditor externo ve solo paquete de evidencia, SoA e informes, con caducidad (FR-093): su `Membership` lleva `expires_at` y `evidence.read` se restringe al `EvidencePackage` compartido.
5. Las aprobaciones de alcance, política y revisión por la dirección recaen en `Executive` (5.1-5.3, 9.3); las de plan de tratamiento y aceptación de riesgo en `Risk Manager` y `Executive` (6.1.3).

Además, toda aprobación pasa por `Approval` con `decided_by ≠ requested_by` y `decided_by ≠ created_by` (AD-10); la `SegregationOfDutiesException` solo se admite cuando el tenant carece de otro usuario con el permiso y queda registrada con evento propio (pendiente de aprobación humana, addendum D.4).

**Principal `ai-assistant`** (AD-17): posee exclusivamente permisos `*.suggest`; una prueba de `tests/security` recorre el catálogo y verifica que no puede escribir estados de requisito, control, riesgo, evidencia ni CAPA. **Principal `system`** (jobs): actúa con permisos de sistema pero siempre con `tenant_id` fijado por el job; no existe un modo "todos los tenants" salvo en `standards.manage` (catálogo global, sin `tenant_id`).

**Roles personalizados** (FR-004): el tenant puede crear roles a partir del catálogo; el catálogo en sí es inmutable por tenant. Cambiar roles exige `reason` y emite `USER_PERMISSION_CHANGED`.

## 3. Aislamiento de tenant (AD-2, AD-13, ADR-002; FR-002)

Capas, de dentro hacia fuera:

1. **PostgreSQL RLS.** Toda tabla de negocio tiene `tenant_id uuid NOT NULL`, `ENABLE ROW LEVEL SECURITY` y `FORCE ROW LEVEL SECURITY`, con política `USING (tenant_id = current_setting('app.tenant_id', true)::uuid)` y `WITH CHECK` idéntico. Si `app.tenant_id` no está fijado, `current_setting(…, true)` devuelve `NULL` y la política no deja pasar ninguna fila: el fallo seguro es "cero filas", nunca "todas". El rol de conexión de la aplicación (`app_rw`) no tiene `BYPASSRLS`, no es propietario de las tablas y no puede alterar políticas; las migraciones corren con un rol distinto (`app_migrator`) solo en `prisma migrate deploy` (AD-20).
2. **`SET LOCAL` por transacción.** `db.withTenant(ctx, fn)` es el único punto de entrada al cliente Prisma para tablas de negocio; ejecuta `SET LOCAL app.tenant_id` dentro de la transacción, de modo que el valor muere con ella y no contamina conexiones del pool. Un `lint` de arquitectura prohíbe `prisma.$transaction` fuera de `packages/db`.
3. **Tablas globales de solo lectura.** El catálogo normativo (AD-7) no lleva `tenant_id`; `app_rw` solo tiene `SELECT`; escribe el loader de `standards-seed` con `app_migrator` en despliegue. `outbox_events`, `processed_events` y `audit_entries` llevan `tenant_id` y RLS; las cargas útiles de pg-boss incluyen `tenant_id` para que el consumidor fije el contexto antes de tocar dominio.
4. **Almacenamiento de objetos.** Claves `tenants/<tenant_id>/<domain>/<object_id>/<version>` (AD-13) construidas por el puerto `ObjectStorage` a partir de `ctx`; ningún llamador pasa una clave arbitraria. URLs firmadas solo tras `authorize('evidence.read', evidence)`, caducidad ≤ 15 min y firma sobre la clave completa. Un bucket por entorno, sin bucket por tenant en el MVP `[ASSUMPTION]`.
5. **Búsqueda e informes heredan.** La proyección de búsqueda y los `QueryModel` de Reporting son tablas y vistas con `tenant_id` y RLS (AD-16); ningún camino de lectura evita `withTenant`.
6. **Prohibido en el MVP**: esquema-por-tenant y base-por-tenant (AD-2).

**Pruebas cross-tenant automáticas** (AD-23, NFR-005, FR-002), en `tests/security/cross-tenant.spec.ts`: dos tenants sembrados (`alpha`, `beta`); con credenciales válidas de `alpha` se invoca cada endpoint de lista, detalle, búsqueda, exportación y descarga con identificadores de `beta`; resultado esperado `0` filas o `404`, nunca `403` ni datos. Una prueba de BD conecta con `app_rw` sin `SET LOCAL` y espera `0` filas en cada tabla de negocio; otra recorre `pg_class` y falla si alguna tabla con `tenant_id` carece de `relrowsecurity` y `relforcerowsecurity`. Las pruebas se generan desde el registro OpenAPI, así que un endpoint nuevo entra automáticamente en la batería.

## 4. Gestión de secretos (AD-14, ADR-014; FR-116)

El puerto `SecretStore` en `packages/secrets`:

```ts
interface SecretStore {
  create(ctx, value: string, meta: { purpose: string; owner_type: string; owner_id: string }): Promise<SecretRef>;
  read(ctx, ref: SecretRef): Promise<string>;           // emite CREDENTIAL_ACCESSED
  rotate(ctx, ref: SecretRef, value: string): Promise<SecretRef>;
  revoke(ctx, ref: SecretRef): Promise<void>;
}
```

La BD almacena solo `secret_ref` (`sec_<uuid>`) con `purpose`, `owner`, `created_at`, `rotated_at`, `status`. El valor vive en el adaptador: en `local` y `ci`, `env-encrypted` lo cifra con AES-256-GCM `[ASSUMPTION]` bajo `SECRETS_MASTER_KEY` (validada al arrancar en `kernel/config.ts`); en `staging` y `production`, KMS/Vault (F2, spine Deferred; proveedor pendiente de decisión humana). Toda lectura emite `CREDENTIAL_ACCESSED`; toda escritura `CREDENTIAL_CREATED | UPDATED | ROTATED | REVOKED`, consumidos por el registro de auditoría. DTOs, *server components* y logs nunca contienen el valor: `SecretRef` es un tipo nominal y el serializador rechaza cualquier propiedad marcada `@secret` en su esquema Zod `[ASSUMPTION mecanismo]`. Los secretos de la propia plataforma (conexión a BD, claves S3, firma de cookies, `SECRETS_MASTER_KEY`) llegan por variables de entorno del despliegue; `.env*` está en `.gitignore` y el escaneo de secretos (§10) bloquea el commit.

## 5. Cifrado

- **En tránsito**: TLS 1.2+ obligatorio (NFR-001); HSTS (§6); conexiones a PostgreSQL y a S3 con TLS y verificación de certificado en `staging`/`production`; en `local` se permite texto plano dentro de `docker-compose`.
- **En reposo**: cifrado del proveedor gestionado para PostgreSQL, almacenamiento de objetos y copias de seguridad (proveedor pendiente de decisión humana, AD-20 / Q6).
- **Columnas sensibles con clave por tenant** `[ASSUMPTION, pendiente de aprobación humana]`: cifrado de sobre con `tenant_keys { tenant_id, wrapped_dek, kek_ref, version }`, DEK envuelta por la clave maestra del `SecretStore`; columnas cifradas en aplicación con AES-256-GCM y AAD = `tenant_id || table || column`. Candidatas: secretos TOTP, IP y `user_agent` de sesiones, campos de configuración de conector marcados sensibles que no son credenciales. No se cifran campos que indexan, filtran o participan en el hash de auditoría (§9). La rotación de DEK es un job de Administration con evento.
- **Hashes, no cifrado, para credenciales verificables**: contraseñas (`argon2id`), tokens de invitación, API keys y tokens de sesión (`SHA-256`).

## 6. Cabeceras HTTP y CSRF (NFR-001)

Cabeceras emitidas por el `middleware` de `apps/web` en toda respuesta `[ASSUMPTION valores]`:

| Cabecera | Valor |
| --- | --- |
| `Content-Security-Policy` | `default-src 'self'; script-src 'self' 'nonce-<req>'; style-src 'self' 'nonce-<req>'; img-src 'self' data: blob:; font-src 'self'; connect-src 'self' <otel-endpoint>; frame-ancestors 'none'; base-uri 'self'; form-action 'self'; object-src 'none'; upgrade-insecure-requests` |
| `Strict-Transport-Security` | `max-age=63072000; includeSubDomains; preload` (solo `staging`/`production`) |
| `X-Content-Type-Options` | `nosniff` |
| `Referrer-Policy` | `strict-origin-when-cross-origin` |
| `Permissions-Policy` | `camera=(), microphone=(), geolocation=(), payment=(), usb=()` |
| `X-Frame-Options` | `DENY` (redundante con `frame-ancestors`, para clientes antiguos) |
| `Cross-Origin-Opener-Policy` | `same-origin` |
| `Cache-Control` en `/api/v1/*` y descargas | `no-store` |

La CSP usa *nonces* por petición generados en el `middleware`, sin `'unsafe-inline'` ni `'unsafe-eval'`; OWASP ZAP baseline (NFR-001) la verifica en CI. Las descargas se sirven con `Content-Disposition: attachment` para impedir la ejecución de HTML o SVG subidos como evidencia.

**CSRF.** `SameSite=Lax` bloquea envíos cross-site de formularios; además, toda mutación desde el navegador exige `Origin` (o `Sec-Fetch-Site: same-origin`) coincidente con el host permitido, y las *Server Actions* de Next.js traen su propia comprobación. Las peticiones con `Authorization: Bearer` no llevan cookie y no son vulnerables a CSRF.

## 7. Rate limiting (NFR-001)

Definido en `API_ARCHITECTURE.md` §11 con límites indicativos `[ASSUMPTION]`: por IP en endpoints de autenticación e invitaciones (30/min), por sesión (100/min por usuario y tenant), por API key (600/min), por tenant agregado (3.000/min) y por subidas (20/10 min). Contadores en tabla `UNLOGGED` de PostgreSQL (sin Redis en el stack). Las respuestas `429 RATE_LIMITED` llevan `Retry-After`. Los intentos fallidos de autenticación tienen un cubo propio con bloqueo progresivo (§1). El worker limita por concurrencia de cola (pg-boss), no por peticiones.

## 8. Seguridad de archivos (AD-13, ADR-008; NFR-004)

1. **Lista MIME permitida** `[ASSUMPTION]`: `application/pdf`, `image/png`, `image/jpeg`, `text/plain`, `text/csv`, `application/json`, `application/vnd.openxmlformats-officedocument.wordprocessingml.document`, `application/vnd.openxmlformats-officedocument.spreadsheetml.sheet`, `application/vnd.openxmlformats-officedocument.presentationml.presentation`, `application/zip` (solo como paquete de evidencia exportado por la plataforma). Se verifica por *magic bytes* además de la extensión y el `Content-Type` declarado; discordancia → `415 UNSUPPORTED_MEDIA_TYPE`. Formatos con macros (`.docm`, `.xlsm`, `.pptm`) y ejecutables quedan fuera.
2. **Tamaño máximo** 100 MB por archivo `[ASSUMPTION]`, aplicado en el borde (`Content-Length`) y durante el streaming (corte al superar el límite).
3. **SHA-256 en servidor**: el handler transmite el cuerpo al `ObjectStorage` calculando el hash al vuelo; el `sha256` se guarda en `EvidenceFile` y se devuelve al cliente para que pueda verificarlo; los hashes enviados por el cliente se ignoran.
4. **Escaneo antivirus asíncrono** (ClamAV 1.4, `[ASSUMPTION]`): el archivo entra con `scan_status = Pending`; el job `evidence-scan` lo analiza en el worker; `Clean` permite la transición a `Collected`; `Infected` mueve el objeto a cuarentena (`tenants/<tenant_id>/quarantine/…`), marca la evidencia `Rejected` con motivo y emite `EVIDENCE_FILE_REJECTED` `[ASSUMPTION evento]`; la UI muestra el estado honesto y nunca ofrece descarga de un archivo no escaneado.
5. **URLs firmadas** ≤ 15 min, ligadas a clave completa y verificadas por permiso; sin objetos públicos.
6. **Sin macros ni renderizado activo**: vista previa solo de PDF e imágenes en `iframe sandbox`; los documentos ofimáticos se descargan, no se renderizan en el MVP `[ASSUMPTION]`.
7. **Procesamiento en worker aislado**: la generación de PDF (Chromium headless, AD-16) y la extracción de texto corren solo en `apps/worker`, contenedor sin privilegios, sistema de archivos de solo lectura salvo `/tmp` y egress limitado a PostgreSQL, S3 y ClamAV `[ASSUMPTION]`.

Todo archivo rechazado por tipo, tamaño o malware queda registrado con motivo y nunca se almacena como evidencia (NFR-004).

## 9. Registro de auditoría con cadena de hashes (AD-6, ADR-005; FR-007, NFR-017)

**Tabla `audit_entries`** (Administration, único escritor):

| Campo | Tipo | Contenido |
| --- | --- | --- |
| `id` | uuid v7 | Identificador |
| `tenant_id` | uuid | Cadena independiente por tenant |
| `sequence` | bigint | Monótono por tenant; `UNIQUE (tenant_id, sequence)` |
| `event_id` | uuid | `outbox_events.id` origen; `UNIQUE` (idempotencia) |
| `occurred_at` | timestamptz | Del evento de dominio |
| `recorded_at` | timestamptz | Momento de inserción |
| `actor_id`, `actor_type` | uuid, enum | Quién |
| `action` | text | `UPPER_SNAKE` del evento (`CONTROL_APPLICABILITY_CHANGED`) |
| `object_type`, `object_id` | text, uuid | Qué |
| `previous_state`, `new_state` | jsonb | Antes y después |
| `reason` | text | Motivo aportado por el actor |
| `correlation_id`, `causation_id` | uuid | Trazabilidad (AD-19) |
| `session_metadata` | jsonb | `ip`, `user_agent`, `session_id`, `auth_method` |
| `previous_hash` | bytea(32) | Hash del eslabón anterior |
| `hash` | bytea(32) | Hash de este eslabón |

**Algoritmo.** `hash_n = SHA-256(previous_hash_{n-1} || canonical_json(entry_n))`, donde `canonical_json` es JSON canónico RFC 8785 de todos los campos salvo `hash`, `previous_hash` y `recorded_at`; el primer eslabón de cada tenant usa `previous_hash = SHA-256(tenant_id)`. El cálculo está en `packages/domain-administration/src/audit/hash-chain.ts` y es determinista: tipos `timestamptz` serializados en ISO 8601 UTC con milisegundos, UUID en minúsculas, `jsonb` canonizado.

**Serialización por tenant.** El consumidor `audit-trail-writer` se ejecuta con clave singleton pg-boss `audit-writer:<tenant_id>`, de modo que nunca hay dos escrituras concurrentes para el mismo tenant; dentro de la transacción lee el último eslabón (`SELECT … FOR UPDATE`), calcula `sequence + 1` y el hash, inserta y registra `processed_events`. Los eventos de un tenant llegan en orden de `outbox_events.id` (uuid v7, monótono).

**Inmutabilidad.** Triggers `BEFORE UPDATE OR DELETE` en `audit_entries` que lanzan excepción; `app_rw` solo tiene `INSERT` y `SELECT`; ninguna migración concede más. La prueba `tests/security/audit-immutability.spec.ts` intenta `UPDATE`, `DELETE` y `TRUNCATE` con `app_rw` y espera error; otra verifica que no existe ruta de API que lo haga.

**Verificación.** `POST /api/v1/audit-trail/verify` (job `audit-chain-verify`): recorre el tenant por `sequence`, recalcula cada hash y compara `previous_hash` con el `hash` anterior; resultado `intact` o `broken` con `first_broken_sequence`. Se programa además una verificación diaria por tenant y el resultado queda como evento `AUDIT_CHAIN_VERIFIED` `[ASSUMPTION evento]`.

**Exportación.** JSON y CSV con todos los campos y `sequence`, `previous_hash`, `hash` en hexadecimal, más `manifest.json` con el último hash y el `sha256` del export; es un `ReportRun` (AD-16) con permiso `organization.read`. El ancla externa diaria queda para F2 (spine Deferred).

**Si la verificación falla.** Un eslabón roto implica manipulación con privilegios de BD, restauración parcial o defecto de serialización: siempre un incidente de seguridad. Procedimiento: (1) ningún eslabón se repara ni reescribe; (2) se emite `AUDIT_CHAIN_BROKEN { tenant_id, first_broken_sequence }` `[ASSUMPTION evento]` con notificación inmediata al `Organization Administrator` y al `Platform Administrator`; (3) la UI muestra el estado `broken` y la posición; (4) se abre un `Incident` de categoría `security` con la última exportación y las copias de seguridad como material forense; (5) la cadena solo continúa tras aprobación humana, con una entrada `AUDIT_CHAIN_RESUMED` `[ASSUMPTION]` que documenta la discontinuidad. La plataforma nunca declara íntegro lo que no ha verificado.

## 10. Cadena de suministro (NFR-003)

- **Dependencias**: Renovate `[ASSUMPTION]` (o Dependabot) con agrupación semanal y `lockfile` pnpm fijado; `pnpm audit` y OSV-Scanner en CI; vulnerabilidad alta o crítica sin excepción documentada bloquea el pipeline.
- **SAST**: CodeQL para TypeScript `[ASSUMPTION]` en cada PR, más `eslint-plugin-security` y `dependency-cruiser`.
- **Escaneo de secretos**: `gitleaks` `[ASSUMPTION]` en *pre-commit* y en CI sobre la PR.
- **SBOM**: CycloneDX por build de imagen, adjunto al release; imágenes firmadas con cosign `[ASSUMPTION]`.
- **Licencias**: lista permitida (MIT, Apache-2.0, BSD, ISC, MPL-2.0); toda dependencia copyleft (GPL, AGPL, LGPL) o de tercero no auditado **requiere aprobación humana** (CLAUDE.md, NFR-003); `license-checker` en CI falla fuera de la lista. ClamAV (GPL-2.0) corre como servicio separado por red, sin enlazarse al código; SeaweedFS es Apache-2.0.
- **GitHub Actions** fijadas por SHA, permisos mínimos por workflow, sin secretos de producción en CI (AD-20).

## 11. Modelo de amenazas resumido (STRIDE por superficie)

S suplantación · T manipulación · R repudio · I revelación · D denegación · E elevación.

| Superficie | Amenaza (STRIDE) | Mitigación | Cubierta por |
| --- | --- | --- | --- |
| UI web | S/T: robo de sesión, CSRF, XSS, clickjacking | Cookie `httpOnly Secure SameSite=Lax`, MFA en roles críticos, revocación ≤ 60 s; verificación de `Origin`, CSP con nonces, `frame-ancestors 'none'` | AD-15, §1, §6 |
| UI web | E: permisos verificados solo en cliente | El `Authorizer` decide en el servicio; la UI solo oculta | AD-8 |
| API pública | S/E/I: API key filtrada, BOLA, enumeración | Hash en BD, scopes ⊆ creador, rotación; carga por repositorio con RLS, `404` sin filtración, pruebas BOLA generadas desde OpenAPI | AD-2, AD-8, AD-15 |
| API pública | T/R: sobrescritura ciega, replays, mutación sin rastro | `If-Match`/`version`, `Idempotency-Key`; outbox en la misma transacción y `audit_entries` con hash | AD-4, AD-6, AD-11 |
| API pública | D: abuso, cuerpos enormes | Rate limiting por IP, sesión, API key y tenant; límites de tamaño | §7, NFR-001 |
| Worker / jobs | E/T: job sobre otro tenant, entrega duplicada | `tenant_id` obligatorio en la carga útil y `withTenant`; `processed_events`, consumidores idempotentes, singleton por tenant para auditoría | AD-2, AD-5, AD-6 |
| Worker / jobs | D: cola envenenada | Reintentos exponenciales (máx. 5), `dead_letter`, alerta al tercer fallo | AD-5, FR-012 |
| Conectores | I/T: credenciales expuestas; conector que escribe dominio o finge estado | `secret_ref` + `SecretStore` con eventos `CREDENTIAL_*`; pipeline fijo del SDK, estados `mock | configured | validated` | AD-14, AD-18 |
| Conectores | S: SSRF hacia red interna | Lista de hosts de proveedor por adaptador; sin URL libre en el MVP `[ASSUMPTION]`; egress del worker restringido | §8, AD-18 |
| Almacenamiento de objetos | I/T: objeto ajeno o sin permiso; malware | Prefijo `tenants/<tenant_id>/`, URL firmada ≤ 15 min tras `authorize`; MIME por *magic bytes*, ClamAV antes de `Collected`, `sha256` en servidor | AD-13, §3, §8 |
| LLM (F2) | T/E/I: inyección de prompt; fuga de evidencia al proveedor | Principal `ai-assistant` solo `*.suggest`, artefactos `generated_by_ai` con revisión humana; puerto `LLMProvider` con campos permitidos y `LLMUsageRecord` | AD-17, AD-14 |
| Semilla normativa | T: catálogo alterado o con texto inventado | Tablas globales de solo lectura para `app_rw`, carga solo por `standards-seed` semver, pruebas de dominio (38 controles, cero texto literal, `SOURCE_DETAIL_REQUIRED`) | AD-7, AD-23 |

## 12. Pruebas de seguridad exigidas (AD-23, NFR-005)

**Por cada cambio OpenSpec** (bloqueantes del pipeline), en `tests/security/`:

| Prueba | Qué comprueba |
| --- | --- |
| `cross-tenant.spec.ts` | Endpoints con ids de otro tenant → `0` filas o `404`; conexión sin `SET LOCAL` → `0` filas; ninguna tabla con `tenant_id` sin RLS forzado |
| `privilege-escalation.spec.ts` | Cada rol por defecto intenta cada permiso no concedido por la matriz (§2) → `403`; nadie se asigna roles ni amplía scopes de su API key; `Auditor` con permisos incompatibles es rechazado |
| `bola.spec.ts` | Permisos `(p)` sobre objetos de otro propietario → `403`; referencias cruzadas a objetos ajenos → `404` |
| `secret-exposure.spec.ts` | Ninguna respuesta, HTML ni log contiene secretos, `key_hash`, tokens de sesión ni `SECRETS_MASTER_KEY`; los DTOs de `Integration` exponen solo `secret_ref` |
| `evidence-access.spec.ts` | Evidencia sin `evidence.read` → `404`; URL firmada expirada rechazada; archivo no escaneado no descargable; `Reviewer` no aprueba su propia evidencia |
| `audit-immutability.spec.ts` | `UPDATE`/`DELETE`/`TRUNCATE` sobre `audit_entries` fallan; la verificación devuelve `intact` tras el E2E y `broken` ante una manipulación simulada con superusuario |
| `ai-assistant-readonly.spec.ts` | `ai-assistant` no puede cambiar estado de requisito, control, riesgo, evidencia ni CAPA |

Cada `PR-DR-nnn` de seguridad o segregación (PR-DR-011, PR-DR-017) tiene su caso unitario nombrado (AD-23); la prueba de i18n rechaza las cadenas prohibidas (PR-DR-009).

**En `mvp-hardening`** (cambio 24), además: **DAST** con OWASP ZAP (baseline y escaneo autenticado) contra el contenedor en CI sin hallazgos altos; **revisión de dependencias** completa (SBOM, licencias, versiones fijadas, dependencias sin uso); revisión de configuración (CSP efectiva, cookies, `no-store`, privilegios de BD, buckets sin acceso público); y **pentest externo recomendado** `[ASSUMPTION]` antes del primer piloto con datos reales, con alcance autenticación, aislamiento de tenant, API pública, subida de archivos y registro de auditoría; sus hallazgos se gestionan como `Finding` con CAPA.

## 13. Registro y monitorización de seguridad (AD-19)

Logs estructurados OpenTelemetry con `tenant_id, actor_id, correlation_id, event_type`, sin secretos ni contenido de evidencia. Generan alerta operativa `[ASSUMPTION umbrales]`: `TENANT_MISMATCH`, ráfagas de `401`/`403`/`404` de un mismo principal, `RATE_LIMITED` sostenido, `EVIDENCE_FILE_REJECTED`, `AUDIT_CHAIN_BROKEN`, uso de `SupportAccessGrant` y cambios de rol.

## 14. Decisiones pendientes de aprobación humana

| # | Decisión propuesta | Sección | Categoría |
| --- | --- | --- | --- |
| S-1 | better-auth 1.7 como proveedor de autenticación; `argon2id`; verificación de correo obligatoria | §1 | Arquitectura de seguridad (ADR-011) |
| S-2 | MFA obligatoria por defecto para `Executive`, `Organization Administrator`, `Platform Administrator`; aceptación de MFA del IdP vía `amr` | §1 | Umbral operativo (PRD D-7) |
| S-3 | Caducidades de sesión (12 h absoluta, 60 min inactividad), bloqueo tras 10 intentos, invitación 7 días | §1 | Umbral operativo |
| S-4 | Formato y `SHA-256` de API keys; rotación con solapamiento; eventos `API_KEY_*` | §1 | Credenciales |
| S-5 | Matriz rol × permiso por defecto del addendum §B y sus 5 reglas transversales | §2 | PRD D-6 |
| S-6 | `SupportAccessGrant` para acceso de soporte del `Platform Administrator` (consentimiento, ≤ 72 h, evento `SUPPORT_ACCESS_USED`) | §2 | Entidad y evento nuevos |
| S-7 | `SegregationOfDutiesException` cuando no hay segundo usuario con permiso | §2 | Addendum D.4 |
| S-8 | Un bucket por entorno con prefijo por tenant; rol `app_migrator` separado para migraciones y semilla | §3 | Aislamiento |
| S-9 | Adaptador `env-encrypted` AES-256-GCM con `SECRETS_MASTER_KEY`; KMS/Vault en F2 | §4 | Gestor de secretos (ADR-014) |
| S-10 | Cifrado de columnas sensibles con DEK por tenant (cifrado de sobre) y lista de columnas candidatas | §5 | Arquitectura de seguridad |
| S-11 | Valores de CSP, HSTS, `Permissions-Policy` y verificación de `Origin` para CSRF | §6 | Configuración de seguridad |
| S-12 | Límites de rate limiting y almacenamiento de contadores en PostgreSQL | §7 | Umbral operativo |
| S-13 | Lista MIME permitida, 100 MB, ClamAV asíncrono con cuarentena, evento `EVIDENCE_FILE_REJECTED`, worker aislado sin egress libre | §8 | Seguridad de archivos (ADR-008) |
| S-14 | Verificación diaria de la cadena, eventos `AUDIT_CHAIN_VERIFIED | BROKEN | RESUMED` y procedimiento ante rotura | §9 | Registro de auditoría (ADR-005) |
| S-15 | Herramientas de cadena de suministro (Renovate, CodeQL, gitleaks, CycloneDX, cosign) y lista de licencias permitidas | §10 | Cadena de suministro (NFR-003) |
| S-16 | Restricción de hosts de conector (sin URL libre) en MVP | §11 | Anti-SSRF |
| S-17 | Pentest externo antes del primer piloto con datos reales | §12 | Recomendación |
| S-18 | Umbrales de alerta de seguridad | §13 | Operación |
