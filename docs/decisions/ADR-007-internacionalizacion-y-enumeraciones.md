---
titulo: "ADR-007: Internacionalización por clave y enumeraciones canónicas únicas"
fecha: 2026-09-17
estado: "propuesto, requiere aprobación humana"
nivel: 3
decisiones_spine: [AD-12, AD-22]
inputs:
  - _bmad-output/planning-artifacts/architecture/architecture-ISO42001-2026-09-17/ARCHITECTURE-SPINE.md
  - CLAUDE.md
  - docs/product/PROMPT_REVIEW_AND_ADJUSTMENTS.md (A5)
  - _bmad-output/planning-artifacts/prds/prd-ISO42001-2026-09-17/prd.md (NFR-018)
  - _bmad-output/planning-artifacts/prds/prd-ISO42001-2026-09-17/addendum.md (§C)
  - docs/product/ROADMAP.md (§3)
sustituye: []
---
# ADR-007: Internacionalización por clave y enumeraciones canónicas únicas

## Contexto

La plataforma es bilingüe ES/EN desde la fundación (A5, NFR-018) y la terminología oficial procede del glosario `resumenes/13` (`safety` = protección, `security` = seguridad, `statement of applicability` = declaración de aplicabilidad). El addendum §C fija más de veinte enumeraciones canónicas con valores en inglés que atraviesan los 21 bounded contexts. Sin una regla única, dos cambios OpenSpec construidos por separado producirían `'not_applicable'` en un módulo y `'Not Applicable'` en otro, textos hardcodeados en componentes y, lo más grave para el producto, cadenas prohibidas por PR-DR-009 (`certified`, `certificado`, `compliance score`), porque la plataforma nunca declara "certificado ISO" (CLAUDE.md).

## Decisión

1. **Enumeraciones en un solo lugar.** Cada enumeración del addendum §C se define una única vez en `packages/kernel/enums/<entity>.ts` como `const` object + tipo derivado, con los valores en inglés exactamente como los lista el addendum. El esquema Prisma declara el `enum` correspondiente con los mismos valores; ningún paquete de dominio redefine ni "alias" valores. El dominio y la API intercambian siempre el valor canónico, jamás su traducción.
2. **Traducción por clave con next-intl 4.14** `[ASSUMPTION librería, fijada en el spine]`. Los mensajes viven en `packages/i18n/messages/{es,en}/<namespace>.json`; las claves siguen `namespace.key`; las enumeraciones se traducen por `enum.<Entity>.<Value>` (por ejemplo `enum.SoAItem.Not Applicable`). `packages/i18n` exporta utilidades sin dependencia de Next.js para que `apps/worker` traduzca correos, informes y notificaciones con el mismo catálogo.
3. **Resolución de idioma** `[ASSUMPTION]`: preferencia del usuario, después idioma por defecto del tenant, después `Accept-Language`; el idioma efectivo viaja en `TenantContext.locale` y se persiste en cada `ReportRun`.
4. **El dominio no produce texto de interfaz.** Los servicios devuelven códigos canónicos (`kernel/errors.ts`) y datos; `problem+json.detail` se emite en inglés técnico con `code`, y la UI traduce el `code`. Ningún componente contiene literales de usuario fuera del catálogo.
5. **Términos prohibidos.** La lista `FORBIDDEN_TERMS = ['certified', 'certificado', 'compliance score']` vive en `packages/kernel` y la consumen las pruebas (PR-DR-009). El estado de preparación se nombra siempre `Certification preparation status` (addendum D.2), nunca un veredicto.

## Alternativas consideradas

| Alternativa | Por qué se descarta |
| --- | --- |
| react-i18next / i18next | Madura, pero su integración con App Router y Server Components exige más pegamento propio que next-intl; no aporta ventaja funcional para dos idiomas. |
| FormatJS / react-intl | Orientado a cliente; la traducción en `apps/worker` (correos, PDF) requeriría un segundo mecanismo. |
| Enumeraciones como tablas de catálogo en BD | Flexibilidad no requerida en el MVP; rompe el tipado estático y la trazabilidad addendum §C → código; los valores cambiarían sin migración ni revisión. |
| Enumeraciones duplicadas por módulo | Es exactamente el fallo que AD-12 previene. |
| Valores canónicos en español | Contradice A5 (código e identificadores en inglés) y la fuente P2, que define los valores en inglés. |

## Consecuencias

**Positivas.** Un único vocabulario canónico en BD, API, eventos y UI; paridad ES/EN verificable en CI; la regla PR-DR-009 pasa de norma escrita a prueba automática; añadir idioma no toca dominio.

**Negativas.** Toda cadena visible obliga a tocar dos archivos JSON; los valores con espacios (`Not Applicable`) se mapean en Prisma con `@map` `[ASSUMPTION]`.

**Cambios OpenSpec que la implementan.** `foundation-project-bootstrap` (paquetes `kernel/enums`, `i18n`, prueba de paridad y de términos prohibidos, página vacía bilingüe); `standards-and-requirements-engine` (`record_type` y estados de requisito); `soa-control-activation` (aplicabilidad y ciclo del control); `evidence-library` (estados y tipos de evidencia); `tasks-approvals-notifications` (plantillas de correo bilingües); `scoring-kpi-dashboard` (prueba de 0 apariciones de términos prohibidos); `reporting-essentials` (informes en ES y EN); `mvp-hardening` (barrido final).

## Verificación

- Prueba de paridad en `packages/i18n`: el conjunto de claves de `es` y `en` es idéntico y ninguna cadena está vacía; falla el build ante clave faltante.
- Prueba de términos prohibidos: recorre `messages/**`, el `openapi.json` generado y las fixtures de respuesta de la API buscando `FORBIDDEN_TERMS` sin distinguir mayúsculas; cero coincidencias.
- Prueba de unicidad: un script compara los valores de `packages/kernel/enums/*.ts` con los `enum` del esquema Prisma y con las claves `enum.<Entity>.<Value>` de ambos idiomas; cualquier desalineación falla.
- E2E Playwright: los 15 pasos de §16.1 se ejecutan en `es` y `en`.

## Estado y aprobación

Propuesto. Requiere aprobación humana porque fija una librería de i18n para toda la plataforma (CLAUDE.md: aprobación antes de bloquear la arquitectura) y porque los valores canónicos del addendum §C son parte del contrato de trazabilidad nivel 2 → 3; cualquier cambio posterior de un valor canónico se trata como cambio de requisito, no como refactor. La restricción de `Not Applicable` para requisitos (D-2) es interpretación normativa y se aprueba aparte.
