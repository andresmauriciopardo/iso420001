---
titulo: "Hoja de ruta única de la plataforma ISO/IEC 42001"
fecha: "2026-09-17"
estado: "borrador para revisión (checkpoint de planificación pendiente)"
autor: "Agente PRD (bmad-prd, headless) para Andrés Mauricio Pardo"
nivel_fuente_de_verdad: 2
fuentes:
  - docs/product/PROMPT_REVIEW_AND_ADJUSTMENTS.md (A1 MVP vertical, A2 hoja de ruta única, A13 checkpoint)
  - docs/product/MASTER_ORCHESTRATION_BMAD_OPENSPEC.md (P1 §17 fases 0-15, §37 orden, §38 primeros cambios, §35/§40 checkpoints)
  - docs/product/MASTER_ISO42001_PLATFORM_REQUIREMENTS.md (P2 §64 fases 0-12, §69)
  - docs/product/PRODUCT_SCOPE.md (MVP / Fase 2 / Fase 3; 23 cambios OpenSpec propuestos)
  - docs/product/DOMAIN_MAP.md (bounded contexts y eventos)
  - docs/product/REQUIREMENTS_ANALYSIS.md (IDs PR-*)
  - _bmad-output/planning-artifacts/prds/prd-ISO42001-2026-09-17/prd.md (FR/NFR, §17 decisiones pendientes)
---

# Hoja de ruta única

## 1. Principio

El ajuste **A2** exige una única hoja de ruta que reconcilie las 16 fases macro de P1 §17 con las 13 fases de P2 §64 y con los tres cortes (MVP, Fase 2, Fase 3) de `PRODUCT_SCOPE.md`. **Las fases de P1 son la referencia**; las de P2 se mapean sobre ellas; los tres cortes son *trenes de entrega* que agrupan fases completas o partes de fase.

El ajuste **A1** fija el criterio de corte: arquitectura de dominio completa desde el inicio (21 bounded contexts, todas las entidades, todo el catálogo de eventos) e implementación por cortes verticales. Una fase de P1 puede por tanto aparecer en dos trenes: su núcleo en el MVP y su ampliación en Fase 2 (marcado `MVP*` en `REQUIREMENTS_ANALYSIS.md`).

**Reglas de la hoja de ruta** (P1 §0.2, §37-38; `CLAUDE.md`):

1. Un cambio OpenSpec = una capacidad coherente e independientemente verificable; nunca un cambio "build-entire-platform".
2. El orden inicial es el de P1 §37; los cinco primeros cambios son los de P1 §38: `foundation-project-bootstrap`, `organization-tenancy`, `identity-and-rbac`, `immutable-audit-trail`, `standards-and-requirements-engine`. **Ninguna integración antes de la fundación de dominio.**
3. Ningún cambio se implementa antes del checkpoint de planificación aceptado (P1 §40 paso I, A13).
4. Cada cambio archivado actualiza `docs/compliance/TRACEABILITY_MATRIX.md` (`TRACEABILITY_MODEL.md` §5).
5. Los cambios marcados con ⚠ dependen de una decisión pendiente de aprobación humana del PRD §17; no se proponen (`/opsx:propose`) hasta que la decisión esté ratificada.

## 2. Correspondencia de fases

| Fase P1 §17 (referencia) | Fase P2 §64 que absorbe | Tren | Sección del PRD |
|---|---|---|---|
| 0 Entorno de desarrollo | 0 Arquitectura | MVP | §15.2 (BMAD, OpenSpec, ADR-001…005) |
| 1 Fundación | 1 Fundación | MVP | §6.1, §7 |
| 2 Motor de normas y requisitos | 2 Motor de normas | MVP | §6.2, §8 |
| 3 Organización, contexto, alcance | 3 AIMS Core (contexto, partes, alcance, política, objetivos, roles) | MVP | §6.3 |
| 4 Portafolio de IA | 4 Riesgo/Impacto (inventario y activos) | MVP núcleo; F2 grafo y descubrimiento; F3 agentes | §6.4 |
| 5 Riesgo e impacto | 4 Riesgo/Impacto (motor, evaluaciones, tratamientos, residual, impacto) | MVP núcleo; F2 tendencia, apetito, dimensiones, sugerencias IA | §6.5, §6.6 |
| 6 SoA y controles | 5 SoA/Controles | MVP; F2 pruebas de controles, excepciones | §6.8 |
| 7 Evidencia | 7 Evidencia/Documentos (evidencia) | MVP; F2 procedencia automatizada, `legal hold` | §6.9 |
| 8 Ciclo de vida y operaciones | 6 Ciclo de vida/Operaciones (tareas en MVP; ciclo de vida, incidentes, cambios, excepciones en F2) | MVP tareas/aprobaciones/notificaciones; F2 resto | §6.7, §6.11, §6.12 |
| 9 Políticas y documentos | 7 Evidencia/Documentos (documentos, políticas, generación) | MVP gestión documental (decisión D-1); F2 generación LLM | §6.10, FR-029 |
| 10 Integraciones | 8 Integraciones | MVP SDK + mock; F2 conectores reales y descubrimiento; F3 conectores futuros | §6.19 |
| 11 Auditoría y CAPA | 9 Auditoría/CAPA | MVP; F2 mejora continua | §6.13, §6.14 |
| 12 Informes | 10 Reporting | MVP motor + 4 esenciales; F2 resto del catálogo, packs, programación, dashboards por dominio; F3 constructor dinámico | §6.16, §6.17 |
| 13 Revisión por la dirección | 11 Revisión por la dirección / Preparación (revisión) | MVP | §6.15 |
| 14 Preparación para certificación | 11 (preparación, evidence pack, informe ejecutivo) | MVP checklist y estados; F2 Certification Readiness Pack y Audit Evidence Pack | §6.18 |
| 15 Seguridad, rendimiento, endurecimiento | 12 Hardening | MVP pruebas de seguridad, WCAG, observabilidad, E2E; F2 residencia efectiva, antimalware, carga, SAML, revisiones de acceso, rotación | §7, §16.2 |

## 3. MVP: orden de implementación y cambios OpenSpec

Orden conforme a P1 §37 (fundación → tenant → autenticación → RBAC → registro de auditoría → normas → requisitos → contexto → alcance → inventario → riesgo → impacto → SoA → controles → evidencia → workflow → … → informes → integraciones → preparación → endurecimiento), con dos desvíos justificados y registrados en el PRD §17: política/objetivos justo después de contexto/alcance (D-9, agrupación de P2 §64 fase 3) y los nombres de P1 §38 para los cinco primeros cambios (D-8), lo que parte `identity-tenancy-rbac` de `PRODUCT_SCOPE.md` en `organization-tenancy` + `identity-and-rbac` y renombra `audit-log-hash-chain` → `immutable-audit-trail` y `standards-engine-seed` → `standards-and-requirements-engine`. Resultado: **24 cambios** (los 23 de `PRODUCT_SCOPE.md` §3.3 con esa partición).

| # | Cambio OpenSpec | Fase P1 | Objetivo | Capacidades (PR-*) | FR/NFR del PRD | Criterio de salida | Depende de |
|---|---|---|---|---|---|---|---|
| 1 | `foundation-project-bootstrap` | 0-1 | Monorepo `apps/` + `packages/`, stack ADR-001, CI con escaneo de dependencias/SAST/secretos, i18n base ES/EN, design system accesible, observabilidad base | PR-NFR-003, 007 (base), 008 (base), 013, 022; PR-XC-012 (base); PR-EXT-009, 012 | NFR-003, NFR-009, NFR-013, NFR-018, NFR-019, NFR-022 | CI en verde con lint, typecheck, pruebas y escaneos; página vacía bilingüe desplegable en Docker; migración inicial aplicada | Checkpoint de planificación aceptado (P1 §40) |
| 2 | `organization-tenancy` | 1 | Tenant, organización, unidades, departamentos, ubicaciones, equipos, membresías, `data_region`, configuración del tenant | PR-XC-002, 003; PR-NFR-011 (campo); PR-EXT-009 | FR-001, FR-002, FR-006, NFR-015 | Prueba automática de RLS cross-tenant en verde; `TENANT_CREATED` en outbox | 1 |
| 3 | `identity-and-rbac` | 1 | Autenticación local + MFA TOTP + OIDC, sesiones, invitaciones, ciclo de vida de usuario, 12 roles, catálogo de permisos, autorización a nivel de objeto | PR-XC-001, 004, 005; PR-NFR-004; PR-EXT-007 | FR-003, FR-004, FR-005, NFR-002 | Pruebas de escalada de privilegios y BOLA en verde; matriz rol × permiso cargada ⚠ D-6 | 2 |
| 4 | `immutable-audit-trail` | 1 | Registro de auditoría append-only con cadena de hashes por tenant, consulta, verificación y exportación | PR-XC-006; PR-NFR-010; PR-DR-017 | FR-007, NFR-017 | Verificación de cadena en UI/API; prueba de que no existe update/delete; entrada por cada mutación de los cambios 2-3 | 3 |
| 5 | `domain-events-outbox-jobs` | 1 | Bus de eventos con outbox, catálogo de eventos de `DOMAIN_MAP.md` §5, cola pg-boss, `correlation_id`, versionado e historial estándar | PR-XC-007, 014, 016, 019; PR-XC-020 | FR-009, FR-012, NFR-012, NFR-014 | Evento publicado y consumido con reintento idempotente; Administration consume todos los eventos hacia el registro de auditoría | 4 |
| 6 | `standards-and-requirements-engine` | 2 | Modelo normativo versionado, semilla `packages/standards-seed/` con 42001 (cláusulas 4-10, 38 controles, Anexos B y C), 19011 y 23894 parciales, `record_type`, mapeos, `RequirementImplementation`, espacio de requisito | PR-CAP-001…008, 090 (semilla); PR-NFR-015, 020, 021; PR-DR-018, 019 | FR-015 a FR-021, FR-024, NFR-020, NFR-021, NFR-024 | Pruebas de dominio (38 controles con distribución, 26 términos, 11 objetivos, 7 fuentes) en verde; ningún texto literal; cada registro cita `resumenes/NN §sección` ⚠ D-2, D-3 | 5 |
| 7 | `aims-context-parties-scope` | 3 | Contexto 4.1 con roles respecto a la IA, partes interesadas 4.2, alcance 4.3 versionado y aprobado, centro de control 4.4 | PR-CAP-011…014; PR-DR-029 | FR-025 a FR-028 | Pasos 2-4 del E2E (§16.1) automatizados; `SCOPE_CHANGED` emitido | 6 |
| 8 | `aims-policy-objectives-roles` | 3, 9 | Política de IA como información documentada con ciclo de vida, gestión documental, RACI y comité, objetivos de la IA con métricas manuales | PR-CAP-015, 017, 018, 070; PR-DR-030 | FR-029, FR-030, FR-031, FR-079 | Política `Approved` y objetivo con responsable visibles en la preparación; `POLICY_APPROVED` emitido ⚠ D-1 | 7 |
| 9 | `ai-inventory-core` | 4 | Sistema de IA con ficha completa, modelo, versión, proveedor, dataset, despliegue; `Supplier`/`Customer`/`Contract` en esquema; disparadores `MODEL_VERSION_CHANGED`, `DATASET_CHANGED` | PR-CAP-020, 021; PR-DR-007 (emisión) | FR-034, FR-035, FR-036 | Paso 5 del E2E; cambio de versión emite evento consumido por riesgo e impacto ⚠ D-4 | 7 |
| 10 | `risk-engine-core` | 5 | Metodología aprobada, escalas 3×3/4×4/5×5, registro con Anexo C, inherente/residual, tratamiento con plan aprobado, aceptación con expiración, reevaluación por cambio de modelo | PR-CAP-030…038; PR-DR-005, 006, 007 (núcleo) | FR-041 a FR-047 | Paso 6 del E2E; pruebas unitarias de PR-DR-005/006; `RISK_ACCEPTANCE_EXPIRING` programado | 9 |
| 11 | `impact-assessment-core` | 5 | Plantillas, instancias, dimensiones semilla (safety ≠ security), hallazgos de impacto, versionado, vínculo al riesgo | PR-CAP-040…044; PR-DR-028 | FR-050 a FR-053 | Paso 7 del E2E; regla PR-DR-028 probada | 10 |
| 12 | `soa-control-activation` | 6 | Catálogo Anexo A en SoA, aplicabilidad, justificación obligatoria, completitud antes de aprobar, aprobación y versionado, activación/desactivación por evento, `ControlImplementation`, controles adicionales, informe de completitud | PR-CAP-050…056, 058, 060; PR-DR-001…004 | FR-059 a FR-067 | Pasos 8-9 del E2E; pruebas de PR-DR-001…004; 26 controles activan tareas y solicitudes; 12 no aplicables no generan nada | 10, 11 |
| 13 | `evidence-library` | 7 | Biblioteca de 15 tipos y 22 campos, estados, hash, versión, solicitudes, revisión con separación de funciones, vínculos, expiración programada, retención sin borrado físico, almacenamiento de objetos | PR-CAP-061…063, 065, 066, 068 (núcleo), 069; PR-DR-011, 015; PR-EXT-005 | FR-008, FR-071 a FR-075, FR-077, NFR-004 (MVP), NFR-016 | Paso 10 del E2E; PR-DR-011 probada; `EVIDENCE_EXPIRED` devuelve el control a `Needs Improvement` | 12 |
| 14 | `tasks-approvals-notifications` | 8 | Tareas desde todos los orígenes, workflow y aprobaciones genéricas, notificaciones in-app y correo con plantillas bilingües, comentarios | PR-CAP-075; PR-XC-008, 009, 017; PR-EXT-006 | FR-010, FR-082, FR-083, FR-084 | Toda aprobación de los cambios 7-13 pasa por `Approval`; `TASK_OVERDUE` notifica | 13 |
| 15 | `internal-audit-core` | 11 | Programa, auditoría con ciclo ISO 19011, criterios desde catálogo y SoA aprobada, evidencia por criterio, hallazgos configurables, conclusión derivada, informe, seguimiento, acceso auditor externo | PR-CAP-085…091; PR-DR-022, 023 | FR-087 a FR-093 | Pasos 11-12 del E2E; PR-DR-023 probada | 13, 14 |
| 16 | `capa-core` | 11 | `Finding` genérico, no conformidad, causa raíz, acción correctiva, verificación de eficacia obligatoria, cierre | PR-CAP-095…097; PR-DR-024 | FR-094 a FR-096 | Paso 13 del E2E; cierre sin verificación rechazado ⚠ D-4 | 15 |
| 17 | `management-review-core` | 13 | Revisión con 14 entradas agregadas y 8 salidas, acta, conversión de acciones en tareas | PR-CAP-100, 101 | FR-098, FR-099 | Paso 14 del E2E; `MANAGEMENT_REVIEW_APPROVED` crea tareas | 10, 15, 16 |
| 18 | `scoring-kpi-dashboard` | 12 | `ScoreSnapshot` transparente (6 puntuaciones), KPI de cumplimiento y riesgo, dashboard ejecutivo | PR-CAP-105 (núcleo), 106, 107; PR-DR-009, 010 | FR-100, FR-101, FR-102 | Prueba de UI: ninguna puntuación sin desplegable; prueba de cadenas: 0 apariciones de "certificado"/"compliance score" | 12, 13, 15, 16 |
| 19 | `reporting-essentials` | 12 | Motor declarativo, filtros, exportación PDF/CSV/JSON, servicio de renderizado, los 4 informes esenciales, exportación de documentos y actas | PR-CAP-110 (núcleo), 111, 072 (núcleo); PR-XC-011; PR-EXT-011 | FR-080, FR-105, FR-106 | Los 4 informes en ES y EN con versión de SoA y de norma; prueba de arquitectura "nuevo informe sin code path" | 18 |
| 20 | `certification-readiness-workspace` | 14 | Lista de 13 preguntas, 21 elementos documentales, estados de preparación explicados | PR-CAP-120; PR-DR-027 | FR-113 | Paso 15 del E2E; estado `Ready for internal review` alcanzado por la organización real | 17, 18, 19 |
| 21 | `connector-sdk-mock` | 10 | SDK de conectores, credenciales vía gestor de secretos, adaptador mock etiquetado, pruebas de contrato, `EvidenceProvenance` en modelo | PR-CAP-125, 131; PR-XC-015 (núcleo); PR-DR-021; PR-NFR-016; PR-EXT-008 | FR-114, FR-115, FR-116, FR-076 (modelo) | Estado `mock` visible; prueba de exposición de secretos; prueba de arquitectura "nuevo conector sin tocar dominio" | 13 (nunca antes del 6) |
| 22 | `search-traceability-navigation` | 12, 15 | Búsqueda léxica por proyección, paneles de trazabilidad bidireccional, "¿por qué es requerido?", navegación §37 sin botones falsos | PR-CAP-160, 165 (núcleo), 166 (núcleo), 175; PR-XC-010 | FR-021 (completo), FR-123, FR-125, FR-126, FR-131 | SM-5 medida en demo (< 2 min); prueba E2E de toda entrada visible con backend | 20 |
| 23 | `demo-tenant-seed` | 15 | Tenant "Demo Financial Services" etiquetado con datos derivados de `resumenes/` | PR-CAP-026; PR-DR-020 | FR-040 | Etiqueta "Demo" en toda pantalla; excluido de criterios de salida | 22 |
| 24 | `mvp-hardening` | 15 | Pruebas de seguridad completas, WCAG 2.2 AA automática, observabilidad, rendimiento, E2E Playwright de los 15 pasos, documentación viva, matriz de trazabilidad completa | PR-NFR-001, 003, 005 (diseño), 006, 007, 008, 014, 017, 018 | NFR-001, NFR-005 a NFR-007, NFR-009 a NFR-011, NFR-023; §16.2 | Los 10 criterios técnicos de §16.2 en verde; checkpoint humano de cierre del MVP | 23 |

**Hitos del MVP** (P1 §35): *Checkpoint 0* = planificación aceptada (antes del cambio 1) · *Checkpoint 1* = fundación (tras el 5) · *Checkpoint 2* = motor de normas y SGIA (tras el 8) · *Checkpoint 3* = SoA operativa con evidencia (tras el 13) · *Checkpoint 4* = flujo E2E completo (tras el 24).

## 4. Fase 2: SGIA conectado y asistido

**Objetivo.** Convertir el SGIA operable en un SGIA **conectado y asistido**: conectores reales, descubrimiento, ciclo de vida con puertas, incidentes, cambios, excepciones, IA asistida con autogobernanza, resto del catálogo de informes, formación, calidad de datos, importación y la parte diferida de los `MVP*`. 54 requisitos `PR-*` + partes diferidas (`PRODUCT_SCOPE.md` §5).

**Criterio de salida de la fase.** El flujo E2E del MVP se extiende con: conectar un proveedor real (al menos uno de Azure AI Foundry, OpenAI o Anthropic **validado contra la API real con credenciales de prueba y aprobación humana previa**), descubrir un candidato y validarlo, hacer avanzar un sistema por una puerta de despliegue bloqueada y desbloquearla con evidencia, registrar un incidente que dispare reevaluación de riesgo, generar una política con LLM y aprobarla tras revisión humana, emitir el Certification Readiness Pack; las funciones LLM aparecen en el inventario del tenant plataforma (PR-DR-026).

Cambios propuestos `[ASSUMPTION: nombres y agrupación del agente PRD; se fijan en épicas]`, en orden sugerido. El primer conector real depende del piloto (Q9): se propone el orden Azure → OpenAI → Anthropic solo como valor por defecto.

| # | Cambio OpenSpec | Fase P1 | Capacidades (PR-*) | FR/NFR | Criterio de salida | Depende de |
|---|---|---|---|---|---|---|
| F2-1 | `llm-provider-abstraction-budget` | 15 | PR-CAP-172; PR-NFR-019; PR-EXT-004 | FR-129 | Dos proveedores intercambiables en pruebas; `LLM_BUDGET_EXCEEDED` probado | MVP cerrado |
| F2-2 | `ai-assistance-engine` | 15 | PR-CAP-170, 171; PR-DR-008 | FR-127, FR-128 | Prueba: ningún servicio de asistencia escribe estados de cumplimiento; referencias normativas validadas contra el catálogo | F2-1 |
| F2-3 | `platform-self-governance` | 4 | PR-CAP-027; PR-DR-026 | FR-130 | Funciones LLM inventariadas en el tenant plataforma con evaluación de impacto e información a usuarios | F2-2 |
| F2-4 | `policy-document-generation` | 9 | PR-CAP-016, 071; PR-CAP-072 (DOCX/XLSX) | FR-081, FR-080 | Borrador `AI Generated` → `Approved` con revisor; exportación DOCX | F2-2 |
| F2-5 | `connector-azure-ai-foundry` ⚠ Q9 | 10 | PR-CAP-126, 129, 130; PR-EXT-001; PR-XC-015 (rotación) | FR-117, FR-120, FR-121, FR-116 | Estado `validated` solo tras prueba con credenciales reales; disponibilidad por dato visible | MVP cambio 21 |
| F2-6 | `connector-openai` ⚠ Q9 | 10 | PR-CAP-127; PR-EXT-002 | FR-118 | Idem | F2-5 |
| F2-7 | `connector-anthropic` ⚠ Q9 | 10 | PR-CAP-128; PR-EXT-003 | FR-119 | Idem | F2-5 |
| F2-8 | `ai-discovery-candidates` | 4, 10 | PR-CAP-024; PR-DR-012 | FR-039 | Candidato nunca pasa a inventario sin validación humana | F2-5 |
| F2-9 | `evidence-automated-provenance` | 7 | PR-CAP-064 | FR-076 | Evidencia automatizada sin procedencia completa no puede aceptarse | F2-5 |
| F2-10 | `ai-asset-graph` | 4 | PR-CAP-022; PR-CAP-165 (visualización) | FR-037, FR-125 | Grafo generado solo desde relaciones explícitas | MVP |
| F2-11 | `lifecycle-stages-gates` | 8 | PR-CAP-045…048; PR-DR-013 | FR-055 a FR-058 | Puerta bloqueada y desbloqueada con evidencia en E2E | MVP |
| F2-12 | `exceptions-management` | 8 | PR-CAP-135; PR-DR-014 | FR-070 | Ninguna excepción sin expiración | F2-11 |
| F2-13 | `incident-management` | 8 | PR-CAP-080, 081 | FR-085, FR-086 | Incidente significativo dispara reevaluación de riesgo e impacto | MVP |
| F2-14 | `aims-change-management` | 3, 8 | PR-CAP-019 | FR-032 | `MODEL_VERSION_CHANGED` propone solicitud de cambio | F2-11 |
| F2-15 | `control-testing` | 6 | PR-CAP-057 | FR-068 | `CONTROL_TEST_FAILED` recalcula el residual y crea `Finding` | MVP |
| F2-16 | `risk-trend-appetite-dimensions` | 5 | PR-CAP-033, 039, 043, 059; PR-CAP-031 (dimensiones) | FR-048, FR-049, FR-054, FR-069, FR-042 | Estado frente al apetito por riesgo y agregado; sugerencias marcadas | F2-2 |
| F2-17 | `reporting-catalog-and-packs` | 12, 14 | PR-CAP-112, 114, 115, 116 | FR-107 a FR-110 | 21 informes adicionales, Certification Readiness Pack y Audit Evidence Pack con hashes; programación | MVP |
| F2-18 | `domain-dashboards` | 12 | PR-CAP-108 | FR-103 | Dashboards solo con controles activos según la SoA | F2-17 |
| F2-19 | `evidence-analysis-ai` | 7 | PR-CAP-067 | FR-078 | Solo sugerencias; nunca cambia estado | F2-2 |
| F2-20 | `training-competence` | 3 | PR-CAP-140 | FR-033 | Brecha de competencia visible al expirar formación | MVP |
| F2-21 | `data-quality-engine` | 15 | PR-CAP-155 | FR-014 | `DATA_QUALITY_ISSUE_DETECTED` convertible en tarea/hallazgo | MVP |
| F2-22 | `import-export-full` | 15 | PR-CAP-150; PR-DR-016 | FR-013 | Importación con previsualización y duplicados; exportación XLSX/DOCX | MVP |
| F2-23 | `retention-legal-hold` | 15 | PR-XC-018 | FR-008 | `legal hold` bloquea expiración y borrado; borrado gobernado por flujo | MVP |
| F2-24 | `regulatory-requirements` ⚠ Q7 | 2 | PR-CAP-009; PR-EXT-015 | FR-022 | `REGULATORY_REQUIREMENT_CHANGED` marca riesgos y controles para revisión | MVP |
| F2-25 | `continual-improvement-engine` | 11 | PR-CAP-098 | FR-097 | Oportunidad de mejora con eficacia y cierre | MVP |
| F2-26 | `identity-hardening` | 1, 15 | PR-XC-001 (SAML), 004 (roles por unidad/sistema), 005 (revisiones de acceso) | FR-003, FR-004, FR-005 | Revisión de acceso con evidencia; SAML probado con un IdP | MVP |
| F2-27 | `phase2-hardening` ⚠ Q6 | 15 | PR-NFR-002, 005 (carga), 011 | NFR-004, NFR-006, NFR-015 | Antimalware activo; residencia efectiva por región; prueba de carga en verde | F2-5…F2-9 |

## 5. Fase 3: extensiones

**Objetivo.** Extensiones opcionales por bloques (10 requisitos `PR-*` + ABAC); cada una puede adelantarse si un cliente la exige sin afectar al núcleo.

| # | Cambio OpenSpec | Fase P1 | Capacidades (PR-*) | FR | Criterio de salida | Depende de |
|---|---|---|---|---|---|---|
| F3-1 | `ai-agents-governance` | 4 | PR-CAP-023 | FR-038 | Agente con herramientas, límites de gasto y *kill switch* gobernado como sistema de IA | F2-11 |
| F3-2 | `dynamic-report-builder` | 12 | PR-CAP-113 | FR-111 | Informe compuesto sin SQL arbitrario, guardado y programado | F2-17 |
| F3-3 | `semantic-search` | 12 | PR-CAP-161 | FR-124 | Búsqueda por embeddings respetando permisos | F2-1 |
| F3-4 | `domain-packages` | 2 | PR-CAP-010; PR-DR-025 | FR-023 | Paquete etiquetado como guía, nunca como requisito ISO | MVP |
| F3-5 | `maturity-model` | 12 | PR-CAP-145 | FR-104 | 12 dimensiones, 5 niveles, nunca "certificación" | F2-17 |
| F3-6 | `connector-dependent-reports` | 12 | PR-CAP-117 | FR-112 | Informes #19-#22 | F2-5…F2-9 |
| F3-7 | `future-connectors` | 10 | PR-CAP-132; PR-EXT-014 | FR-122 | Nuevo conector sin cambio de dominio | F2-5 |
| F3-8 | `abac-fine-grained-access` | 1 | PR-XC-004 (ABAC) | FR-004 | Políticas por atributo probadas sin regresión RBAC | F2-26 |
| — | Auditoría de tercera parte (ISO/IEC 42006) ⚠ Q8 | — | PR-EXT-013 | §12 | No se planifica hasta adquirir la norma | — |

## 6. Puntos de aprobación humana en la hoja de ruta

| Momento | Qué se aprueba | Fuente |
|---|---|---|
| Antes del cambio 1 | Checkpoint de planificación (informe de estado, PRD, arquitectura, decisiones §17 del PRD) | P1 §40 paso I, A13 |
| Antes del cambio 3 | Matriz rol × permiso por defecto (D-6) | PRD §17 |
| Antes del cambio 6 | Semántica de `Not Applicable` en requisitos (D-2, interpretación normativa) y separación catálogo/estado (D-3); revisión humana de la versión de semilla | PRD §17, `CLAUDE.md` |
| Antes del cambio 8 | Política y objetivos en MVP (D-1) | PRD §17 |
| Antes del cambio 9 y 16 | `Finding` genérico; `AIProvider`/`Supplier`/`Integration` (D-4) | PRD §17 |
| Antes de cada migración irreversible | La migración | `CLAUDE.md` |
| Antes de F2-5…F2-7 | Credenciales reales del proveedor; orden de conectores (Q9) | `CLAUDE.md`, brief Q9 |
| Antes de declarar cualquier requisito implementado | Evidencia en la matriz de trazabilidad | `CLAUDE.md`, `TRACEABILITY_MODEL.md` |
| Cierre del MVP | Los 10 criterios de PRD §16.2 | PRD §16 |

## 7. Riesgos de la hoja de ruta

- **Arrastre de decisiones pendientes**: D-2 y D-3 bloquean el cambio 6, que es el cuello de botella del MVP; deben resolverse en el checkpoint 0.
- **Corpus incompleto** (23894 al 23 %, 42006 ausente): el cambio 6 siembra lo disponible con `SOURCE_DETAIL_REQUIRED`; adquirir fuentes antes de F2-16 y de cualquier soporte a tercera parte.
- **Dependencia del piloto**: sin organización real (Q4) el MVP no puede cerrarse (SM-1); sin piloto tampoco se puede elegir el primer conector (Q9).
- **Ceremonia**: 24 cambios en el MVP es lo mínimo compatible con "un cambio = una capacidad"; agrupar más rompería la verificabilidad independiente; agrupar menos multiplica la ceremonia (R9 del PRD).
