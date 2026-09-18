---
titulo: "ADR-009: Generación de documentos e informes (HTML a PDF, CSV/JSON, XLSX en F2)"
fecha: 2026-09-17
estado: "propuesto, requiere aprobación humana"
nivel: 3
decisiones_spine: [AD-16, AD-13, AD-20]
inputs:
  - _bmad-output/planning-artifacts/architecture/architecture-ISO42001-2026-09-17/ARCHITECTURE-SPINE.md
  - CLAUDE.md
  - _bmad-output/planning-artifacts/prds/prd-ISO42001-2026-09-17/prd.md (NFR-015, NFR-018, NFR-021)
  - docs/product/ROADMAP.md (§3, cambio 19)
sustituye: []
---
# ADR-009: Generación de documentos e informes (HTML a PDF, CSV/JSON, XLSX en F2)

## Contexto

`reporting-essentials` entrega cuatro informes esenciales en ES y EN con versión de SoA y de norma, exportación PDF/CSV/JSON y la exportación de documentos aprobados y actas (políticas, alcance, SoA, informe de auditoría, acta de revisión por la dirección). AD-16 fija que un informe es una `ReportDefinition` declarativa sobre `QueryModel` sin SQL libre, que `ReportRun` es asíncrono con `sha256` del resultado y que el render ocurre en `apps/worker`. Este ADR decide el motor de render y los formatos. Toda puntuación mostrada debe llevar su desglose (CLAUDE.md), y ningún documento generado puede contener los términos prohibidos de PR-DR-009.

## Decisión

1. **Render solo en `apps/worker`.** El route handler crea `ReportRun { definition_id, params, locale, requested_by, status: Queued }` y encola `report.render`; nunca renderiza en la petición HTTP.
2. **HTML como formato intermedio.** Cada `ReportDefinition` referencia una plantilla en `packages/domain-reporting/src/templates/<report>.tsx` que se renderiza a HTML estático con `react-dom/server` `[ASSUMPTION]`, usa los tokens de `packages/ui` con CSS de impresión y traduce con `packages/i18n` según `ReportRun.locale`.
3. **PDF con Chromium headless vía Playwright 1.63** (`page.pdf`) dentro del worker; fuentes empaquetadas en la imagen, navegador sin acceso a red durante el render `[ASSUMPTION]`, un navegador por proceso, timeout de 120 s y concurrencia 1 por worker `[ASSUMPTION]`.
4. **CSV y JSON** se generan directamente desde el dataset del `QueryModel` (streaming; CSV UTF-8 con cabecera `[ASSUMPTION sobre BOM]`), sin pasar por HTML.
5. **XLSX en F2 con ExcelJS 4.4 (MIT).** El paquete npm `xlsx` queda prohibido por CVEs sin corrección en el registro npm; la prueba de licencias y dependencias lo rechaza.
6. **Pie obligatorio** en todo documento: `generated_at`, tenant, `standard_version`, `soa_version`, `sha256`, locale y la etiqueta `Certification preparation status` cuando aplique; nunca un veredicto de certificación.
7. **Salida** al puerto `ObjectStorage` (ADR-008) bajo `tenants/<tenant_id>/reports/<report_run_id>/<version>`; `sha256` calculado en servidor y guardado en `ReportRun`.
8. **Añadir un informe = añadir una definición y una plantilla**; no hay route handler ni servicio nuevos (NFR-021).

## Alternativas consideradas

| Alternativa | Por qué se descarta |
| --- | --- |
| `@react-pdf/renderer` | Motor de maquetación propio, sin paridad HTML/CSS; tablas largas y paginación débiles; duplica estilos frente a la UI. |
| LaTeX | Toolchain pesado en la imagen y competencia ajena al equipo web; sin reutilizar el design system. |
| Servicio externo de PDF (SaaS) | El contenido del informe saldría de la región del tenant (NFR-015) y añade coste por documento y dependencia de terceros. |
| Puppeteer | Equivalente funcional, pero Playwright ya está en el stack (E2E); un solo binario de Chromium que mantener. |
| Render en la petición HTTP de `apps/web` | Bloquea el pool, consume memoria en el proceso web y rompe la separación web/worker (AD-20). |

## Consecuencias

**Positivas.** Una sola tecnología de plantillas para UI e informes; bilingüismo gratuito vía `packages/i18n`; PDF fiel al HTML; integridad por `sha256`; informes nuevos sin código.

**Negativas.** Chromium en la imagen del worker aumenta su tamaño y superficie de seguridad `[ASSUMPTION sobre tamaño]`; los PDF no son byte-idénticos entre versiones de Chromium, por lo que el `sha256` certifica el artefacto emitido, no la reproducibilidad.

**Cambios OpenSpec.** `reporting-essentials` (motor, plantillas, servicio de render, cuatro informes); `scoring-kpi-dashboard` (datasets de `ScoreSnapshot`); `aims-context-parties-scope` y `aims-policy-objectives-roles` (exportación de alcance y política); `soa-control-activation` (informe de completitud); `internal-audit-core` (informe de auditoría); `management-review-core` (acta); `certification-readiness-workspace`; `mvp-hardening` (rendimiento p95 del render).

## Verificación

- Snapshot del HTML de cada `ReportDefinition` en `es` y `en`; cambio de plantilla = revisión explícita.
- Prueba de humo del PDF: se genera, tiene al menos una página y el texto extraído contiene el pie con `soa_version` y `standard_version` `[ASSUMPTION herramienta de extracción]`.
- Prueba de términos prohibidos sobre el HTML renderizado de todos los informes.
- Prueba de arquitectura "nuevo informe sin code path": añadir una definición de fixture no modifica el número de route handlers ni de servicios de aplicación; ninguna plantilla importa `packages/db` ni `@prisma/client`.
- `dependency-cruiser` `[ASSUMPTION]`: `playwright` solo se importa en `apps/worker`; `xlsx` está en la lista de paquetes prohibidos del lockfile.

## Estado y aprobación

Propuesto. Requiere aprobación humana porque fija el motor de render de toda la plataforma (bloqueo de arquitectura), introduce un navegador completo en la imagen de producción (superficie de seguridad) y descarta servicios externos por residencia de datos, lo que condiciona el coste operativo. La elección de ExcelJS para F2 y los parámetros de concurrencia y timeout son `[ASSUMPTION]` revisables en `reporting-essentials`.
