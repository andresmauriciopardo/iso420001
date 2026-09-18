---
titulo: "ADR-010: Estrategia de pruebas por capas con invariantes probadas por identificador"
fecha: 2026-09-17
estado: "propuesto, requiere aprobación humana"
nivel: 3
decisiones_spine: [AD-23, AD-3, AD-2]
inputs:
  - _bmad-output/planning-artifacts/architecture/architecture-ISO42001-2026-09-17/ARCHITECTURE-SPINE.md
  - CLAUDE.md
  - docs/product/PROMPT_REVIEW_AND_ADJUSTMENTS.md (A9)
  - _bmad-output/planning-artifacts/prds/prd-ISO42001-2026-09-17/prd.md (NFR-023, NFR-024, §16)
  - _bmad-output/planning-artifacts/prds/prd-ISO42001-2026-09-17/addendum.md (§D.5)
  - docs/product/ROADMAP.md (§3)
sustituye: []
---
# ADR-010: Estrategia de pruebas por capas con invariantes probadas por identificador

## Contexto

CLAUDE.md exige que las pruebas acompañen cada cambio y que una casilla `[x]` en `tasks.md` solo se marque con pruebas, lint y typecheck en verde. NFR-023 pide la pirámide completa; NFR-024 pide pruebas de dominio con fixtures derivados de `resumenes/` (38 controles con distribución exacta, 26 términos, 11 objetivos, 7 fuentes) y pruebas de contrato de conectores; PRD §16.1 define un E2E de 15 pasos y §16.2 diez criterios técnicos. AD-23 añade que cada regla `PR-DR-nnn` tenga un caso nombrado y que existan pruebas de arquitectura. Este ADR fija herramientas, capas y política de CI.

## Decisión

**Siete capas, una convención de nombre.**

1. **Unitarias (Vitest 5.0)** en cada `packages/domain-*`, sin Next.js ni BD: transiciones de estado, permisos, puntuaciones. Cada regla de dominio tocada tiene un caso cuyo nombre empieza por su identificador: `it('PR-DR-011: reviewer cannot approve own evidence', ...)`. Umbral de cobertura del 80 % de líneas en paquetes de dominio `[ASSUMPTION]`.
2. **Integración con PostgreSQL 18 real** en contenedor (Testcontainers `[ASSUMPTION]`): migraciones aplicadas, RLS con dos tenants, repositorios, route handlers invocados como funciones con `Request` real. Prohibido sustituir la BD por mocks o SQLite.
3. **E2E (Playwright 1.63)** en `tests/e2e` contra `docker-compose`: los 15 pasos de §16.1 como escenarios etiquetados `@step-nn`, ejecutados en `es` y `en`.
4. **Seguridad** en `tests/security`: cross-tenant, escalada de privilegios, BOLA, exposición de secretos (ADR-014), términos prohibidos; se ejecuta en cada PR.
5. **Dominio de la semilla** en `packages/standards-seed`: distribución de los 38 controles, 26 términos, 11 objetivos, 7 fuentes, cero texto literal (n-gramas ≥ 12 palabras contra `resumenes/`, addendum D.5), cada registro cita `resumenes/NN §sección`.
6. **Arquitectura** en `tests/architecture` con `dependency-cruiser` `[ASSUMPTION]`: arcos permitidos de AD-3, "nuevo informe sin código", "nuevo conector sin tocar dominio", `process.env` solo en `packages/kernel/config.ts`, Prisma solo en `packages/db` y repositorios de dominio.
7. **Accesibilidad** con `@axe-core/playwright` `[ASSUMPTION]` sobre las pantallas del E2E (WCAG 2.2 AA), incorporada en `mvp-hardening`.

**Política de mocks.** Se mockean solo puertos externos (`LLMProvider`, `ObjectStorage`, `SecretStore`, adaptadores de conectores); cada adaptador real tiene además una prueba de contrato contra su servicio en contenedor. Ninguna prueba afirma un conector `validated` sin credenciales reales.

**CI (GitHub Actions).** Etapas `lint+typecheck → unit → architecture → integration → security → e2e`; en PR corre todo salvo el E2E completo, que corre en `main` y en nocturno `[ASSUMPTION]`. Un fallo en cualquier etapa bloquea el merge.

## Alternativas consideradas

| Alternativa | Por qué se descarta |
| --- | --- |
| Jest | Más lento con ESM y TypeScript 7; Vitest comparte configuración con el toolchain y ya está en ADR-001. |
| Mocks de BD o SQLite en integración | RLS, triggers append-only y `jsonb` no se pueden verificar; darían falsa confianza. |
| Solo reglas de import de ESLint en lugar de pruebas de arquitectura | No detecta ciclos ni prueba "nuevo informe sin código". |
| Cypress | Playwright ya sirve para E2E y para el render PDF (ADR-009); un solo navegador. |
| QA manual como puerta | Contradice "pruebas acompañan cada cambio" y no escala a 24 cambios. |

## Consecuencias

**Positivas.** Cada `PR-DR-nnn` es localizable por nombre de prueba, lo que permite generar la matriz de trazabilidad desde el código; RLS y append-only se prueban contra el motor real; las invariantes de arquitectura no dependen de revisión humana.

**Negativas.** Tiempo de CI estimado de 15 a 20 minutos por PR `[ASSUMPTION]`; los runners necesitan Docker para Testcontainers.

**Cambios OpenSpec.** `foundation-project-bootstrap` (frameworks, CI, pruebas de arquitectura base); `organization-tenancy` (primera prueba RLS cross-tenant); `identity-and-rbac` (escalada y BOLA); `immutable-audit-trail` (no existe update/delete); `standards-and-requirements-engine` (pruebas de dominio de la semilla); `connector-sdk-mock` (kit de contrato); `reporting-essentials` ("nuevo informe sin code path"); `mvp-hardening` (E2E de 15 pasos, axe, rendimiento).

## Verificación

- Un meta-test recorre los archivos de prueba y comprueba que cada `PR-DR-001..030` declarado en `openspec/` tiene al menos un caso con su identificador; su salida alimenta `docs/compliance/TRACEABILITY_MATRIX.md`.
- La propia tubería de CI: cada etapa publica su informe; el merge exige todas en verde.
- Prueba de que `tests/integration` no contiene mocks de `@prisma/client`.

## Estado y aprobación

Propuesto. Requiere aprobación humana porque fija herramientas para toda la plataforma (Testcontainers, dependency-cruiser y axe son `[ASSUMPTION]`), porque el umbral de cobertura y la política de CI condicionan qué se puede publicar (CLAUDE.md: publicar requiere aprobación) y porque la prueba de texto literal implementa NFR-020, cuyo mecanismo el PRD deja como supuesto.
