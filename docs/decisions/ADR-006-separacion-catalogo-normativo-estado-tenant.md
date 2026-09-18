---
titulo: "ADR-006: Separación entre el catálogo normativo global versionado y el estado de cumplimiento por tenant"
fecha: 2026-09-17
estado: "propuesto, requiere aprobación humana"
nivel: 3
decisiones_spine: [AD-7, AD-12]
inputs: [CLAUDE.md, docs/product/PROMPT_REVIEW_AND_ADJUSTMENTS.md (A4, A6, A7, A10), _bmad-output/planning-artifacts/prds/prd-ISO42001-2026-09-17/prd.md (§8.1-§8.4, §17 D-2, D-3, FR-015..FR-021, FR-059, FR-060, FR-065), docs/product/DOMAIN_MAP.md (§1, §7.1), docs/product/TRACEABILITY_MODEL.md (§3)]
sustituye: []
---

# ADR-006: Separación entre el catálogo normativo global versionado y el estado de cumplimiento por tenant

## Contexto

El PRD §8.1 modela la norma como conocimiento global y de solo lectura (`Standard`, `StandardVersion`, `Clause`, `Requirement`, `Annex`, `ControlDomain`, `Control`, `ControlGuidance`, mapeos, `AuditQuestion`, `SeedRecordSource`) con varias versiones soportadas desde el inicio (42001:2023 + Amd 1:2024, UNE-ISO/IEC 42001:2025 como adopción idéntica, ISO 19011:2026, ISO/IEC 23894:2023 parcial, 42006 pendiente; A7). El estado de cumplimiento (`RequirementImplementation`, `SoAItem`, `ControlImplementation`) es de cada tenant. El PRD §17 D-3 adopta provisionalmente la separación y pide ratificarla antes de `standards-and-requirements-engine`; `DOMAIN_MAP.md` §7.1 la marca `[ASSUMPTION]`. A4 y A6 exigen que la semilla venga de `packages/standards-seed`, sin texto literal y con cita a `resumenes/NN §sección` (A10).

## Decisión

- El catálogo es **global** (sin `tenant_id`), **versionado** por `StandardVersion` e **inmutable** tras publicarse; se carga exclusivamente desde `packages/standards-seed` (dataset JSON semver revisado por humanos) por el job `seed-loader` con un rol de BD propio. El rol de aplicación solo lee.
- El estado del tenant referencia el catálogo por `catalog_id` + `standard_version_id` (`requirement_implementations.requirement_id`, `soa_items.control_id`, `risk_sources.catalog_source_id`, `audit_criteria.target_id`). `tenant_standard_adoptions` registra qué versión adopta cada tenant.
- Publicar una nueva `StandardVersion` **nunca** modifica filas de tenant. Migrar un tenant de versión es un command explícito (`adoptStandardVersion`) que crea nuevas filas de estado a partir del mapeo `supersedes` entre versiones y deja las antiguas como historial.
- Los controles adicionales o alternativos de la organización (FR-066) son `OrganizationControl` del tenant, referenciables desde `SoAItem`, y nunca se insertan en `controls` del catálogo.
- `record_type` obligatorio en todo registro normativo; la orientación del Anexo B se modela como `ControlGuidance` y jamás como `Control` (PR-DR-018).
- La ausencia de detalle se marca `source_detail_required = true` y se muestra como `SOURCE_DETAIL_REQUIRED` (PR-DR-019); ISO/IEC 42006 se registra como `pending_dependency` sin contenido.
- Cambiar una paráfrasis o un identificador de la semilla es cambio de interpretación normativa y exige aprobación humana (`CLAUDE.md`), incluso en versiones `patch`.

## Alternativas consideradas

| Alternativa | Por qué se descarta |
| --- | --- |
| Copiar la norma en cada tenant (una fila de requisito/control por tenant con su estado) | Consultar es más simple, pero actualizar de versión exige migración masiva por tenant; imposible garantizar que 38 controles sigan siendo 38 en todos; duplica paráfrasis sujetas a derechos de autor |
| Catálogo global mutable (editar controles publicados) | Rompe la trazabilidad `resumenes/ → semilla → catálogo → estado`; una corrección alteraría retroactivamente el significado de estados ya aprobados |
| Catálogo como archivos JSON leídos en runtime (sin tablas) | Impide claves foráneas desde el estado del tenant, búsquedas y mapeos consultables; la semilla sigue siendo JSON pero se materializa en tablas |
| Importar `resumenes/*.md` en runtime | Prohibido por A4: los resúmenes son nivel 0/1, no un dataset estructurado ni revisado para producto |
| Un solo `Requirement` polimórfico para cláusulas y controles | Mezcla `normative_requirement` de cláusulas 4-10 (no excluibles) con controles del Anexo A (excluibles con justificación); la SoA exige distinguirlos |

## Consecuencias

- Positivas: una norma, una verdad; nuevas versiones sin tocar tenants; pruebas de dominio deterministas sobre el catálogo (38/26/11/7); derechos de autor controlados en un único paquete; la matriz de trazabilidad cita el mismo `resumenes/NN §sección` que `SeedRecordSource`.
- Negativas: consultas de estado requieren unir tenant + catálogo (vistas de lectura por contexto lo simplifican); la migración de versión es un proceso con reglas de mapeo que hay que diseñar (F2, cuando exista una segunda versión con cambios reales); el `Platform Administrator` necesita un flujo de publicación de semilla con revisión humana.
- Riesgos: desalineación entre `seed_version` del dataset y `version_semilla` de la matriz de trazabilidad → regla de integridad 6 de `TRACEABILITY_MODEL.md`; identificadores mal transcritos → prueba de distribución y prueba de que cada `identifier` existe en `resumenes/02 §4.12`.
- Cambios OpenSpec que lo implementan: `standards-and-requirements-engine` (catálogo, semilla, `RequirementImplementation`, adopción por tenant), `soa-control-activation` (`SoAItem` → `Control`, `OrganizationControl`), `internal-audit-core` (criterios desde catálogo y SoA), `demo-tenant-seed` (fixtures derivados).

## Verificación

- Pruebas de dominio de la semilla (NFR-024, PRD §16.2.1): exactamente 38 controles con la distribución A.2:3, A.3:2, A.4:5, A.5:4, A.6:9, A.7:5, A.8:4, A.9:3, A.10:3; 26 términos; 11 objetivos y 7 fuentes del Anexo C; cero registros sin `record_type`; cero registros sin `SeedRecordSource`.
- Prueba de derechos de autor (addendum D.5): ningún n-grama ≥ 12 palabras coincide con `resumenes/` ni con la lista de literales.
- Prueba de integración: el rol de aplicación no puede escribir en el catálogo; publicar una versión no altera `requirement_implementations`.
- Prueba de arquitectura: ningún paquete importa `resumenes/`.

## Estado y aprobación

Propuesto. Ratifica el PRD §17 D-3 y toca la interpretación de qué es excluible (Anexo A) frente a no excluible (cláusulas 4-10, D-2); `CLAUDE.md` exige aprobación humana para cambios de interpretación normativa y para bloquear la arquitectura. Punto de aprobación: antes de `standards-and-requirements-engine` (Checkpoint 1).
