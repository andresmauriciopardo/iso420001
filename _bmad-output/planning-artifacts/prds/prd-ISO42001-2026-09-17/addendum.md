---
title: "Addendum al PRD: Plataforma SaaS de gobernanza de IA ISO/IEC 42001"
status: draft
created: 2026-09-17
updated: 2026-09-17
project: ISO42001
purpose: "Profundidad que no cabe en el PRD y que consumirán arquitectura, UX, épicas y la matriz de trazabilidad."
---

# Addendum al PRD

Este documento conserva el detalle que respalda el PRD pero no pertenece a su narrativa principal: el índice inverso `PR-* → FR/NFR` (eslabón [2]→PRD de la trazabilidad), la propuesta de matriz rol × permiso (conflicto C15), el catálogo de enumeraciones canónicas, los mecanismos propuestos para decisiones que el PRD deja a arquitectura, las alternativas descartadas y el mapa de qué se tomó de cada insumo. No contiene decisiones de proceso ni auditoría: eso vive en `.memlog.md`.

## A. Índice inverso: requisito de producto (PR-*) → FR/NFR del PRD

Generado automáticamente a partir de las citas "Cubre PR-…" de `prd.md`, de la tabla de reglas de dominio (§9) y de la tabla de dependencias (§15). Cobertura: 205/205 identificadores de `REQUIREMENTS_ANALYSIS.md`. Cuando un PR aparece en varios FR, el primero es el principal. Este índice alimenta la columna `product_requirement` de `docs/compliance/TRACEABILITY_MATRIX.md` (`TRACEABILITY_MODEL.md` §4.2).

| PR-* | FR / NFR del PRD |
|---|---|
| PR-CAP-001 | FR-015 |
| PR-CAP-002 | FR-016 |
| PR-CAP-003 | FR-020 |
| PR-CAP-004 | FR-021 |
| PR-CAP-005 | FR-018 |
| PR-CAP-006 | FR-015 |
| PR-CAP-007 | FR-017 |
| PR-CAP-008 | FR-019 |
| PR-CAP-009 | FR-022 |
| PR-CAP-010 | FR-023 |
| PR-CAP-011 | FR-025 |
| PR-CAP-012 | FR-026 |
| PR-CAP-013 | FR-027 |
| PR-CAP-014 | FR-028 |
| PR-CAP-015 | FR-029 |
| PR-CAP-016 | FR-081 |
| PR-CAP-017 | FR-030 |
| PR-CAP-018 | FR-031 |
| PR-CAP-019 | FR-032 |
| PR-CAP-020 | FR-034 |
| PR-CAP-021 | FR-035, FR-036 |
| PR-CAP-022 | FR-037 |
| PR-CAP-023 | FR-038 |
| PR-CAP-024 | FR-039 |
| PR-CAP-026 | FR-040 |
| PR-CAP-027 | FR-130 |
| PR-CAP-030 | FR-043 |
| PR-CAP-031 | FR-006, FR-042 |
| PR-CAP-032 | FR-044 |
| PR-CAP-033 | FR-048 |
| PR-CAP-034 | FR-045 |
| PR-CAP-035 | FR-046 |
| PR-CAP-036 | FR-047 |
| PR-CAP-037 | FR-041 |
| PR-CAP-038 | FR-043 |
| PR-CAP-039 | FR-049 |
| PR-CAP-040 | FR-050 |
| PR-CAP-041 | FR-051 |
| PR-CAP-042 | FR-052 |
| PR-CAP-043 | FR-054 |
| PR-CAP-044 | FR-053 |
| PR-CAP-045 | FR-055 |
| PR-CAP-046 | FR-056 |
| PR-CAP-047 | FR-057 |
| PR-CAP-048 | FR-058 |
| PR-CAP-050 | FR-059 |
| PR-CAP-051 | FR-060 |
| PR-CAP-052 | FR-060 |
| PR-CAP-053 | FR-064 |
| PR-CAP-054 | FR-063 |
| PR-CAP-055 | FR-065 |
| PR-CAP-056 | FR-065 |
| PR-CAP-057 | FR-068 |
| PR-CAP-058 | FR-066 |
| PR-CAP-059 | FR-069 |
| PR-CAP-060 | FR-067 |
| PR-CAP-061 | FR-071 |
| PR-CAP-062 | FR-072 |
| PR-CAP-063 | FR-073 |
| PR-CAP-064 | FR-076 |
| PR-CAP-065 | FR-074 |
| PR-CAP-066 | FR-075 |
| PR-CAP-067 | FR-078 |
| PR-CAP-068 | FR-008 |
| PR-CAP-069 | FR-077 |
| PR-CAP-070 | FR-029, FR-079 |
| PR-CAP-071 | FR-081 |
| PR-CAP-072 | FR-080 |
| PR-CAP-075 | FR-082 |
| PR-CAP-080 | FR-085 |
| PR-CAP-081 | FR-086 |
| PR-CAP-085 | FR-087 |
| PR-CAP-086 | FR-088 |
| PR-CAP-087 | FR-089 |
| PR-CAP-088 | FR-090 |
| PR-CAP-089 | FR-091 |
| PR-CAP-090 | FR-024 |
| PR-CAP-091 | FR-092 |
| PR-CAP-095 | FR-094 |
| PR-CAP-096 | FR-095 |
| PR-CAP-097 | FR-096 |
| PR-CAP-098 | FR-097 |
| PR-CAP-100 | FR-098 |
| PR-CAP-101 | FR-099 |
| PR-CAP-105 | FR-101 |
| PR-CAP-106 | FR-100 |
| PR-CAP-107 | FR-102 |
| PR-CAP-108 | FR-103 |
| PR-CAP-110 | FR-105 |
| PR-CAP-111 | FR-067, FR-106 |
| PR-CAP-112 | FR-107 |
| PR-CAP-113 | FR-111 |
| PR-CAP-114 | FR-109 |
| PR-CAP-115 | FR-108 |
| PR-CAP-116 | FR-110 |
| PR-CAP-117 | FR-112 |
| PR-CAP-120 | FR-113 |
| PR-CAP-125 | FR-114 |
| PR-CAP-126 | FR-117 |
| PR-CAP-127 | FR-118 |
| PR-CAP-128 | FR-119 |
| PR-CAP-129 | FR-120 |
| PR-CAP-130 | FR-121 |
| PR-CAP-131 | FR-115 |
| PR-CAP-132 | FR-122 |
| PR-CAP-135 | FR-070 |
| PR-CAP-140 | FR-033 |
| PR-CAP-145 | FR-104 |
| PR-CAP-150 | FR-013 |
| PR-CAP-155 | FR-014 |
| PR-CAP-160 | FR-123 |
| PR-CAP-161 | FR-124 |
| PR-CAP-165 | FR-125 |
| PR-CAP-166 | FR-021, FR-126 |
| PR-CAP-170 | FR-127 |
| PR-CAP-171 | FR-128 |
| PR-CAP-172 | FR-129 |
| PR-CAP-175 | FR-131, NFR-011 |
| PR-XC-001 | FR-003 |
| PR-XC-002 | FR-001 |
| PR-XC-003 | FR-002 |
| PR-XC-004 | FR-004, FR-093 |
| PR-XC-005 | FR-005, FR-093 |
| PR-XC-006 | FR-007, NFR-017 |
| PR-XC-007 | FR-007 |
| PR-XC-008 | FR-083 |
| PR-XC-009 | FR-084 |
| PR-XC-010 | FR-123 |
| PR-XC-011 | FR-080 |
| PR-XC-012 | FR-006, FR-084, FR-132, NFR-018 |
| PR-XC-013 | FR-011 |
| PR-XC-014 | FR-012 |
| PR-XC-015 | FR-116 |
| PR-XC-016 | FR-009 |
| PR-XC-017 | FR-010 |
| PR-XC-018 | FR-008 |
| PR-XC-019 | FR-012, NFR-014 |
| PR-XC-020 | NFR-012 |
| PR-NFR-001 | NFR-001 |
| PR-NFR-002 | NFR-004 |
| PR-NFR-003 | NFR-003 |
| PR-NFR-004 | FR-002, NFR-002 |
| PR-NFR-005 | NFR-006 |
| PR-NFR-006 | NFR-007 |
| PR-NFR-007 | NFR-009 |
| PR-NFR-008 | NFR-013 |
| PR-NFR-009 | NFR-008 |
| PR-NFR-010 | FR-007, NFR-017 |
| PR-NFR-011 | FR-001, FR-006, NFR-015 |
| PR-NFR-012 | FR-008, NFR-016 |
| PR-NFR-013 | NFR-019 |
| PR-NFR-014 | NFR-005, NFR-023 |
| PR-NFR-015 | FR-018, NFR-024 |
| PR-NFR-016 | FR-115, NFR-024 |
| PR-NFR-017 | FR-131, NFR-011 |
| PR-NFR-018 | NFR-010 |
| PR-NFR-019 | FR-129 |
| PR-NFR-020 | FR-018, NFR-020 |
| PR-NFR-021 | NFR-021 |
| PR-NFR-022 | NFR-022 |
| PR-DR-001 | FR-061, FR-064, NFR-024, FR-067, FR-100 |
| PR-DR-002 | FR-064, NFR-024 |
| PR-DR-003 | FR-061, NFR-024 |
| PR-DR-004 | FR-062, NFR-024 |
| PR-DR-005 | FR-045, NFR-024, FR-044 |
| PR-DR-006 | FR-046, NFR-024 |
| PR-DR-007 | FR-047, NFR-024, FR-052 |
| PR-DR-008 | FR-128, NFR-024, FR-081 |
| PR-DR-009 | FR-100, FR-113, NFR-024 |
| PR-DR-010 | FR-100, NFR-024, FR-102 |
| PR-DR-011 | FR-074, NFR-024, FR-072 |
| PR-DR-012 | FR-039, NFR-024 |
| PR-DR-013 | FR-057, NFR-024 |
| PR-DR-014 | FR-070, NFR-024 |
| PR-DR-015 | FR-008, NFR-024 |
| PR-DR-016 | FR-013, NFR-024 |
| PR-DR-017 | FR-007, FR-027, NFR-024, FR-083 |
| PR-DR-018 | FR-017, NFR-024, FR-061 |
| PR-DR-019 | FR-018, NFR-024, FR-128 |
| PR-DR-020 | FR-040, FR-115, NFR-024 |
| PR-DR-021 | FR-115, NFR-024 |
| PR-DR-022 | FR-006, FR-090, NFR-024 |
| PR-DR-023 | FR-091, NFR-024 |
| PR-DR-024 | FR-096, NFR-024 |
| PR-DR-025 | FR-023, NFR-024 |
| PR-DR-026 | FR-130, NFR-024 |
| PR-DR-027 | FR-079, FR-113, NFR-024 |
| PR-DR-028 | FR-053, NFR-024 |
| PR-DR-029 | FR-025, NFR-024 |
| PR-DR-030 | FR-031, NFR-024 |
| PR-EXT-001 | FR-117, NFR-024 |
| PR-EXT-002 | FR-118, NFR-024 |
| PR-EXT-003 | FR-119, NFR-024 |
| PR-EXT-004 | FR-129, NFR-024 |
| PR-EXT-005 | NFR-024, FR-071, FR-002 |
| PR-EXT-006 | FR-084, NFR-024 |
| PR-EXT-007 | FR-003, NFR-024 |
| PR-EXT-008 | FR-116, NFR-024 |
| PR-EXT-009 | FR-002, NFR-024 |
| PR-EXT-010 | NFR-004, NFR-024 |
| PR-EXT-011 | NFR-003, NFR-024, FR-080 |
| PR-EXT-012 | NFR-013, NFR-024 |
| PR-EXT-013 | NFR-024 |
| PR-EXT-014 | FR-122, NFR-024 |
| PR-EXT-015 | FR-022, NFR-024 |

## B. Matriz rol × permiso por defecto (propuesta, conflicto C15) `[ASSUMPTION]`

P2 §4 enumera 12 roles y un catálogo de permisos, pero no dice qué rol tiene qué permiso. Esta propuesta es el punto de partida configurable de cada tenant (FR-004) y **requiere aprobación humana** (PRD §17 D-6). Leyenda: **X** = concedido; **(p)** = solo sobre objetos de los que es propietario; **(F2)** = disponible en Fase 2; vacío = no concedido. Roles: PA Platform Administrator · OA Organization Administrator · GM AI Governance Manager · RM Risk Manager · CM Compliance Manager · AU Auditor · SO AI System Owner · CO Control Owner · EO Evidence Owner · RV Reviewer · EX Executive · RO Read Only.

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

**Reglas transversales de la propuesta.** (1) `Platform Administrator` administra tenants y catálogo global pero **no** ve datos de negocio de los tenants por defecto; el acceso de soporte exige consentimiento del tenant y queda registrado `[ASSUMPTION]`. (2) `Auditor` no puede tener a la vez `controls.manage`, `evidence.upload` ni `evidence.approve` sobre el alcance que audita (independencia). (3) `Reviewer` nunca aprueba evidencia que subió (PR-DR-011). (4) `Read Only` invitado como auditor externo ve solo paquete de evidencia, SoA e informes, con caducidad (FR-093). (5) Las aprobaciones de alcance, política y revisión por la dirección recaen en `Executive` (alta dirección, 5.1-5.3, 9.3); las de plan de tratamiento y aceptación en `Risk Manager` y `Executive` (gerencia designada, 6.1.3).

## C. Catálogo de enumeraciones canónicas

Valores almacenados en inglés y traducidos por i18n (A5, `GLOSSARY.md` §1).

| Entidad / atributo | Valores | Fuente |
|---|---|---|
| Aplicabilidad (`SoAItem.applicability`) | `Applicable`, `Not Applicable`, `Applicable with Alternative Control`, `Pending Assessment` | P2 §13 |
| Estado de requisito (`RequirementImplementation.status`) | `Not Started`, `Planned`, `In Progress`, `Implemented`, `Partially Implemented`, `Not Applicable` (restringido, D-2), `Needs Review`, `Evidence Pending`, `Ready for Audit`, `Nonconforming` | P2 §6 |
| Ciclo del control (`ControlImplementation.status`) | `Not Applicable`, `Applicable`, `Planned`, `Implementing`, `Implemented`, `Operating`, `Needs Improvement`, `Ineffective`, `Retired` | P2 §14 |
| Estado de evidencia | `Requested`, `Missing`, `Collected`, `Under Review`, `Accepted`, `Rejected`, `Expired`, `Superseded` | P2 §15 |
| Tipo de evidencia | `document`, `screenshot`, `api_result`, `system_record`, `configuration`, `log`, `report`, `meeting_record`, `approval`, `training_record`, `assessment`, `audit_result`, `monitoring_metric`, `source_code_result`, `automated_scan` | P2 §15 |
| Documento / política | `Draft`, `AI Generated`, `Under Review`, `Pending Approval`, `Approved`, `Published`, `Superseded`, `Archived` | P2 §8 (C4: §20 es subconjunto) |
| Tipo de registro normativo | `normative_requirement`, `implementation_guidance`, `recommended_practice`, `evidence_example` | P2 §1, A6 |
| Clasificación de decisión | `organizational_decision`, `product_requirement`, `technical_design_decision` (además de los tipos normativos) | CLAUDE.md |
| Tratamiento del riesgo | `Avoid`, `Mitigate`, `Transfer/Share`, `Accept/Retain` | P2 §9 |
| Fase de auditoría | `Planned`, `Scheduled`, `Preparing`, `Opening`, `Fieldwork`, `Evidence Collection`, `Findings`, `Closing`, `Report`, `Follow-up`, `Closed` | P2 §23 |
| Clasificación de hallazgo (por defecto, configurable) | `Conformity`, `Nonconformity` (grado `major`/`minor` o 1-5), `Observation`, `Opportunity for Improvement` | P2 §23, ISO 19011 6.4.8 |
| `Finding.source_type` | `audit`, `control_test`, `impact`, `incident`, `data_quality`, `ai_gap_analysis` | DOMAIN_MAP §3.13 |
| Incidente (F2) | `Reported`, `Triaged`, `Investigating`, `Contained`, `Remediating`, `Resolved`, `Closed`, `Lessons Learned`; categorías: 13 de P2 §22 | P2 §22 |
| Etapas de ciclo de vida (F2) | `Idea`, `Assessment`, `Design`, `Development`, `Training`, `Validation`, `Approval`, `Deployment`, `Production`, `Operation & Monitoring` (D-5), `Change`, `Retirement` | P2 §12 |
| Estado de conector | `mock`, `configured`, `validated` | PR-DR-021 |
| Disponibilidad de dato (F2) | `Available`, `Not Available`, `Permission Required`, `Not Supported by Provider API`, `Not Configured` | P2 §16 |
| Error de conector (F2) | `Authentication Failed`, `Authorization Failed`, `Rate Limited`, `Provider Unavailable`, `Invalid Configuration`, `Unsupported Endpoint`, `Partial Sync`, `Data Mapping Error`, `Timeout`, `Unknown Error` | P2 §62 |
| Preparación para certificación | `Ready for internal review`, `Audit-ready`, `Certification preparation status` | P2 §53 |
| Puntuaciones | `Requirement Readiness`, `Evidence Coverage`, `Control Implementation`, `Control Effectiveness`, `Risk Exposure`, `Audit Readiness` | P2 §39 |
| Dimensiones de impacto | `individuals`, `groups`, `society`, `privacy`, `fairness`, `transparency`, `safety`, `security`, `human_rights`, `accessibility`, `environment`, `economic_social`, `automated_decisions`, `human_oversight`, `foreseeable_misuse` | P2 §10, C12 |
| Roles respecto a la IA | `ai_provider`, `ai_producer`, `ai_customer`, `ai_partner`, `ai_subject`, `relevant_authority` | 42001 4.1 nota 1 (`resumenes/01 §5`) |
| Madurez (F3) | `Initial`, `Developing`, `Defined`, `Managed`, `Optimized` | P2 §51 |
| Estado de fila de trazabilidad | `planned`, `in_progress`, `implemented`, `verified`, `unverified`, `gap`, `deferred(F2)`, `deferred(F3)`, `retired` | TRACEABILITY_MODEL §3 |

## D. Mecanismos propuestos para decisiones que el PRD deja a arquitectura

Ninguno es vinculante; se documentan para que `bmad-architecture` no parta de cero.

### D.1 Riesgo residual (FR-044, PRD §18.1)
Propuesta: `residual = inherent × (1 − Σ efficacy_i × weight_i)` acotado a la escala del tenant, donde `efficacy_i` toma 0 para controles `Planned`/`Implementing`/sin evidencia `Accepted`, un valor declarado (0-1) para `Implemented`/`Operating` con evidencia vigente, y el valor de la última `ControlTest` cuando existe (F2); `weight_i` lo asigna el `Risk Manager` por control mapeado (por defecto uniforme). La fórmula, numerador y exclusiones se exponen como `ScoreSnapshot` (FR-100). Alternativa descartada: matriz fija de reducción por número de controles (opaca y no configurable).

### D.2 Estados de preparación (FR-113, PRD §18.2)
Propuesta: `Ready for internal review` = 13/13 casillas de P2 §53 salvo "auditoría interna completada", "revisión por la dirección completada" y "CAPA cerradas"; `Audit-ready` = 13/13 y 21/21 elementos documentales con registro y evidencia `Accepted` donde aplica; `Certification preparation status` = etiqueta descriptiva permanente del panel (porcentaje de casillas con desglose), nunca un veredicto. Ninguna combinación produce la palabra "certificado".

### D.3 Recalculo de estado de requisito (FR-020, PRD §18.3)
Propuesta: los eventos consumidos por AIMS generan una *sugerencia de estado* con explicación ("todas las evidencias de los controles mapeados están aceptadas → sugerido `Ready for Audit`") y una tarea para el propietario; el cambio efectivo lo confirma el humano. Alternativa (cambio automático) descartada porque contradice "la IA/el sistema sugiere, el humano decide" aplicado a estados de cumplimiento y porque dificulta explicar el historial.

### D.4 Separación de funciones en tenants pequeños (FR-074, FR-096, PRD §18.8)
Propuesta: cuando solo exista un usuario con el permiso necesario, permitir la acción con una **excepción de separación de funciones** registrada (motivo obligatorio, evento específico, visible en la revisión por la dirección y en el informe de preparación). Alternativa (bloquear) descartada porque impediría a organizaciones muy pequeñas completar el flujo; alternativa (permitir en silencio) descartada por PR-DR-011.

### D.5 Detección de texto literal en la semilla (NFR-020)
Propuesta: prueba que compara n-gramas largos (≥ 12 palabras) de cada `requirement_summary` con los archivos de `resumenes/` y con una lista de frases marcadas como literales de la norma; coincidencia = fallo. Complemento: revisión humana registrada por versión de semilla.

### D.6 Cadena de hashes (FR-007)
Propuesta alineada con ADR-005: `hash_n = SHA-256(hash_{n-1} ‖ canonical_json(entry_n))` por tenant; ancla diaria del último hash exportable para custodia externa (F2) `[ASSUMPTION]`.

## E. Alternativas consideradas y descartadas en el PRD

| Alternativa | Por qué se descarta |
|---|---|
| Renumerar los 205 `PR-*` como FR del PRD | Rompe el eslabón [2] de la trazabilidad y los documentos de nivel 2 ya publicados; se opta por FR con cita a PR-* (memlog D1) |
| Sección de personas independiente | El skill prescribe personas inline en las trayectorias; evita "persona theater" |
| Diferir política y objetivos a Fase 2 (lista literal de A1) | 5.2 y 6.2 son información documentada obligatoria; sin ellas la preparación no puede completarse (D-1) |
| Permitir `Not Applicable` sin restricción en requisitos (P2 §6) | La norma no admite excluir cláusulas 4-10; expondría a hallazgos de certificación (D-2) |
| Tres entidades de hallazgo independientes | Triplica flujos de CAPA (D-4) |
| Cambio automático de estado de requisito por eventos | Contradice el principio "el sistema sugiere, el humano decide" aplicado al cumplimiento (D.3) |
| Un solo cambio OpenSpec "mvp-core" | Prohibido por P1 §0.2 y CLAUDE.md |
| Mantener los 23 nombres de PRODUCT_SCOPE | P1 §38 fija los cinco primeros nombres; se parte `identity-tenancy-rbac` (D-8) |
| Conectores reales en el MVP | A1/A9: SDK + mock primero; nunca "validado" sin credenciales reales |
| Vista multi-cliente para consultoras en el MVP | Sin validación de segmento (Q2); riesgo de fuga cross-tenant en la capa de agregación; F2/F3 |

## F. Mapa de insumos: qué se tomó de cada documento

| Insumo | Qué aporta al PRD | Dónde |
|---|---|---|
| Brief (`brief.md`) | Resumen ejecutivo, problema, visión y ciclo, segmentos, 7 diferenciadores, alcance MVP/diferido/fuera, 8 métricas con contramétricas, 10 riesgos, restricciones, 10 preguntas abiertas | §1-§3, §10, §12, §14, §17 (Q1-Q10) |
| Brief `addendum.md` | Panorama competitivo (referenciado), alternativas descartadas, mapa ciclo→fases, personas ampliadas (base de UJ), ADR-001…005, definiciones operativas de métricas, roadmap aparcado | §3, §4.3, §10, `ROADMAP.md` §4-5 |
| `REQUIREMENTS_ANALYSIS.md` | Los 205 `PR-*` con fase y prioridad; conflictos C1-C15; requisitos añadidos por A5-A10 | §6, §7, §9, §15, §17, addendum §A |
| `PRODUCT_SCOPE.md` | Principio A1, hoja de ruta única, corte MVP por bloques, 23 cambios OpenSpec, flujo E2E de 15 pasos, criterios técnicos, supuestos 1-8, fuera de alcance | §11, §13, §16, `ROADMAP.md` §3 |
| `GLOSSARY.md` | Terminología vinculante, trampas de traducción, términos de producto | §5, FR-132 |
| `DOMAIN_MAP.md` | 21 bounded contexts, propiedad de entidades, catálogo de eventos, decisiones abiertas 1-5 | §6 (encabezados de contexto y eventos), §8.6-8.7, §18.4-18.7 |
| `PROMPT_REVIEW_AND_ADJUSTMENTS.md` | Ajustes A1-A13 como decisiones organizacionales prevalentes | Transversal; §11, §17 |
| `TRACEABILITY_MODEL.md` | Cadena [0]-[9], convención de IDs, definición de hecho | §0, §16.3, addendum §A |
| `CLAUDE.md` | Reglas de dominio no negociables, idioma, aprobación humana | §3 principios, §9, §17 |
| P1 §13, §17, §37-38 | 24 áreas, 16 fases, orden e identificadores de los primeros cambios | §0, `ROADMAP.md` |
| P2 §4, §15, §21, §27, §30, §37-39, §41, §46, §53, §64, §69 | Roles y permisos, campos de evidencia, tareas, 25 informes, disparadores, navegación, puntuaciones, eventos, E2E, checklist, fases, aceptación | §6, §7, §16, addendum §B-C |
| `resumenes/01`, `resumenes/02` | Verificación de cláusulas 4-10, tabla de información documentada (21 filas), 38 controles y su distribución, roles respecto a la IA | §2, §8, FR-016, FR-018, FR-059, NFR-024 |
