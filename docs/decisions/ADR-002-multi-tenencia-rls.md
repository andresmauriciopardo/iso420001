---
titulo: "ADR-002: Multi-tenencia con PostgreSQL compartido, tenant_id y Row Level Security"
fecha: 2026-09-17
estado: "propuesto, requiere aprobación humana"
nivel: 3
decisiones_spine: [AD-2, AD-13]
inputs: [CLAUDE.md, docs/product/PROMPT_REVIEW_AND_ADJUSTMENTS.md (A8), _bmad-output/planning-artifacts/prds/prd-ISO42001-2026-09-17/prd.md (FR-002, NFR-001, NFR-002, NFR-015), docs/product/MASTER_ISO42001_PLATFORM_REQUIREMENTS.md (§4, §40)]
sustituye: []
---

# ADR-002: Multi-tenencia con PostgreSQL compartido, `tenant_id` y Row Level Security

## Contexto

La plataforma es SaaS multi-tenant desde el primer cambio (FR-001, FR-002). El aislamiento de datos es el requisito de seguridad número uno (NFR-001, NFR-002, P2 §40 "never expose cross-tenant data") y un fallo aquí es un incidente de cumplimiento para el cliente. El ajuste A8 fija PostgreSQL compartido con `tenant_id` en toda tabla y RLS obligatoria, más aislamiento del almacenamiento de objetos por prefijo, y descarta esquema-por-tenant en el MVP. Debe convivir con un catálogo normativo global (ADR-006), jobs que recorren varios tenants y un rol `Platform Administrator` que no debe ver datos de negocio por defecto (addendum §B).

## Decisión

- Una única base de datos PostgreSQL 18 y un único esquema de aplicación. Toda tabla de negocio lleva `tenant_id uuid NOT NULL` con `ENABLE ROW LEVEL SECURITY` y `FORCE ROW LEVEL SECURITY`, y una política `USING/WITH CHECK (tenant_id = current_setting('app.tenant_id', true)::uuid)`. Un generador en `packages/db` aplica la política a toda tabla con esa columna; una prueba de integración falla si alguna carece de ella.
- Los roles de conexión de la aplicación (`app_rw`, `worker_rw`) no tienen `BYPASSRLS` ni son propietarios de las tablas. El superusuario nunca se usa desde la aplicación.
- Cada transacción empieza con `SET LOCAL app.tenant_id` y `SET LOCAL app.actor_id`, ejecutados por `withTenantTransaction(ctx, fn)` en `packages/db`; fuera de transacción la política no coincide y la consulta devuelve vacío (*fail-closed*).
- Las tablas del catálogo normativo global no llevan `tenant_id` y son de solo lectura para la aplicación (ADR-006). `pgboss.*` y `outbox_events` son técnicas: `outbox_events` lleva `tenant_id` y RLS; `pgboss` solo la usa el worker.
- Los jobs multi-tenant iteran la lista de tenants y fijan `app.tenant_id` por iteración; no existe un "modo sin tenant" para tablas de negocio.
- Almacenamiento de objetos: toda clave empieza por `tenants/<tenant_id>/`; el adaptador rechaza claves fuera del prefijo del `TenantContext`.
- `data_region` se registra por tenant desde `organization-tenancy`; la residencia efectiva (un despliegue por región) es una decisión de despliegue diferida (PRD Q6).
- El `Platform Administrator` administra tenants y catálogo pero solo accede a datos de negocio con `SupportAccessGrant` consentido por el tenant y registrado en el audit trail `[ASSUMPTION, addendum §B regla 1]`.

## Alternativas consideradas

| Alternativa | Por qué se descarta |
| --- | --- |
| Esquema por tenant | Migraciones × N tenants, conexiones y pool por esquema, catálogo global duplicado o cross-schema; A8 lo descarta para el MVP |
| Base de datos por tenant | Coste operativo alto, copias y PITR por tenant, imposible agregar métricas de plataforma; solo justificado para clientes con requisito contractual (F3) |
| Filtro `tenant_id` solo en la capa de aplicación (sin RLS) | Un `where` olvidado es una fuga; RLS es defensa en profundidad y verificable con una prueba genérica |
| RLS con `SET ROLE` por tenant | Miles de roles de BD; complica el pool; `current_setting` es el patrón estándar |
| Cifrado por tenant como único aislamiento | No evita listados cruzados; se usa como complemento para columnas sensibles (SECURITY_ARCHITECTURE) |

## Consecuencias

- Positivas: aislamiento verificable por una prueba genérica; una sola migración por cambio; catálogo global compartido sin duplicar; coste operativo mínimo.
- Negativas: todo acceso a datos debe pasar por `withTenantTransaction` (disciplina en el código; prueba de arquitectura que prohíbe `prisma.$transaction` fuera de `packages/db`); las consultas agregadas de plataforma (métricas globales) requieren iterar tenants o vistas materializadas dedicadas; RLS añade un predicado a cada consulta (índices `(tenant_id, …)` obligatorios).
- Riesgos: `Prisma` no gestiona `SET LOCAL` de forma nativa; el wrapper en `packages/db` es crítico y se prueba con casos negativos. Un pool compartido con transacciones interrumpidas podría filtrar `app.tenant_id` si se usara `SET` en lugar de `SET LOCAL`; se prohíbe `SET` no local por lint.
- Cambios OpenSpec que lo implementan: `organization-tenancy` (RLS, `tenants`, `data_region`, prueba cross-tenant), `foundation-project-bootstrap` (roles de BD, wrapper), `evidence-library` (prefijos de almacenamiento), `mvp-hardening` (suite de seguridad completa).

## Verificación

- Prueba de integración que enumera `information_schema.columns` con `tenant_id` y comprueba `relrowsecurity` y `relforcerowsecurity` en `pg_class`, más la existencia de la política.
- Suite `tests/security/cross-tenant.spec.ts`: usuario A con todos los permisos no lee, lista, actualiza ni descarga objetos de B por API, UI ni URL firmada; jobs no mezclan tenants.
- Prueba negativa: consulta sin `SET LOCAL` devuelve vacío y registra alerta.
- Revisión de `GRANT`s en la migración inicial: `app_rw` sin `BYPASSRLS`, sin `DELETE` en tablas de cumplimiento.

## Estado y aprobación

Propuesto. Es arquitectura de seguridad y bloquea el modelo de datos: `CLAUDE.md` exige aprobación humana. Requieren decisión explícita: el mecanismo de acceso de soporte del `Platform Administrator` y la política de residencia (`data_region` registrada frente a despliegue por región). Punto de aprobación: antes de `organization-tenancy`.
