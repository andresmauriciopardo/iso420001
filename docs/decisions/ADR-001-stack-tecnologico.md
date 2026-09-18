---
titulo: "ADR-001: Stack tecnológico por defecto"
fecha: 2026-09-17
estado: "propuesto, requiere aprobación humana"
nivel: 3
decisiones_spine: [AD-1, AD-20, AD-22]
inputs: [CLAUDE.md, docs/product/PROMPT_REVIEW_AND_ADJUSTMENTS.md (A8), _bmad-output/planning-artifacts/architecture/architecture-ISO42001-2026-09-17/.memlog.md (entradas version)]
sustituye: []
---

# ADR-001: Stack tecnológico por defecto

## Contexto

El repositorio no contiene código de aplicación; `CLAUDE.md` ("Estándares de ingeniería") y el ajuste A8 fijan un stack por defecto que la arquitectura puede modificar con justificación. El equipo es pequeño, el MVP son 24 cambios OpenSpec ejecutados por agentes con revisión humana, y el producto exige: TypeScript de extremo a extremo para compartir esquemas (Zod) entre API, UI y dominio; PostgreSQL como única base (RLS, outbox, colas); UI bilingüe accesible; pruebas unitarias, de integración, E2E y de seguridad en cada cambio; contenedores para desplegar en cualquier proveedor. Todas las versiones se verificaron en la web el 2026-09-17 (registro npm, endoflife.date y sitios oficiales; ver memlog).

## Decisión

Adoptar el stack por defecto de `CLAUDE.md` con las versiones mayores verificadas y dos sustituciones justificadas (autenticación y almacenamiento local, en ADR-011 y ADR-008):

| Capa | Tecnología | Versión verificada |
| --- | --- | --- |
| Runtime | Node.js Active LTS | 24.21 (migración a 26 LTS planificada para octubre de 2026) |
| Lenguaje | TypeScript `strict` | 7.0 (compilador nativo; 6.0 como puente si una herramienta no lo soporta) |
| Web | Next.js App Router + React | 16.3 / 19.3 |
| UI | Tailwind CSS + shadcn/ui (Base UI por defecto) | 4.3 / CLI 4.21 |
| Datos | PostgreSQL + Prisma ORM | 18.6 / 7.10 (Prisma 8 en RC: migración planificada) |
| Validación | Zod | 4.6 |
| Colas y jobs | pg-boss | 12.33 |
| Observabilidad | OpenTelemetry JS (`api` / `sdk-trace-node` / `sdk-node`) | 1.9 / 2.11 / 0.222 (fijar exactas) |
| Pruebas | Vitest / Playwright | 5.0 / 1.63 |
| Monorepo | pnpm + Turborepo `[ASSUMPTION]` | 12.4 (o 10.x si hay fricción) / 2.10 |
| i18n | next-intl `[ASSUMPTION]` | 4.14 |
| OpenAPI | zod-openapi `[ASSUMPTION]` | 6.0 |
| Autenticación | better-auth (ADR-011) `[ASSUMPTION]` | 1.7 |
| Contenedores y CI | Docker Engine / GitHub Actions | 29.8 / `actions/checkout` v7 |

Las versiones exactas las fija el `lockfile` en `foundation-project-bootstrap`; este ADR pinnea la versión mayor. Ningún paquete con licencia copyleft entra sin aprobación humana (`CLAUDE.md`).

## Alternativas consideradas

| Alternativa | Por qué se descarta |
| --- | --- |
| Backend separado (NestJS/Fastify) + SPA | Duplica esquemas y despliegues; el monolito modular con Route Handlers cubre el MVP y mantiene un solo `TenantContext` |
| Drizzle ORM en lugar de Prisma | `CLAUDE.md` fija Prisma; Prisma 7 verificado y con esquema multi-archivo suficiente; revisar en la migración a Prisma 8 |
| BullMQ + Redis | Añade una dependencia operativa temprana; pg-boss cubre reintentos, cron y singleton keys sobre PostgreSQL (ADR-004) |
| Jest | Vitest 5 es nativo de Vite/TS y comparte configuración con el front |
| Auth.js/next-auth | En modo mantenimiento desde septiembre de 2025; v5 sin GA ni TOTP nativo (ADR-011) |
| MinIO CE como S3 local | Repositorio archivado en abril de 2026 (ADR-008) |
| Node 26 Current | Aún no es LTS; se adopta en cuanto lo sea |

## Consecuencias

- Positivas: un único lenguaje y un único despliegue; esquemas Zod compartidos entre API, UI, eventos y configuración; PostgreSQL concentra RLS, outbox, colas y auditoría; ecosistema verificado y activo.
- Negativas: dependencia del ciclo de releases de Next.js; `sdk-node` de OpenTelemetry es 0.x y puede romper entre minors (fijar exactas); shadcn/ui migró a Base UI y la documentación de terceros aún referencia Radix; pnpm 12 reescrito en Rust puede fricionar en CI.
- Riesgos: Prisma 8 cambia paquetes por base de datos; se planifica un cambio propio de hardening. TypeScript 7 nativo puede no estar soportado por alguna herramienta menor: se mantiene 6.0 como puente.
- Cambios OpenSpec que lo implementan: `foundation-project-bootstrap` (todo el stack, CI, Docker, i18n base), `mvp-hardening` (actualizaciones y migración Node 26/Prisma 8 si procede).

## Verificación

- CI en verde con lint, typecheck, pruebas y escaneo de dependencias, SAST y secretos.
- `package.json` raíz con `engines.node` y `packageManager` fijados; `renovate`/`dependabot` con agrupación por mayor.
- Prueba de arquitectura: ningún paquete fuera de `packages/db` y `adapters/prisma` importa `@prisma/client`.
- Revisión de licencias automatizada (`license-checker` o equivalente `[ASSUMPTION]`) que falla ante copyleft no aprobado.

## Estado y aprobación

Propuesto. `CLAUDE.md` exige aprobación humana antes de bloquear la arquitectura; el stack es su primer bloque. Requiere además decisión explícita sobre las dos sustituciones (better-auth, SeaweedFS) y sobre el gestor de paquetes. Punto de aprobación: Checkpoint 0, antes de `foundation-project-bootstrap`.
