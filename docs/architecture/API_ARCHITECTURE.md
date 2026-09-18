---
titulo: "Arquitectura de API — Plataforma SaaS de gobernanza de IA ISO/IEC 42001"
fecha: 2026-09-17
estado: "borrador para revisión"
nivel: 3
inputs:
  - CLAUDE.md
  - docs/product/MASTER_ISO42001_PLATFORM_REQUIREMENTS.md (§4, §40-§43)
  - _bmad-output/planning-artifacts/prds/prd-ISO42001-2026-09-17/prd.md (FR-001..FR-014, NFR-001..NFR-005, NFR-017, §17)
  - _bmad-output/planning-artifacts/prds/prd-ISO42001-2026-09-17/addendum.md (§B, §C, D.6)
spine: _bmad-output/planning-artifacts/architecture/architecture-ISO42001-2026-09-17/ARCHITECTURE-SPINE.md
decisiones_relacionadas: [ADR-001, ADR-002, ADR-003, ADR-004, ADR-005, ADR-011, ADR-013, ADR-014]
---

# Arquitectura de API

Este documento desarrolla el contrato de API fijado por el spine (AD-15) y las decisiones que lo sostienen: mutación por servicios de aplicación con outbox (AD-4), entrega de eventos (AD-5), auditoría append-only (AD-6), autorización por permiso declarado (AD-8), versionado y concurrencia (AD-11), identidad y formato (AD-22). No introduce decisiones nuevas; toda inferencia lleva `[ASSUMPTION]` y, si toca seguridad, queda **pendiente de aprobación humana**. Cubre FR-011 (API documentada por dominio), FR-002 (aislamiento), FR-004 (autorización a nivel de objeto), FR-007 (huella de auditoría idéntica por UI y API) y NFR-001/NFR-002.

## 1. Estilo y principios (AD-1, AD-15)

La API es HTTP/JSON orientada a recursos, servida por **Route Handlers de Next.js 16 App Router** en `apps/web/app/api/v1/<domain>/…`. Cada handler es una capa de entrega sin lógica de negocio (AD-1): valida la forma de la petición con **Zod 4**, resuelve el `TenantContext`, invoca un *command* o *query* del paquete `packages/domain-<context>` correspondiente y traduce el `Result` a HTTP. Principios que se aplican a todo endpoint:

1. **Un contrato, no veintisiete.** Envelope de lista, errores, paginación, filtros, orden y cabeceras son idénticos en todos los dominios (AD-15).
2. **Permiso declarado, verificado en el servicio.** El handler declara el permiso para documentación y pre-comprobación; la autoridad es el `Authorizer` dentro del servicio de aplicación (AD-8).
3. **Toda mutación deja rastro.** No existe endpoint mutante que no escriba un evento en el outbox dentro de la misma transacción (AD-4); la entrada en `audit_entries` la produce el consumidor `audit-trail-writer` (AD-6). UI y API comparten el mismo servicio, así que la huella de auditoría es la misma (FR-011).
4. **Esquema primero.** Los esquemas Zod de entrada y salida son la única definición del contrato; OpenAPI se genera de ellos, nunca a mano.
5. **Aislamiento heredado.** El handler no filtra por tenant; lo hace la BD por RLS a partir de `SET LOCAL app.tenant_id` (AD-2). Un handler que "olvida" el tenant devuelve cero filas, no datos ajenos.

`[ASSUMPTION]` El prefijo `/api/v1` sustituye al prefijo `/api/` de P2 §43 para permitir coexistencia de versiones; la lista de recursos es la de P2 §43 más los transversales del PRD (`/audit-trail`, `/approvals`, `/notifications`, `/health`).

## 2. Mapa de recursos y estructura de carpetas (AD-15, AD-22)

Rutas en kebab-case y plural (AD-22). Cada recurso pertenece a un único bounded context propietario (AD-3); el handler solo importa el `index.ts` público de ese paquete.

| Prefijo | Contexto propietario | Fase |
| --- | --- | --- |
| `/organizations`, `/users`, `/roles` | Identity & Organization | MVP |
| `/standards`, `/requirements`, `/controls` (catálogo global, lectura) | Standards & Compliance | MVP |
| `/soa` | Controls & SoA | MVP |
| `/ai-systems`, `/assets`, `/models`, `/datasets`, `/providers` | AI Portfolio | MVP |
| `/risks` | Risk | MVP |
| `/impact-assessments` | Impact | MVP |
| `/lifecycle` | Lifecycle | F2 |
| `/evidence` | Evidence | MVP |
| `/policies` | Documents & Policies | MVP |
| `/audits`, `/findings` | Audit | MVP |
| `/capa` | CAPA | MVP |
| `/incidents` | Incidents | F2 |
| `/objectives`, `/kpis` | Objectives & KPIs | MVP |
| `/reports` | Reporting | MVP |
| `/integrations` | Integrations | MVP (mock), F2 (reales) |
| `/tasks`, `/approvals` | Workflow | MVP |
| `/management-reviews` | Management Review | MVP |
| `/notifications` | Notifications | MVP |
| `/audit-trail` | Administration | MVP |
| `/health`, `/openapi.json` | apps/web (composición) | MVP |

Estructura de carpetas en `apps/web/app/api/v1`:

```text
apps/web/app/api/v1/
  _lib/                                  # entrega HTTP, sin dominio (AD-1)
    define-route.ts                      # fábrica: auth → rate limit → tenant → Zod → servicio → respuesta
    problem.ts                           # serializador application/problem+json (RFC 9457)
    resolve-tenant-context.ts            # cookie de sesión | Bearer api_key → TenantContext
    idempotency.ts                       # almacén Idempotency-Key (tabla idempotency_keys)
    preconditions.ts                     # If-Match / ETag ↔ version
    pagination.ts                        # cursor opaco, filter[], sort
    rate-limit.ts                        # cubos por sesión, api_key e IP
    openapi-registry.ts                  # registro zod-openapi de todas las rutas
  openapi.json/route.ts
  health/route.ts
  organizations/route.ts
  organizations/[organizationId]/route.ts
  soa/route.ts                           # GET lista, POST crea SoA (Idempotency-Key)
  soa/[soaId]/route.ts                   # GET, PATCH (If-Match)
  soa/[soaId]/items/route.ts
  soa/[soaId]/items/[itemId]/route.ts
  soa/[soaId]/items/[itemId]/applicability/route.ts   # POST transición
  soa/[soaId]/approval/route.ts          # POST solicita aprobación (Approval, AD-10)
  evidence/route.ts
  evidence/[evidenceId]/route.ts
  evidence/[evidenceId]/files/route.ts   # POST multipart en streaming
  evidence/[evidenceId]/download-url/route.ts  # POST → URL firmada ≤ 15 min
  evidence/[evidenceId]/review/route.ts  # POST Accepted | Rejected
  audit-trail/route.ts                   # GET consulta paginada
  audit-trail/verify/route.ts            # POST lanza verificación (job)
  audit-trail/export/route.ts            # POST exportación JSON/CSV (job)
  …                                      # un directorio por recurso de la tabla anterior
```

Regla de forma: un `route.ts` exporta solo funciones `GET|POST|PATCH|DELETE` construidas con `defineRoute(...)`; cualquier `import` de `@prisma/client` o de internals de un paquete de dominio dentro de `apps/web/app/api` falla la prueba de arquitectura (AD-3, AD-4). Los subrecursos de transición (`/applicability`, `/review`, `/approval`) se modelan como `POST` sobre el recurso, no como `PATCH` de un campo, porque cada transición es un *command* con máquina de estados explícita (convención "Estado y transiciones" del spine).

## 3. Autenticación y resolución de `TenantContext` (AD-15, AD-2, AD-14)

Dos mecanismos, ambos resueltos al mismo `TenantContext` antes de tocar el dominio:

**Sesión (UI y llamadas del navegador).** Cookie `httpOnly; Secure; SameSite=Lax` emitida por **better-auth 1.7** `[ASSUMPTION, ADR-011, pendiente de aprobación humana]`. La sesión contiene `user_id`, `active_tenant_id` (membresía activa elegida en el selector de organización, FR-001), `mfa_verified` y `session_id`. El tenant activo se cambia con un *command* explícito (`switchActiveTenant`) que valida `Membership` activa; nunca se acepta `tenant_id` desde cuerpo, query o cabecera de la petición.

**API key (integraciones).** `Authorization: Bearer <api_key>`. La clave tiene la forma `aims_<env>_<prefix>_<secret>` `[ASSUMPTION]`: `prefix` (8 caracteres) se guarda en claro para identificar la clave en UI y logs; el valor completo se genera con 256 bits de entropía y solo se muestra una vez; en BD se almacena `key_hash = SHA-256(api_key)` `[ASSUMPTION: hash rápido suficiente por la entropía de la clave; pendiente de aprobación humana]`. Cada `ApiKey` pertenece a un único tenant, a un principal creador y lleva `scopes[]` = subconjunto de los permisos efectivos de quien la creó (nunca puede excederlos), `expires_at` opcional, `last_used_at` y `status ∈ {Active, Revoked}`. **Rotación**: crear la clave sucesora, ventana de solapamiento de 24 h `[ASSUMPTION]`, revocar la anterior. Toda creación, rotación y revocación emite `API_KEY_CREATED | API_KEY_ROTATED | API_KEY_REVOKED` `[ASSUMPTION: eventos nuevos de Identity & Organization; pendiente de aprobación humana]` y queda en el registro de auditoría. La desactivación de un usuario revoca sus claves personales (FR-005).

`TenantContext` (definido en `packages/kernel`):

```ts
type TenantContext = {
  tenant_id: string;                 // uuid v7
  actor_id: string;                  // user_id | api_key_id | 'system' | 'ai-assistant'
  actor_type: 'user' | 'api_key' | 'system' | 'ai_assistant';
  permissions: ReadonlySet<Permission>;   // resueltas: roles del tenant ∩ scopes de la api_key
  business_unit_ids: string[];       // alcance business_unit (F2)
  locale: 'es' | 'en';
  correlation_id: string;            // generado en el borde (AD-19)
  request: { ip: string; user_agent: string; session_id?: string; auth_method: 'session' | 'api_key'; mfa_verified: boolean };
};
```

El adaptador de BD expone `db.withTenant(ctx, fn)`, que abre la transacción, ejecuta `SET LOCAL app.tenant_id = ctx.tenant_id` y corre `fn`. El rol de conexión no tiene `BYPASSRLS` (AD-2). Las peticiones sin credenciales válidas reciben `401 UNAUTHENTICATED`; las de un principal cuyo tenant activo no coincide con el recurso reciben `404` (nunca `403`, para no filtrar existencia; FR-002).

## 4. Autorización por permiso declarado (AD-8)

Cada endpoint declara exactamente un permiso del catálogo `packages/kernel/permissions.ts` (`<resource>.<action>`, P2 §4 más los añadidos del addendum §B). La declaración en la ruta alimenta OpenAPI (extensión `x-permission`) y un pre-chequeo barato; el servicio de aplicación vuelve a evaluar `authorize(permission, object)` con el alcance `tenant | business_unit | owned_object` y la propiedad `(p)`, cargando el objeto por repositorio con tenant fijado. Una prueba de arquitectura comprueba que el permiso declarado por la ruta coincide con el del *command* que invoca `[ASSUMPTION herramienta]`.

Ejemplo de tabla endpoint → permiso (extracto; la tabla completa se genera desde el registro OpenAPI):

| Método y ruta | Permiso | Alcance | Notas |
| --- | --- | --- | --- |
| `GET /soa` | `soa.read` | tenant | Todos los roles por defecto |
| `POST /soa` | `soa.manage` | tenant | `Idempotency-Key` obligatorio |
| `GET /soa/{soaId}` | `soa.read` | tenant | Devuelve `ETag` = `version` |
| `PATCH /soa/{soaId}` | `soa.manage` | tenant | `If-Match` obligatorio |
| `GET /soa/{soaId}/items` | `soa.read` | tenant | Incluye `Not Applicable` (visible y auditable, AD-9) |
| `POST /soa/{soaId}/items/{itemId}/applicability` | `soa.manage` | tenant | Transición; PR-DR-003 |
| `POST /soa/{soaId}/approval` | `soa.approve` | tenant | Crea `Approval` (AD-10); `decided_by ≠ requested_by` |
| `GET /evidence` | `evidence.read` | tenant / paquete para `Read Only` | Auditor externo ve solo el paquete (regla B.4) |
| `POST /evidence` | `evidence.upload` | tenant o `(p)` | `Idempotency-Key` obligatorio |
| `POST /evidence/{evidenceId}/files` | `evidence.upload` | owned_object | Multipart en streaming; SHA-256 en servidor (AD-13) |
| `POST /evidence/{evidenceId}/download-url` | `evidence.read` | tenant | URL firmada ≤ 15 min ligada al `tenant_id` del objeto |
| `POST /evidence/{evidenceId}/review` | `evidence.approve` | tenant | Rechaza si `actor_id = uploaded_by` (PR-DR-011) |
| `DELETE /evidence/{evidenceId}` | `evidence.delete` | tenant | Archiva (`deleted_at`); nunca purga (AD-11, AD-21) |

Reglas de respuesta de autorización: permiso ausente → `403 PERMISSION_DENIED` **solo** cuando el objeto pertenece al tenant del actor; objeto de otro tenant o inexistente → `404 NOT_FOUND`. El principal `ai-assistant` solo posee permisos `*.suggest` (AD-17); una prueba de la suite `tests/security` verifica que no puede invocar ningún endpoint mutante de requisito, control, riesgo, evidencia ni CAPA.

## 5. Convenciones de petición y respuesta (AD-15)

**Formato.** `Content-Type: application/json; charset=utf-8` en cuerpos; `snake_case` en propiedades JSON (coincide con columnas, AD-22); fechas en ISO 8601 UTC (`timestamptz`) y fechas de negocio `YYYY-MM-DD`; identificadores UUID v7 generados en servidor (el cliente nunca propone `id`).

**Paginación por cursor.** Toda lista responde `{ items: T[], next_cursor: string | null, total?: number }`. El cursor es opaco (base64url de la clave de orden más `id`, firmado con HMAC para impedir manipulación `[ASSUMPTION]`). Parámetros: `limit` (por defecto 25, máximo 200 `[ASSUMPTION]`) y `cursor`. `total` solo se calcula con `?include=total` porque es costoso bajo RLS `[ASSUMPTION]`.

**Filtros.** `?filter[field]=value` para igualdad; operadores `?filter[field][op]=value` con `op ∈ {eq, ne, in, gte, lte, contains}` `[ASSUMPTION]`. Solo se aceptan campos y operadores declarados en el esquema Zod de la lista; cualquier otro → `400 VALIDATION_FAILED`. Los valores de enumeración se filtran por el valor canónico en inglés (AD-12), p. ej. `filter[applicability]=Not%20Applicable`.

**Orden.** `?sort=-created_at,name`: prefijo `-` descendente; campos permitidos por esquema; orden estable añadiendo `id` como desempate.

**Lectura de una entidad.** Cabecera `ETag: "<version>"` (entero de concurrencia optimista, AD-11) y `Last-Modified`. Las entidades exponen los campos comunes `id, tenant_id, created_at, created_by, updated_at, updated_by, status, version, deleted_at`.

**Límites de tamaño** `[ASSUMPTION]`: cuerpo JSON ≤ 1 MB; archivo ≤ 100 MB (NFR-004); listas con `filter[id][in]` ≤ 200 valores; exportaciones grandes son asíncronas (`ReportRun`/job) y se consultan por `GET /reports/runs/{runId}` con estados `Queued | Running | Ready | Failed` (FR-012).

**Terminología prohibida.** Ningún DTO ni mensaje contiene `certified`, `certificado` ni `compliance score` (PR-DR-009); la prueba de i18n del spine (AD-12) recorre también los esquemas de respuesta.

## 6. Errores: `application/problem+json` (RFC 9457)

Toda respuesta de error usa `Content-Type: application/problem+json` con:

```json
{
  "type": "https://docs.<host>/problems/<code-kebab>",
  "title": "Texto corto en el idioma del actor",
  "status": 422,
  "detail": "Explicación accionable; para invariantes incluye el PR-DR-nnn",
  "code": "INVARIANT_VIOLATION",
  "correlation_id": "0192f3a1-…",
  "errors": [{ "path": ["justification"], "code": "REQUIRED", "message": "…" }]
}
```

`code` es el código canónico de `packages/kernel/errors.ts`. Códigos fijados por el spine y su mapeo HTTP `[ASSUMPTION: mapeo]`:

| `code` | HTTP | Cuándo |
| --- | --- | --- |
| `UNAUTHENTICATED` | 401 | Sin sesión ni API key válida |
| `MFA_REQUIRED` | 401 | Rol con MFA obligatoria sin segundo factor verificado |
| `PERMISSION_DENIED` | 403 | Permiso ausente sobre objeto del propio tenant |
| `NOT_FOUND` | 404 | Inexistente o de otro tenant (sin filtración de existencia) |
| `TENANT_MISMATCH` | 404 | Detectado en servicio: referencia cruzada a objeto de otro tenant; se registra como incidente de seguridad, se responde como `NOT_FOUND` |
| `VALIDATION_FAILED` | 400 | Esquema Zod rechaza la entrada; `errors[]` con rutas |
| `IDEMPOTENCY_KEY_REQUIRED` | 400 | POST creador sin `Idempotency-Key` |
| `IDEMPOTENCY_KEY_REUSED` | 409 | Misma clave con cuerpo distinto |
| `PRECONDITION_REQUIRED` | 428 | PATCH/PUT o POST de transición sin `If-Match` |
| `CONFLICT_VERSION` | 412 | `If-Match` distinto de `version` actual |
| `INVARIANT_VIOLATION` | 422 | Regla de dominio (`PR-DR-nnn` en `detail`) o transición inválida |
| `SEGREGATION_OF_DUTIES` | 422 | `decided_by = requested_by` u otro conflicto AD-10 sin excepción registrada |
| `SOURCE_DETAIL_REQUIRED` | 422 | Registro normativo sin cita a `resumenes/NN` (AD-7) |
| `LEGAL_HOLD_ACTIVE` | 423 | Archivado o expiración sobre objeto con `LegalHold` (AD-21) |
| `PAYLOAD_TOO_LARGE` | 413 | Límites de §5 |
| `UNSUPPORTED_MEDIA_TYPE` | 415 | MIME fuera de la lista permitida (AD-13) |
| `RATE_LIMITED` | 429 | Cubo agotado; incluye `Retry-After` |
| `INTERNAL_ERROR` | 500 | Nunca expone stack ni SQL; solo `correlation_id` |

Los códigos no listados en el spine (`UNAUTHENTICATED`, `MFA_REQUIRED`, `NOT_FOUND`, `VALIDATION_FAILED`, `IDEMPOTENCY_*`, `PRECONDITION_REQUIRED`, `SEGREGATION_OF_DUTIES`, `LEGAL_HOLD_ACTIVE`, `PAYLOAD_TOO_LARGE`, `UNSUPPORTED_MEDIA_TYPE`, `RATE_LIMITED`, `INTERNAL_ERROR`) son `[ASSUMPTION]` como ampliación del catálogo de `kernel/errors.ts`. Un `Result` del dominio se mapea a problem+json en `_lib/problem.ts`; ningún handler construye errores a mano.

## 7. Idempotencia y concurrencia (AD-11, AD-15)

**Idempotencia.** Todo `POST` que crea un recurso exige `Idempotency-Key` (UUID o cadena ≤ 128 caracteres). El almacén `idempotency_keys (tenant_id, actor_id, key, request_hash, response_status, response_body, created_at)` es una tabla de negocio con RLS; la clave se reserva dentro de la misma transacción del *command* para que dos peticiones concurrentes idénticas no creen dos recursos. Una repetición con el mismo `request_hash` devuelve la respuesta original (`200/201` con cabecera `Idempotent-Replayed: true`); con cuerpo distinto → `409 IDEMPOTENCY_KEY_REUSED`. Retención de claves: 24 h `[ASSUMPTION]`. Los `POST` de transición pueden enviar `Idempotency-Key` y se honra igual; su protección principal es `If-Match`.

**Concurrencia.** `PATCH`, `PUT` y los `POST` de transición exigen `If-Match: "<version>"` `[ASSUMPTION: extensión de AD-11 a POST de transición]`; el servicio ejecuta `UPDATE … WHERE id = $1 AND version = $2` incrementando `version`; cero filas → `412 CONFLICT_VERSION` con la `version` vigente en `errors[0]`. La respuesta de una mutación exitosa incluye el nuevo `ETag`.

## 8. Mutaciones, outbox y auditoría automática (AD-4, AD-5, AD-6)

La secuencia de toda petición mutante es fija y la implementa `defineRoute`. El handler nunca decide qué evento emitir: eso lo hace el *command*, que escribe al menos un evento del catálogo o `ENTITY_MUTATED { entity_type, entity_id, before, after }` en `outbox_events` **dentro de la misma transacción** que la mutación. El relay del worker mueve el outbox a colas pg-boss `events.<TYPE>`; el consumidor `audit-trail-writer` (Administration) serializa por tenant y añade la entrada con hash encadenado (AD-6). Los metadatos de sesión que AD-6 exige en `audit_entries` (IP, `user_agent`, `session_id`, `auth_method`) viajan en `payload.actor_metadata` del envelope `[ASSUMPTION: clave reservada del payload; el envelope AD-5 no cambia]`.

```mermaid
sequenceDiagram
  autonumber
  participant C as Cliente (UI o integración)
  participant R as Route Handler (apps/web/app/api/v1)
  participant L as _lib (auth, rate limit, tenant, Zod)
  participant S as Servicio de aplicación (packages/domain-controls-soa)
  participant DB as PostgreSQL (RLS, outbox_events)
  participant W as apps/worker (relay + pg-boss)
  participant A as audit-trail-writer (domain-administration)

  C->>R: POST /api/v1/soa/{soaId}/items/{itemId}/applicability<br/>Cookie | Bearer, If-Match, Idempotency-Key?
  R->>L: resolveTenantContext(request)
  L-->>R: TenantContext | 401 UNAUTHENTICATED
  R->>L: rateLimit(ctx) → 429 RATE_LIMITED si agotado
  R->>L: parse(params, body) con Zod → 400 VALIDATION_FAILED
  R->>S: changeApplicability(ctx, params, body, ifMatch)
  S->>S: authorize('soa.manage', soaItem) → 403 | 404
  S->>S: validate(command) + máquina de estados → 422 INVARIANT_VIOLATION (PR-DR-003)
  S->>DB: BEGIN; SET LOCAL app.tenant_id
  S->>DB: UPDATE soa_items … WHERE id AND version = ifMatch → 412 CONFLICT_VERSION si 0 filas
  S->>DB: INSERT object_versions (snapshot)
  S->>DB: INSERT outbox_events (CONTROL_APPLICABILITY_CHANGED, CONTROL_DEACTIVATED)
  S->>DB: COMMIT
  S-->>R: Result.ok(SoAItem v+1)
  R-->>C: 200 application/json, ETag: "v+1"
  W->>DB: relay: outbox_events → cola events.CONTROL_APPLICABILITY_CHANGED
  W->>A: entrega (singleton por tenant)
  A->>DB: INSERT audit_entries (hash = SHA-256(previous_hash || canonical_json(entry)))
  A->>DB: INSERT processed_events (consumer_name, event_id)
```

Garantías derivadas: (a) si la transacción falla no hay evento ni auditoría huérfanos; (b) si el worker cae, el evento espera en `outbox_events` y la entrada de auditoría aparece al reanudarse (objetivo ≤ 5 s en operación normal, FR-007); (c) la misma acción por UI produce la misma entrada porque comparte el *command*.

## 9. Versionado y deprecación (AD-15)

La versión mayor va en la ruta (`/api/v1`). Cambios aditivos (campos opcionales nuevos, valores de enumeración nuevos, endpoints nuevos) no cambian la versión; los consumidores deben tolerar campos desconocidos. Un cambio incompatible crea `/api/v2` y ambas coexisten. La retirada se anuncia con cabeceras `Deprecation: true`, `Sunset: <HTTP-date>` y `Link: <…/v2/…>; rel="successor-version"` en cada respuesta de la ruta deprecada, con un mínimo de 6 meses entre anuncio y `Sunset` `[ASSUMPTION]`. Las enumeraciones canónicas (AD-12) y las versiones de evento (AD-5) evolucionan por sus propias reglas, independientes de la versión de API.

## 10. OpenAPI generado desde Zod

Cada esquema se registra con **zod-openapi 6** `[ASSUMPTION]` a través de `_lib/openapi-registry.ts`; `defineRoute` añade la operación con `operationId`, permiso (`x-permission`), cabeceras exigidas (`Idempotency-Key`, `If-Match`) y las respuestas problem+json por código. El documento se sirve en `GET /api/v1/openapi.json` (público, sin datos de tenant `[ASSUMPTION]`) y un visor interactivo en `/api/v1/docs` solo para principales autenticados `[ASSUMPTION]`. En CI, un paso genera `openapi.json` y falla el build si difiere del comprometido en `docs/api/openapi.v1.json` `[ASSUMPTION]`, de modo que el contrato publicado nunca se desvía del código. Los ejemplos del documento se toman de los fixtures de dominio derivados de `resumenes/` (identificadores 4.1-10.2 y A.2.2-A.10.4, sin texto literal).

## 11. Rate limiting y protección de disponibilidad (NFR-001)

Límites indicativos `[ASSUMPTION, pendiente de aprobación humana: arquitectura de seguridad]`:

| Sujeto | Límite | Ventana | Ámbito |
| --- | --- | --- | --- |
| Sesión de usuario | 100 peticiones | 1 min | por usuario y tenant (valor inicial NFR-001) |
| API key | 600 peticiones | 1 min | por clave; configurable por tenant |
| IP sin autenticar | 30 peticiones | 1 min | endpoints de autenticación e invitaciones |
| Subidas de archivo | 20 archivos | 10 min | por usuario |
| Tenant (agregado) | 3.000 peticiones | 1 min | protección de vecinos ruidosos |

Los contadores viven en la tabla `UNLOGGED rate_limit_buckets` de PostgreSQL `[ASSUMPTION: no hay Redis en el stack]`. Respuestas con `RateLimit-Limit`, `RateLimit-Remaining`, `RateLimit-Reset` y, al agotar, `429 RATE_LIMITED` con `Retry-After`. El worker no está sometido a estos límites; su protección es la concurrencia por cola de pg-boss.

## 12. Ejemplo completo: `POST /api/v1/soa/{soaId}/items/{itemId}/applicability`

Transición de aplicabilidad de un control del Anexo A dentro de una SoA (AD-9, PR-DR-003). El esquema Zod es **estructural**: no duplica la invariante de dominio, para que el error sea siempre `INVARIANT_VIOLATION` procedente de la máquina de estados y no un `VALIDATION_FAILED` distinto según la capa.

```ts
// packages/domain-controls-soa/src/commands/change-applicability.schema.ts
import { z } from 'zod';
import { SoaApplicability } from '@aims/kernel/enums';

export const ChangeApplicabilityParams = z.object({
  soaId: z.uuid(),
  itemId: z.uuid(),
});

export const ChangeApplicabilityBody = z.object({
  applicability: z.enum(SoaApplicability),           // 'Applicable' | 'Not Applicable' | 'Applicable with Alternative Control' | 'Pending Assessment'
  justification: z.string().trim().max(4000).optional(),
  alternative_control_description: z.string().trim().max(4000).optional(),
  reason: z.string().trim().min(1).max(2000),        // motivo de auditoría (AD-6)
});

export const SoaItemResponse = z.object({
  id: z.uuid(),
  soa_id: z.uuid(),
  control_id: z.uuid(),                              // catalog_id (AD-7)
  standard_version_id: z.uuid(),
  applicability: z.enum(SoaApplicability),
  justification: z.string().nullable(),
  status: z.string(),
  version: z.number().int(),
  updated_at: z.iso.datetime(),
  updated_by: z.uuid(),
});
```

```ts
// apps/web/app/api/v1/soa/[soaId]/items/[itemId]/applicability/route.ts
import { defineRoute } from '@/app/api/v1/_lib/define-route';
import { changeApplicability, ChangeApplicabilityParams, ChangeApplicabilityBody, SoaItemResponse } from '@aims/domain-controls-soa';

export const POST = defineRoute({
  operationId: 'soa.changeItemApplicability',
  permission: 'soa.manage',
  params: ChangeApplicabilityParams,
  body: ChangeApplicabilityBody,
  response: SoaItemResponse,
  preconditions: { ifMatch: 'required' },
  idempotency: 'optional',
  handler: ({ ctx, params, body, ifMatch }) => changeApplicability(ctx, { ...params, ...body, expected_version: ifMatch }),
});
```

Secuencia dentro de `changeApplicability` (servicio de aplicación, AD-4):

1. `authorize('soa.manage', { type: 'SoAItem', id: itemId })`: carga el `SoAItem` por repositorio con tenant fijado; inexistente o de otro tenant → `NOT_FOUND`; sin permiso → `PERMISSION_DENIED`. Rechaza si la SoA está `Approved` y no se ha abierto una versión nueva (`INVARIANT_VIOLATION`).
2. `validate`: máquina de estados `packages/domain-controls-soa/src/state/soa-item.ts`. Si `applicability = 'Not Applicable'` y `justification` está vacía → `INVARIANT_VIOLATION` con `PR-DR-003`. Si `applicability = 'Applicable with Alternative Control'` sin `alternative_control_description` → `INVARIANT_VIOLATION` con `PR-DR-003` `[ASSUMPTION: la misma regla de justificación]`.
3. `transaction`: `SET LOCAL app.tenant_id`; `UPDATE soa_items … WHERE id AND version = expected_version` (→ `CONFLICT_VERSION`); snapshot en `object_versions`; `INSERT outbox_events` con `CONTROL_APPLICABILITY_CHANGED { soa_id, control_id, previous_applicability, new_applicability, justification, reason }` y, cuando el control pasa de activo a `Not Applicable`, además `CONTROL_DEACTIVATED` (AD-9); si pasa a `Applicable`, `CONTROL_ACTIVATED`.
4. Respuesta `200` con `SoAItemResponse` y `ETag: "<version+1>"`.

Petición de ejemplo:

```http
POST /api/v1/soa/0192f3a1-4c2e-7b31-9a4d-0e1a2b3c4d5e/items/0192f3a1-4c2e-7b31-9a4d-0e1a2b3c4d99/applicability HTTP/1.1
Authorization: Bearer aims_live_k7Qm2Xp9_…
Content-Type: application/json
If-Match: "3"
Idempotency-Key: 5f1c0e7e-2b9a-4d8e-9a3f-1c2d3e4f5a6b

{ "applicability": "Not Applicable", "reason": "La organización no entrena modelos; rol declarado: cliente de IA (contexto 4.1)" }
```

Respuesta (falta `justification`):

```http
HTTP/1.1 422 Unprocessable Content
Content-Type: application/problem+json
Content-Language: es

{
  "type": "https://docs.<host>/problems/invariant-violation",
  "title": "Invariante de dominio incumplida",
  "status": 422,
  "detail": "PR-DR-003: la transición a Not Applicable exige una justificación documentada no vacía. El control permanece en su aplicabilidad anterior.",
  "code": "INVARIANT_VIOLATION",
  "invariant": "PR-DR-003",
  "correlation_id": "0192f3a2-9d0e-7f11-8c2b-3a4b5c6d7e8f",
  "errors": [
    { "path": ["justification"], "code": "REQUIRED_FOR_NOT_APPLICABLE", "message": "Obligatoria cuando applicability = Not Applicable" }
  ]
}
```

La misma petición con `justification` no vacía responde `200`, y en ≤ 5 s el registro de auditoría del tenant contiene una entrada `CONTROL_APPLICABILITY_CHANGED` con actor, motivo, estado anterior (`Applicable`) y nuevo (`Not Applicable`), consultable en `GET /api/v1/audit-trail?filter[object_id]=<itemId>`.

## 13. Endpoints transversales (AD-6, AD-10, AD-19)

- `GET /audit-trail` (`organization.read`): paginado, filtros por `actor_id`, `action`, `object_type`, `object_id`, rango de fechas; devuelve `hash` y `previous_hash` de cada entrada. `POST /audit-trail/verify` lanza el job de verificación y devuelve `202` con `job_id`; `GET /audit-trail/verify/{jobId}` → `{ status, result: 'intact' | 'broken', first_broken_sequence? }`. `POST /audit-trail/export` genera JSON o CSV con hashes (job).
- `GET|POST /approvals` (Workflow): lista pendientes del actor; `POST /approvals/{approvalId}/decision` con `decision ∈ {Granted, Rejected}` y `reason`; rechaza `SEGREGATION_OF_DUTIES` salvo excepción registrada (AD-10).
- `GET /notifications`, `POST /notifications/{id}/read`.
- `GET /health`: `{ status, version, checks: { db, storage, queue } }`, sin autenticación, sin datos de tenant; el worker expone su propio `/health` (AD-19).

## 14. Trazabilidad

| Requisito | Dónde se satisface |
| --- | --- |
| FR-002 aislamiento | §3 (`withTenant`), §4 (404 sin filtración), §12 |
| FR-003 autenticación | §3 |
| FR-004 permiso a nivel de objeto | §4 |
| FR-007 / NFR-017 auditoría | §8, §13 |
| FR-011 API documentada | §2, §5, §6, §10 |
| FR-012 jobs consultables | §5 (límites), §13 |
| FR-013 exportación | §5, §13 |
| NFR-001 rate limiting, URLs firmadas | §11, §4 (`download-url`) |
| NFR-002 sin secretos al cliente | §3 (hash de API key, valor mostrado una vez), §6 (`INTERNAL_ERROR`) |
| NFR-004 archivos | §5, §4 (`/files`) |

## 15. Decisiones que requieren aprobación humana

| # | Decisión propuesta en este documento | Tipo | Por qué necesita aprobación |
| --- | --- | --- | --- |
| A-1 | Prefijo `/api/v1` en lugar de `/api/` de P2 §43 | technical_design_decision | Cambia las rutas enumeradas en un artefacto de nivel 1 |
| A-2 | better-auth 1.7 como proveedor de sesión (ADR-011) | seguridad | Arquitectura de seguridad (CLAUDE.md) |
| A-3 | Formato y hash SHA-256 de API keys; scopes ⊆ permisos del creador; rotación con solapamiento de 24 h | seguridad | Credenciales; nuevos eventos `API_KEY_*` |
| A-4 | Ampliación del catálogo de códigos de error y su mapeo HTTP (§6) | technical_design_decision | Afecta a `kernel/errors.ts`, compartido por todos los contextos |
| A-5 | `If-Match` obligatorio también en `POST` de transición | technical_design_decision | Extiende AD-11 |
| A-6 | Límites de rate limiting (§11) y de tamaño (§5) | seguridad | Umbrales operativos (PRD D-7) |
| A-7 | Metadatos de sesión en `payload.actor_metadata` del envelope | technical_design_decision | Toca el contrato de evento AD-5 sin cambiar sus campos |
| A-8 | `openapi.json` público; visor `/docs` autenticado; drift check en CI | seguridad | Exposición de superficie |
| A-9 | Política de deprecación (6 meses, `Sunset`) | product_requirement | Compromiso con integradores |
| A-10 | Cursor firmado con HMAC y `total` bajo demanda | technical_design_decision | Rendimiento y contrato de lista |
