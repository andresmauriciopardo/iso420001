---
titulo: "ADR-013: Estilo de API (Route Handlers, Zod, OpenAPI, problem+json, cursor, idempotencia)"
fecha: 2026-09-17
estado: "propuesto, requiere aprobación humana"
nivel: 3
decisiones_spine: [AD-15, AD-4, AD-8, AD-22]
inputs:
  - _bmad-output/planning-artifacts/architecture/architecture-ISO42001-2026-09-17/ARCHITECTURE-SPINE.md
  - CLAUDE.md
  - docs/product/MASTER_ISO42001_PLATFORM_REQUIREMENTS.md (§43)
  - docs/product/ROADMAP.md (§3)
sustituye: []
---
# ADR-013: Estilo de API (Route Handlers, Zod, OpenAPI, problem+json, cursor, idempotencia)

## Contexto

P2 §43 pide una API documentada organizada por dominio (`/api/organizations`, `/api/soa`, `/api/ai-systems`, …) y no menciona versionado. La API la consumen la UI, integraciones externas por API key y, en F2, conectores. Sin contrato único, 21 contextos producirían envelopes de error, paginación y semántica de concurrencia distintos, y las integraciones romperían con cada cambio. AD-15 fija el contrato; este ADR lo detalla y documenta la única desviación respecto a P2: el prefijo `/v1`.

## Decisión

1. **Route Handlers de Next.js 16.3** en `apps/web/app/api/v1/<domain>/…` `[ASSUMPTION: prefijo v1; P2 §43 no versiona]`, con rutas kebab-case en plural (AD-22) que reproducen la lista de P2 §43 (`/api/v1/ai-systems`, `/api/v1/soa`, `/api/v1/impact-assessments`, …). No hay servidor de API separado (AD-20).
2. **Un solo constructor de endpoint** `defineRoute({ permission, input: { params, query, body }, output, handler })` en `apps/web/lib/api/`. Valida con Zod 4.6, resuelve `TenantContext` (ADR-011), declara el permiso (AD-8), invoca el servicio de aplicación y mapea el `Result` al HTTP. Los esquemas DTO son propiedad del dominio (`packages/domain-<ctx>/src/contracts/`); el handler no contiene lógica.
3. **OpenAPI 3.1 generado con zod-openapi 6.0** `[ASSUMPTION]` a partir de los mismos esquemas; servido en `/api/v1/openapi.json` y versionado como snapshot en `docs/api/openapi.json` `[ASSUMPTION ruta]`.
4. **Errores `application/problem+json`** (RFC 9457) con `type, title, status, detail, code, correlation_id, errors[]`. Mapeo canónico: `PERMISSION_DENIED → 403`; `TENANT_MISMATCH` y objeto inexistente → `404` para no revelar existencia `[ASSUMPTION]`; `INVARIANT_VIOLATION → 422` con `PR-DR-nnn` en `detail`; `CONFLICT_VERSION → 412`; `If-Match` ausente → `428`; `Idempotency-Key` ausente → `400 IDEMPOTENCY_KEY_REQUIRED`.
5. **Paginación por cursor** `{ items, next_cursor, total? }`; `limit` por defecto 25 y máximo 200 `[ASSUMPTION]`; filtros `?filter[field]=`; orden `?sort=-created_at`.
6. **Idempotencia y concurrencia.** `Idempotency-Key` obligatoria en todo POST que crea; se guarda en `idempotency_keys(tenant_id, key, request_hash, response, expires_at)` con vigencia de 24 h `[ASSUMPTION]`; misma clave con cuerpo distinto → `422 IDEMPOTENCY_MISMATCH`. PATCH/PUT exigen `If-Match: "<version>"` contra `version` de la entidad (ADR-015).
7. **Autenticación**: cookie de sesión para la UI y `Authorization: Bearer <api_key>` para integraciones, ambas resueltas al mismo `TenantContext`. Rate limiting por principal.
8. **Lecturas desde Server Components** pueden llamar a los servicios de aplicación directamente con el mismo `authorize` `[ASSUMPTION]`; toda mutación desde la UI pasa por route handlers para que exista un único contrato auditable.

## Alternativas consideradas

| Alternativa | Por qué se descarta |
| --- | --- |
| tRPC | Tipado extremo a extremo para la UI, pero sin OpenAPI natural para integraciones por API key ni para P2 §43. |
| GraphQL | Autorización por campo y N+1 sobre RLS complican AD-8; sobredimensionado para recursos CRUD con máquinas de estado. |
| Servidor API separado (Fastify/NestJS) | Duplica autenticación y despliegue; contradice AD-20 (una imagen, dos modos). |
| Solo Server Actions | Sin API externa; imposible para integraciones y auditoría del contrato. |
| Sin versionado, literal a P2 §43 | Las integraciones externas necesitan estabilidad; el prefijo cuesta nada hoy y evita una migración de rutas. |

## Consecuencias

**Positivas.** Un solo envelope, un solo mapeo de errores, permisos declarados por endpoint y OpenAPI siempre alineado con Zod; los códigos `PR-DR-nnn` viajan en `detail`, lo que conecta la API con la trazabilidad.

**Negativas.** Cada endpoint exige `defineRoute`, esquemas de entrada y salida; `If-Match` e `Idempotency-Key` añaden fricción al cliente, absorbida por el cliente HTTP compartido de la UI.

**Riesgos.** Deriva entre OpenAPI publicado y comportamiento real, mitigada por el snapshot en CI.

**Cambios OpenSpec.** `foundation-project-bootstrap` (`defineRoute`, problem+json, generación OpenAPI, `/api/v1/health`); `identity-and-rbac` (resolución de sesión y API keys); todos los cambios de dominio (7-20) añaden sus recursos; `connector-sdk-mock` (consumo por API key); `search-traceability-navigation`; `mvp-hardening` (rate limiting, cabeceras).

## Verificación

- Prueba de arquitectura: todo archivo `route.ts` bajo `app/api/v1` usa `defineRoute`; ninguno importa `@prisma/client`.
- Snapshot de `openapi.json` comparado en CI; un cambio exige actualización explícita.
- Integración: cada `code` de `kernel/errors.ts` produce el `status` de la tabla y un cuerpo `problem+json` válido; POST sin `Idempotency-Key` → 400; PATCH sin `If-Match` → 428; `If-Match` obsoleto → 412; reintento con la misma clave devuelve la misma respuesta.
- `tests/security`: BOLA sobre cada recurso con ids del otro tenant → 404.

## Estado y aprobación

Propuesto. Requiere aprobación humana porque el prefijo `/v1` se desvía del documento de nivel 1 (P2 §43): según la jerarquía de CLAUDE.md, la desviación debe registrarse como corrección del artefacto superior y propagarse, no adoptarse en silencio. También deben ratificarse el mapeo `TENANT_MISMATCH → 404` (decisión de seguridad), la vigencia de las claves de idempotencia y los límites de paginación.
