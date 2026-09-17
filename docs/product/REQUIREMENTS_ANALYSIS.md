---
titulo: "Análisis y clasificación de requisitos de la plataforma ISO/IEC 42001"
fecha: "2026-09-17"
estado: "borrador para revisión"
autor: "Agente Analista de Requisitos (Claude) para Andrés Mauricio Pardo"
nivel_fuente_de_verdad: 2
fuentes:
  - docs/product/PROMPT_REVIEW_AND_ADJUSTMENTS.md (ajustes A1-A13; prevalecen sobre el prompt maestro)
  - docs/product/MASTER_ISO42001_PLATFORM_REQUIREMENTS.md (prompt maestro, §1-§73)
  - docs/product/MASTER_ORCHESTRATION_BMAD_OPENSPEC.md (§10 categorías de clasificación, §15 bounded contexts, §16 trazabilidad)
  - CLAUDE.md (reglas del dominio de cumplimiento)
  - resumenes/01_ISO-IEC-42001-2023_Requisitos_Clausulas-4-10.md (§1, §3, §5 y tabla de información documentada obligatoria)
  - resumenes/02_ISO-IEC-42001-2023_Anexo-A_Controles_y_Anexo-B_Guia-Implementacion.md (§3 y §4.12 mapa de los 38 controles)
  - resumenes/03_ISO-IEC-42001-2023_Anexos-C-D_Objetivos-Fuentes-de-Riesgo-y-Sectores.md (§3 lista C.2 y C.3)
  - resumenes/04_ISO-IEC-23894-2023_Gestion-de-Riesgos-de-IA.md (§5 términos)
  - resumenes/05_ISO-19011-2026_Directrices-Auditoria-Sistemas-de-Gestion.md (cláusula 3 y §5 términos)
  - resumenes/13_Glosario-Bilingue-ES-EN_ISO42001_y_Nota-Version-Inglesa.md (tablas 4.2, 4.4, 4.5)
---

# Análisis y clasificación de requisitos de la plataforma ISO/IEC 42001

## 0. Cómo leer este documento

Este análisis descompone el prompt maestro (`MASTER_ISO42001_PLATFORM_REQUIREMENTS.md`, en adelante **P2**) en requisitos atómicos con identificador estable, y aplica encima los ajustes A1-A13 de `PROMPT_REVIEW_AND_ADJUSTMENTS.md` (en adelante **A**). Donde P2 y A discrepan, prevalece A. Ningún requisito ISO se ha inventado: las cláusulas y controles citados existen en `resumenes/` y se referencian por archivo.

### Convenciones de identificador

| Prefijo | Categoría | Bloques de numeración |
|---|---|---|
| `PR-CAP-nnn` | Capacidad de producto | 001-010 motor de normas · 011-019 SGIA/contexto · 020-029 inventario de IA · 030-039 riesgo · 040-044 impacto · 045-049 ciclo de vida · 050-060 SoA y controles · 061-069 evidencia · 070-074 documentos y políticas · 075-079 tareas · 080-084 incidentes · 085-094 auditoría · 095-099 CAPA y mejora · 100-104 revisión por la dirección · 105-109 objetivos, KPI y puntuación · 110-119 informes · 120-124 preparación para certificación · 125-134 integraciones · 135-139 excepciones · 140-144 formación · 145-149 madurez · 150-154 importación/exportación · 155-159 calidad de datos · 160-164 búsqueda · 165-169 trazabilidad y explicación · 170-174 IA asistida · 175-179 UI/navegación |
| `PR-XC-nnn` | Capacidad transversal | secuencial |
| `PR-NFR-nnn` | Requisito no funcional | secuencial |
| `PR-DR-nnn` | Regla de dominio (invariante de negocio) | secuencial |
| `PR-EXT-nnn` | Dependencia externa | secuencial |

Los huecos de numeración son intencionales para poder insertar requisitos sin renumerar. Un ID nunca se reutiliza; un requisito retirado se marca `retirado` en lugar de borrarse.

### Columnas de las tablas

- **Enunciado**: una frase en forma "La plataforma debe…" o regla equivalente.
- **§**: sección de P2 de la que procede; `A<n>` cuando procede de un ajuste; `C.md` cuando procede de `CLAUDE.md`.
- **Tipo**: `PR` = `product_requirement`; `OD` = `organizational_decision`; `TDD` = `technical_design_decision`.
- **ISO**: cláusula(s) o control(es) de ISO/IEC 42001 relacionados **solo cuando la relación es evidente**, con el archivo de `resumenes/` entre corchetes: `[01]` cláusulas 4-10, `[02]` Anexo A/B, `[03]` Anexos C/D, `[04]` ISO/IEC 23894, `[05]` ISO 19011:2026, `[09]` plantilla SoA, `[10]` plantilla riesgo, `[11]` plantilla FODA, `[12]` plantilla stakeholders, `[13]` glosario. Un guion indica que no hay relación normativa directa (el requisito es de producto o técnico).
- **Fase**: `MVP`, `F2` (Fase 2) o `F3` (Fase 3) según el ajuste A1. Cuando el requisito se parte entre fases, se indica el núcleo que entra en MVP y se marca `MVP*`.
- **Prioridad**: MoSCoW (`Must`, `Should`, `Could`) **dentro de su fase**.
- `[ASSUMPTION]` marca inferencias del analista no explícitas en P2 ni en A.

---

## 1. Capacidades de producto (PR-CAP)

### 1.1 Motor de normas y requisitos (§1, §6, §35, §47, §57)

| ID | Enunciado | § | Tipo | ISO | Fase | Prioridad |
|---|---|---|---|---|---|---|
| PR-CAP-001 | Modelo de conocimiento normativo versionado con las entidades `Standard`, `StandardVersion`, `Clause`, `SubClause`, `Requirement`, `Annex`, `ControlDomain`, `Control`, `ControlGuidance`, `RequirementControlMapping`, `StandardRelationship`. | §1 | PR | Estructura de cláusulas 4-10 y anexos A-D [01 §3] [02 §3.1] | MVP | Must |
| PR-CAP-002 | Representar cada requisito de las cláusulas 4 a 10 (no solo los títulos) con ficha completa: identificador, norma, versión, cláusula, subcláusula, título, resumen, propósito, aplicabilidad, guía de implementación, evidencia esperada, rol responsable, relaciones y notas. | §6 | PR | 4.1 a 10.2 [01 §4] | MVP | Must |
| PR-CAP-003 | Estado de implementación por requisito con los valores canónicos `Not Started`, `Planned`, `In Progress`, `Implemented`, `Partially Implemented`, `Not Applicable`, `Needs Review`, `Evidence Pending`, `Ready for Audit`, `Nonconforming`. | §6 | PR | — | MVP | Must |
| PR-CAP-004 | Espacio de trabajo (página de detalle) por requisito que responde las 16 preguntas de §6 (qué, por qué, quién, evidencia, controles, riesgos, políticas, sistemas de IA afectados, tareas, estado, evidencia recogida, preguntas de auditoría, hallazgos, CAPA, preparación) sin obligar a recorrer cinco módulos. | §6, §66 | PR | 7.5 [01 §4] | MVP* | Should |
| PR-CAP-005 | Semilla determinista y versionada (`packages/standards-seed/`) con metadatos de ISO/IEC 42001, cláusulas 4-10, cada requisito representado en `resumenes/`, dominios y 38 controles del Anexo A, estructura de riesgo de ISO/IEC 23894, estructura de auditoría de ISO 19011, roles, escalas de riesgo, ciclo de vida, informes y dashboards de ejemplo; usa `SOURCE_DETAIL_REQUIRED` donde falte detalle. | §47, A4, A6 | PR | Anexo A: 38 controles [02 §3.2]; cláusula 3: 26 términos [13 §4.2] | MVP* | Must |
| PR-CAP-006 | `StandardVersion` soporta desde el inicio ISO/IEC 42001:2023 + Amd 1:2024, UNE-ISO/IEC 42001:2025 (adopción idéntica), ISO 19011:2026, ISO/IEC 23894:2023 (cobertura parcial marcada `SOURCE_DETAIL_REQUIRED`) y registra ISO/IEC 42006 como dependencia pendiente. | A7 | PR | [01 §1, §2] [05] [04 §8] | MVP | Must |
| PR-CAP-007 | Cada registro normativo lleva `record_type` ∈ {`normative_requirement`, `implementation_guidance`, `recommended_practice`, `evidence_example`} y se muestra diferenciado en la UI. | §1.9, A6 | PR | Distinción *shall*/*should* del Anexo B [13 §4.5] | MVP | Must |
| PR-CAP-008 | Relaciones navegables requisito ↔ control ↔ riesgo ↔ política/procedimiento ↔ tarea ↔ KPI ↔ pregunta de auditoría, mantenidas como mapeos explícitos y no como texto libre. | §6 | PR | 6.1.3 b)-e) (controles frente al Anexo A) [01 §4] | MVP* | Must |
| PR-CAP-009 | Módulo de requisitos regulatorios/externos que mapea `RegulatoryRequirement → Requirement ISO → Control → AISystem → Evidence` sin fijar una jurisdicción. | §35 | PR | 4.1 nota 2 (requisitos legales aplicables) [01 §4] | F2 | Should |
| PR-CAP-010 | Arquitectura de paquetes de dominio sectorial (riesgos, preguntas, plantillas de evidencia, controles recomendados, informes) etiquetados como guía organizacional, no como requisitos ISO. | §57 | PR | Anexo D (informativo) [03] | F3 | Could |

### 1.2 SGIA: contexto, partes interesadas, alcance, liderazgo, objetivos (§7, §8, §25 parcial)

| ID | Enunciado | § | Tipo | ISO | Fase | Prioridad |
|---|---|---|---|---|---|---|
| PR-CAP-011 | Registro del contexto de la organización: cuestiones internas y externas, contexto estratégico, tecnológico y regulatorio, panorama de IA, dependencias, fortalezas/debilidades, oportunidades/amenazas e historial de revisión; incluye los roles de la organización respecto a la IA. | §7 (4.1) | PR | 4.1 [01 §4] [11] | MVP | Must |
| PR-CAP-012 | Gestión de partes interesadas por tipo (empleados, clientes, reguladores, proveedores, socios tecnológicos, inversores, grupos afectados, otros) con necesidades, expectativas, influencia, requisitos pertinentes, necesidades de comunicación y fecha de revisión. | §7 (4.2) | PR | 4.2 [01 §4] [12] | MVP | Must |
| PR-CAP-013 | Diseñador de alcance del SGIA componible desde unidades de negocio, ubicaciones, sistemas de IA, procesos, productos, servicios, actividades del ciclo de vida y proveedores, con exclusiones justificadas, aprobación, versionado e historial de cambios. | §7 (4.3) | PR | 4.3 [01 §4] | MVP | Must |
| PR-CAP-014 | Centro de control del SGIA que muestra estado, objetivos, riesgos, controles, evidencia, auditorías y mejoras en una sola vista. | §7 (4.4) | PR | 4.4 [01 §4] | MVP | Should |
| PR-CAP-015 | Ciclo de vida de la política de IA con estados `Draft`, `AI Generated`, `Under Review`, `Pending Approval`, `Approved`, `Published`, `Superseded`, `Archived`, y registro de la política como información documentada. | §8 | PR | 5.2; A.2.2, A.2.4 [01 §4] [02 §4.12] | MVP | Must |
| PR-CAP-016 | Generador de política de IA asistido por LLM que usa contexto, alcance, objetivos, controles aplicables, postura de riesgo, regulación y principios de la organización, marcado como generado y sujeto a aprobación humana. | §8, §19, §20 | PR | 5.2; A.2.2 [02] | F2 | Should |
| PR-CAP-017 | Roles y responsabilidades del SGIA: matriz RACI, comité de gobernanza de IA y roles nominales (AI System Owner, Model Owner, Data Owner, Human Supervisor, Risk Owner, Control Owner, Evidence Owner, Auditor). | §8 | PR | 5.3; A.3.2 [01 §4] [02 §4.12] | MVP | Should |
| PR-CAP-018 | Objetivos de la IA (`AIObjective`, `AIObjectiveMetric`) coherentes con la política, medibles cuando sea posible, con plan (qué, recursos, responsable, plazo, evaluación) y disponibles como información documentada. | §7, §26, §69 | PR | 6.2 [01 §4]; A.6.1.2, A.9.3 [02 §4.12] | MVP | Should |
| PR-CAP-019 | Gestión de cambios del SGIA: versionado de objetos importantes y flujo `Change Request → Impact Analysis → Risk Reassessment → Impact Reassessment → Control Review → Approval → Implementation → Evidence`. | §33 | PR | 6.3 [01 §4]; B.6.1.3 control de cambios [02] | F2 | Should |

### 1.3 Inventario de IA y activos (§11, §18, §48, §49, §50, A9)

| ID | Enunciado | § | Tipo | ISO | Fase | Prioridad |
|---|---|---|---|---|---|---|
| PR-CAP-020 | Registro de sistema de IA con los atributos de §11: nombre, descripción, propósito de negocio, propietarios (negocio/técnico), proveedor, modelo y versión, despliegue, entorno, geografía, datos y clasificación, datos personales/sensibles, usuarios, partes afectadas, nivel de automatización, tipo de decisión, supervisión humana, uso previsto, uso prohibido, mal uso previsible, clasificación de riesgo e impacto, estado de ciclo de vida, dependencias, contratos, controles, incidentes, seguimiento y evidencia. | §11 | PR | A.4.2 (inventario de activos), A.9.4 (uso previsto), A.6.2.7 [02 §4.12] | MVP | Must |
| PR-CAP-021 | Activos relacionados como entidades propias: `AIAsset`, `AIModel`, `AIModelVersion`, `AIProvider`, `AIDeployment`, `AIUseCase`, `AIProject`, `AIDataset`, `AIDataSource`, `AIIntegration`. | §5, §11 | PR | A.4.2 a A.4.5 (recursos de datos, herramientas, sistema) [02 §4.12] | MVP* | Must |
| PR-CAP-022 | Grafo de relaciones de activos (proceso de negocio → sistema de IA → modelo, versión, dataset, proveedor, API, infraestructura, usuarios, propietario, riesgos, controles, evidencia, incidentes, contratos) con visualización gráfica. | §11 | PR | A.4.2 (diagramas de flujo de datos y arquitectura) [02] | F2 | Should |
| PR-CAP-023 | Agentes de IA como sistemas de IA de primera clase: agente, sub-agente, herramientas y permisos, memoria, nivel de autonomía, acciones, sistemas externos, aprobación humana, límites de gasto y ejecución, acciones prohibidas, kill switch, logs, modelo, prompts, versiones. | §50 | PR | A.9.2, A.9.3 (uso responsable, supervisión humana) [02] | F3 | Should |
| PR-CAP-024 | Motor de descubrimiento automático `CONNECT → DISCOVER → NORMALIZE → CLASSIFY → MATCH → CREATE/UPDATE ASSET → MAP REQUIREMENTS → MAP CONTROLS → IDENTIFY RISKS → REQUEST HUMAN VALIDATION`, que crea `CandidateAISystem` y solicita confirmación humana (alcance, propietario, uso previsto, datos, partes afectadas, riesgo, supervisión). | §18 | PR | A.4.2 [02] | F2 | Should |
| PR-CAP-026 | Tenant de demostración realista ("Demo Financial Services") con varios sistemas de IA de ejemplo (soporte al cliente, detección de fraude, decisión de crédito, propensión, asistente de código, clasificador documental, asistente generativo, agente), proveedores, riesgos alto/medio/bajo, unidades de negocio, controles, evidencia, incidentes, hallazgos, políticas, informes y revisión por la dirección, etiquetado como demo. | §48, §49 | PR | — | MVP | Should |
| PR-CAP-027 | Las funciones asistidas por LLM de la propia plataforma se registran como sistemas de IA en el inventario del tenant plataforma y quedan sujetas a los controles A.6, A.8 y A.9. | A9 | PR | A.6, A.8, A.9 [02 §3.2] | F2 | Should |

### 1.4 Riesgo de la IA (§9, A9)

| ID | Enunciado | § | Tipo | ISO | Fase | Prioridad |
|---|---|---|---|---|---|---|
| PR-CAP-030 | Registro de riesgos de IA con fuente de riesgo, evento, causa, consecuencia, partes afectadas, sistema de IA afectado, activo afectado, controles existentes, probabilidad, impacto, riesgo inherente, tratamiento, riesgo residual, propietario, fecha objetivo, aceptación, seguimiento y revisión. | §9 | PR | 6.1.2, 8.2 [01 §4, tabla riesgo vs impacto] [10] | MVP | Must |
| PR-CAP-031 | Escalas de puntuación configurables por tenant (3x3, 4x4, 5x5, personalizadas) sin un método universal codificado; dimensiones adicionales opcionales (severidad, velocidad, detectabilidad, confianza, incertidumbre). | §9 | PR | 6.1.2 a) (resultados coherentes, válidos y comparables) [01 §4] | MVP* | Must |
| PR-CAP-032 | Cálculo de riesgo inherente y riesgo residual a partir de la escala del tenant y de la eficacia de los controles mapeados. | §9 | PR | 6.1.3 (riesgo residual) [01 §5] | MVP | Must |
| PR-CAP-033 | Cálculo de tendencia de riesgo y de estado frente al apetito de riesgo (`Risk Appetite Status`) por riesgo y agregado. | §9 | PR | 6.1.1 nota 1 (apetito de riesgo) [01 §5] [04 §5] | F2 | Should |
| PR-CAP-034 | Opciones de tratamiento `Avoid`, `Mitigate`, `Transfer/Share`, `Accept/Retain`, con plan de tratamiento del riesgo (`RiskTreatmentPlan`) aprobado por la gerencia designada. | §9 | PR | 6.1.3 g), 8.3 [01 §4] | MVP | Must |
| PR-CAP-035 | Flujo de aceptación de riesgo que exige rol autorizado, justificación, fecha de expiración/revisión, riesgo residual y conserva historial de aprobaciones. | §9 | PR | 6.1.3 (aceptación de riesgos residuales por la gerencia designada) [01 §4, §5] | MVP | Must |
| PR-CAP-036 | Disparadores de reevaluación de riesgo ante cambio de modelo, versión, dataset, proveedor, caso de uso, despliegue, incidente, requisito regulatorio, degradación detectada o cambio del panorama de amenazas. | §9 | PR | 8.2 (a intervalos planificados o ante cambios significativos) [01 §4] [04] | MVP* | Should |
| PR-CAP-037 | Registro de la metodología y criterios de riesgo de la IA del tenant (criterios de aceptabilidad, escalas, apetito) como información documentada aprobada. [ASSUMPTION: P2 no lo nombra como entidad, pero §53 exige "Risk methodology approved?" y 6.1.2 exige conservar información documentada sobre el proceso.] | §9, §53 | PR | 6.1.1, 6.1.2 [01 §4, tabla información documentada] | MVP | Must |
| PR-CAP-038 | Catálogo semilla de fuentes de riesgo y objetivos del Anexo C (11 objetivos C.2.1-C.2.11 y 7 fuentes C.3.1-C.3.7) y de la estructura de ISO/IEC 23894, seleccionables al crear riesgos. | §1, §47 | PR | Anexo C [03 §3]; ISO/IEC 23894 [04] | MVP | Should |
| PR-CAP-039 | Sugerencia de riesgos por IA a partir de las características del sistema (etiquetada como sugerencia; nunca crea riesgos aprobados). | §19 | PR | — | F2 | Could |

### 1.5 Evaluación del impacto del sistema de IA (§10)

| ID | Enunciado | § | Tipo | ISO | Fase | Prioridad |
|---|---|---|---|---|---|---|
| PR-CAP-040 | Motor de evaluación de impacto con `AssessmentTemplate`, `AssessmentInstance`, `Question`, `Answer`, evidencia, hallazgos, severidad, mitigación, propietario, aprobación y revisión. | §10 | PR | 6.1.4, 8.4 [01 §4]; A.5.2, A.5.3 [02 §4.12] | MVP | Must |
| PR-CAP-041 | Plantilla semilla que cubre las dimensiones de §10: individuos, grupos, sociedad, privacidad, equidad, transparencia, protección (*safety*), seguridad (*security*), derechos humanos, accesibilidad, medio ambiente, impacto económico/social, decisiones automatizadas, supervisión humana y mal uso previsible. | §10 | PR | A.5.4, A.5.5 [02 §4.12]; 3.24 [13 §4.2] | MVP | Must |
| PR-CAP-042 | Versionado de las evaluaciones de impacto y vínculo a la etapa del ciclo de vida del sistema de IA en la que se realizaron. | §10 | PR | 8.4 (repetición ante cambios) [01 §4] | MVP* | Should |
| PR-CAP-043 | Asistencia IA para la evaluación de impacto: sugerencia de preguntas y áreas de impacto. | §19 | PR | — | F2 | Could |
| PR-CAP-044 | Los resultados de la evaluación de impacto quedan disponibles y enlazados para la evaluación de riesgo del mismo sistema. | §10 | PR | 6.1.4 último párrafo (deben considerarse en 6.1.2) [01 tabla riesgo vs impacto] | MVP | Should |

### 1.6 Ciclo de vida del sistema de IA (§12)

| ID | Enunciado | § | Tipo | ISO | Fase | Prioridad |
|---|---|---|---|---|---|---|
| PR-CAP-045 | Etapas de ciclo de vida configurables con la plantilla por defecto `Idea, Assessment, Design, Development, Training, Validation, Approval, Deployment, Production, Monitoring, Change, Retirement`. | §12 | PR | A.6.1.3, A.6.2.2-A.6.2.8 [02 §4.12] | F2 | Should |
| PR-CAP-046 | Puertas de ciclo de vida configurables con criterios de entrada, evidencia requerida, controles requeridos, revisión de riesgo, revisión de impacto, aprobación, criterios de salida y traza de auditoría (ejemplo: puerta de despliegue). | §12 | PR | A.6.2.5 (plan de despliegue, criterios de lanzamiento, aprobaciones) [02 §4.12] | F2 | Should |
| PR-CAP-047 | Bloqueo de la progresión de etapa si una puerta obligatoria está incompleta, salvo excepción autorizada registrada. | §12 | PR | A.6.2.5 [02] | F2 | Should |
| PR-CAP-048 | Registro de revisiones de ciclo de vida (`LifecycleReview`) por etapa. | §5, §12 | PR | A.6.2.6 [02] | F2 | Could |

### 1.7 Declaración de aplicabilidad y controles (§13, §14)

| ID | Enunciado | § | Tipo | ISO | Fase | Prioridad |
|---|---|---|---|---|---|---|
| PR-CAP-050 | Catálogo completo del Anexo A cargado desde la semilla: 9 dominios (A.2 a A.10) y 38 controles con la distribución A.2:3, A.3:2, A.4:5, A.5:4, A.6:9, A.7:5, A.8:4, A.9:3, A.10:3, con objetivo de dominio y orientación del Anexo B como `implementation_guidance`. | §13, §47 | PR | Anexo A y B [02 §3.2, §4.12] | MVP | Must |
| PR-CAP-051 | Módulo SoA de primera clase con, por control: identificador, dominio, título, propósito, aplicabilidad, seleccionado, justificación, riesgos, sistemas de IA, requisitos, estado de implementación, propietario, requisitos de evidencia, estado de evidencia, excepciones, riesgo residual, aprobación, fecha de vigencia y fecha de revisión. | §13 | PR | 6.1.3 f); 3.26 [01 §4] [13 §4.2] [09] | MVP | Must |
| PR-CAP-052 | Opciones de aplicabilidad `Applicable`, `Not Applicable`, `Applicable with Alternative Control`, `Pending Assessment`. | §13 | PR | 6.1.3 d)-f) [01 §4] | MVP | Must |
| PR-CAP-053 | Activación y desactivación de controles dirigida por eventos (`CONTROL_ACTIVATED`, `CONTROL_DEACTIVATED`) que enciende o apaga flujos, tareas, requisitos de evidencia, seguimiento y métricas del control. | §13, §65 | PR | 6.1.3, 8.1 [01 §4] | MVP | Must |
| PR-CAP-054 | Aprobación formal de la SoA (permiso `soa.approve`) con versiones y comparación entre versiones. [ASSUMPTION: el versionado explícito de la SoA no está en P2, pero se infiere de §33 "control applicability changed" y de 8.3 "plan actualizado".] | §4, §13 | PR | 6.1.3 f), 8.3 [01 §4] | MVP | Must |
| PR-CAP-055 | Espacio de trabajo operativo por control activo con objetivo, razón de ser, requisitos y riesgos mapeados, sistemas de IA, propietario, plan de implementación, tareas, evidencia, pruebas, métricas, excepciones, hallazgos, acciones correctivas, historial de auditoría, eficacia y próxima revisión. | §14 | PR | Anexo A (todos) [02] | MVP* | Should |
| PR-CAP-056 | Ciclo de vida del control con estados canónicos `Not Applicable`, `Applicable`, `Planned`, `Implementing`, `Implemented`, `Operating`, `Needs Improvement`, `Ineffective`, `Retired`. | §14 | PR | — | MVP | Must |
| PR-CAP-057 | Pruebas de controles (eficacia de diseño, implementación, eficacia operativa) con procedimiento, probador, fecha, muestra, evidencia, resultado, hallazgos y acciones correctivas. | §14 | PR | 9.1 [01 §4] | F2 | Should |
| PR-CAP-058 | Controles adicionales o alternativos definidos por la organización, fuera del Anexo A, incorporados a la SoA con la misma trazabilidad. | §13 | PR | 6.1.3 c) (controles adicionales); A.1 y 3.26 nota 1 [01 §4] [02 §3.1] [13 §4.2] | MVP | Should |
| PR-CAP-059 | Sugerencia de mapeo de controles por IA a partir de riesgos y sistemas. | §19 | PR | — | F2 | Could |
| PR-CAP-060 | Informe de completitud de la SoA que incluye los 38 controles, incluidos los no aplicables con su justificación. | §13 | PR | 6.1.3 f) [01 §4] | MVP | Must |

### 1.8 Evidencia (§15, §42, §44)

| ID | Enunciado | § | Tipo | ISO | Fase | Prioridad |
|---|---|---|---|---|---|---|
| PR-CAP-061 | Biblioteca de evidencia con 15 tipos (documento, captura, resultado de API, registro de sistema, configuración, log, informe, acta, aprobación, registro de formación, evaluación, resultado de auditoría, métrica de seguimiento, resultado de código, escaneo automatizado) y los 22 campos de §15. | §15 | PR | 7.5, 9.1 [01 §4, tabla información documentada] | MVP | Must |
| PR-CAP-062 | Estados de evidencia `Requested`, `Missing`, `Collected`, `Under Review`, `Accepted`, `Rejected`, `Expired`, `Superseded`. | §15 | PR | — | MVP | Must |
| PR-CAP-063 | Hash criptográfico y versión de toda evidencia (manual o automatizada) para integridad verificable. | §15 | PR | 7.5.3 (control de la información documentada) [01 §4] | MVP | Must |
| PR-CAP-064 | Procedencia de evidencia automatizada: conector, endpoint, hora de recogida, metadatos de la petición, identificador del objeto fuente, transformación, versión del colector y hash. | §15 | PR | — | F2 | Must |
| PR-CAP-065 | Solicitudes de evidencia (`EvidenceRequest`) con vencimiento y revisión formal (`EvidenceReview`) por revisor autorizado que acepta o rechaza con motivo. | §15 | PR | 9.1 [01 §4] | MVP | Must |
| PR-CAP-066 | Vínculos de evidencia (`EvidenceLink`) a requisitos, controles, riesgos, sistemas de IA, auditorías y evaluaciones de impacto. | §15 | PR | — | MVP | Must |
| PR-CAP-067 | Análisis de evidencia por IA: qué requisitos y controles soporta, brechas, inconsistencias e información obsoleta (solo sugerencias). | §19 | PR | — | F2 | Could |
| PR-CAP-068 | Retención, expiración, archivado, `legal hold` y flujo de borrado configurables por clase de evidencia y documento. | §42 | PR | 7.5.3 (retención y disposición) [01 §4] | MVP* | Should |
| PR-CAP-069 | Detección programada de evidencia obsoleta o próxima a expirar con evento `EVIDENCE_EXPIRED`. | §44, §65 | PR | — | MVP | Should |

### 1.9 Documentos y políticas (§20)

| ID | Enunciado | § | Tipo | ISO | Fase | Prioridad |
|---|---|---|---|---|---|---|
| PR-CAP-070 | Gestión de información documentada (`Document`, `DocumentVersion`, `Policy`, `PolicyVersion`, `Procedure`) con ciclo `Draft`, `Review`, `Approval`, `Published`, `Superseded`, `Archived`, control de versiones, aprobación y distribución. | §20 | PR | 7.5.1-7.5.3 [01 §4] | MVP | Must |
| PR-CAP-071 | Motor de generación de documentos con 15 plantillas (política de IA, gestión de riesgos, desarrollo, IA responsable, gobernanza de datos, seguridad, incidentes, ciclo de vida, supervisión humana, proveedores, evaluación de impacto, seguimiento, auditoría, evidencia, excepciones) alimentadas con datos reales de la organización. | §20 | PR | A.2.2, A.2.3 [02 §4.12] | F2 | Should |
| PR-CAP-072 | Exportación de documentos a PDF, DOCX, Markdown y HTML. | §20 | PR | — | MVP* | Should |

### 1.10 Tareas (§21)

| ID | Enunciado | § | Tipo | ISO | Fase | Prioridad |
|---|---|---|---|---|---|---|
| PR-CAP-075 | Tareas de cumplimiento originadas desde requisito, control, riesgo, evaluación de impacto, hallazgo, incidente, acción correctiva, revisión de política, solicitud de evidencia, puerta de ciclo de vida, revisión por la dirección o descubrimiento; campos de §21 (título, descripción, origen, propietario, revisor, prioridad, vencimiento, estado, dependencias, evidencia requerida, aprobación requerida, escalado, SLA) y recurrencia. | §21 | PR | 8.1 [01 §4] | MVP | Must |

### 1.11 Incidentes de IA (§22)

| ID | Enunciado | § | Tipo | ISO | Fase | Prioridad |
|---|---|---|---|---|---|---|
| PR-CAP-080 | Gestión de incidentes de IA con 13 categorías (comportamiento inesperado, salida incorrecta, sesgo, privacidad, seguridad, alucinación, calidad de datos, deriva, uso no autorizado, contenido inseguro, regulatorio, proveedor, fallo de supervisión humana) y ciclo `Reported, Triaged, Investigating, Contained, Remediating, Resolved, Closed, Lessons Learned`. | §22 | PR | A.8.4 (comunicación de incidentes), A.6.2.6 [02 §4.12] | F2 | Must |
| PR-CAP-081 | Un incidente significativo puede disparar reevaluación de riesgo, reevaluación de impacto, revisión de control, acción correctiva y notificación a la dirección. | §22 | PR | 8.2, 8.4, 10.2 [01 §4] | F2 | Should |

### 1.12 Auditoría interna alineada con ISO 19011 (§23)

| ID | Enunciado | § | Tipo | ISO | Fase | Prioridad |
|---|---|---|---|---|---|---|
| PR-CAP-085 | Programa de auditoría con planificación anual basada en riesgos, alcance, objetivos, criterios, recursos, competencia e independencia de auditores, métodos, auditorías remotas y combinadas. | §23 | PR | 9.2.2 [01 §4]; ISO 19011 cl. 5 y 3.5 [05] | MVP* | Must |
| PR-CAP-086 | Auditoría con plan y ciclo `Planned, Scheduled, Preparing, Opening, Fieldwork, Evidence Collection, Findings, Closing, Report, Follow-up, Closed`. | §23 | PR | 9.2 [01]; ISO 19011 cl. 6, 3.7 [05] | MVP | Must |
| PR-CAP-087 | Evidencia de auditoría vinculada a criterio, requisito, control, sistema de IA y hallazgo. | §23 | PR | ISO 19011 3.8, 3.10 [05] | MVP | Must |
| PR-CAP-088 | Hallazgos con clasificación configurable por organización, con valores por defecto `Conformity`, `Nonconformity`, `Observation`, `Opportunity for Improvement`, y graduación opcional (mayor/menor o numérica). | §23 | PR | ISO 19011 3.11, 6.4.8 [05 §5] | MVP | Must |
| PR-CAP-089 | Conclusión de auditoría derivada de hallazgos y evidencia reales, con trazabilidad `criterio → evidencia → hallazgo → conclusión → acción correctiva → seguimiento` e informe de auditoría. | §23 | PR | ISO 19011 3.12, 6.5, 6.7 [05] | MVP | Must |
| PR-CAP-090 | Banco de preguntas de auditoría por requisito y control (semilla; ampliable por IA en Fase 2). | §6, §19 | PR | — | MVP* | Could |
| PR-CAP-091 | Seguimiento de auditoría (`Follow-up`) que verifica correcciones y acciones correctivas del auditado. | §23 | PR | ISO 19011 6.7 [05 §5] | MVP | Should |

### 1.13 No conformidades, CAPA y mejora continua (§24, §56)

| ID | Enunciado | § | Tipo | ISO | Fase | Prioridad |
|---|---|---|---|---|---|---|
| PR-CAP-095 | Módulo CAPA que, por no conformidad, registra requisito, control, evidencia, hallazgo, corrección inmediata, causa raíz, acción correctiva, propietario, vencimiento, prueba de eficacia, verificación y cierre. | §24 | PR | 10.2 [01 §4]; 3.16, 3.17 [13 §4.2] | MVP | Must |
| PR-CAP-096 | Métodos de análisis de causa raíz: 5 porqués, Ishikawa y personalizado. | §24 | PR | 10.2 b) [01 §4] | MVP | Should |
| PR-CAP-097 | Verificación de la eficacia de la acción correctiva como paso obligatorio antes del cierre. | §24 | PR | 10.2 d) [01 §4] | MVP | Must |
| PR-CAP-098 | Motor de mejora continua con registros `Improvement Opportunity → Analysis → Action → Owner → Implementation → Effectiveness → Closure` alimentados por hallazgos, incidentes, cambios de riesgo, retroalimentación, regulación, seguimiento, revisión por la dirección y lecciones aprendidas. | §56 | PR | 10.1 [01 §4] | F2 | Should |

### 1.14 Revisión por la dirección (§25)

| ID | Enunciado | § | Tipo | ISO | Fase | Prioridad |
|---|---|---|---|---|---|---|
| PR-CAP-100 | Revisión por la dirección con 14 entradas (decisiones previas, objetivos, KPI, riesgos, impactos, auditorías, incidentes, no conformidades, CAPA, cambios regulatorios, cambios del portafolio, retroalimentación de partes interesadas, desempeño de proveedores, necesidades de recursos), 8 salidas (decisiones, acciones, recursos, objetivos, política, alcance, riesgos, mejora) y acta formal como información documentada. | §25 | PR | 9.3.1-9.3.3 [01 §4] | MVP | Must |
| PR-CAP-101 | Agregación automática de las entradas de la revisión desde los módulos de riesgo, auditoría, CAPA, incidentes, KPI y portafolio en la fecha de corte. | §25 | PR | 9.3.2 [01 §4] | MVP | Should |

### 1.15 Objetivos, KPI, puntuación y dashboards (§26, §29, §38, §39)

| ID | Enunciado | § | Tipo | ISO | Fase | Prioridad |
|---|---|---|---|---|---|---|
| PR-CAP-105 | Motor de KPI y métricas configurable con las familias de §26: cumplimiento, riesgo, portafolio de IA, evidencia, auditoría y formación. | §26 | PR | 9.1 [01 §4] | MVP* | Should |
| PR-CAP-106 | Motor de puntuación transparente para `Requirement Readiness`, `Evidence Coverage`, `Control Implementation`, `Control Effectiveness`, `Risk Exposure` y `Audit Readiness`, exponiendo fórmula, numerador, denominador, exclusiones, registros fuente y marca temporal. | §39 | PR | 9.1 [01 §4] | MVP | Must |
| PR-CAP-107 | Dashboard ejecutivo que responde de inmediato: preparación del SGIA, exposición al riesgo, eficacia de controles, cobertura de evidencia, portafolio, hallazgos abiertos, CAPA abiertas, incidentes, preparación para auditoría y tareas vencidas, con navegación de detalle. | §38 | PR | 4.4, 9.1 [01 §4] | MVP | Should |
| PR-CAP-108 | Dashboards por dominio (ejecutivo, ISO 42001, riesgo, inventario, ciclo de vida, gobernanza de datos, IA responsable, seguridad, terceros, auditoría, evidencia, incidentes, CAPA, formación, integraciones) que solo muestran controles y flujos activos según la SoA. | §29 | PR | — | F2 | Should |

### 1.16 Informes (§27, §28, §52)

| ID | Enunciado | § | Tipo | ISO | Fase | Prioridad |
|---|---|---|---|---|---|---|
| PR-CAP-110 | Motor de informes genérico (definición declarativa, sin un code path por informe) con filtros por rango de fechas, unidad de negocio, departamento, sistema de IA, proveedor, riesgo, dominio de control, propietario, ciclo de vida, estado y filtros personalizados, y exportación PDF, XLSX, CSV, JSON y HTML. | §27 | PR | — | MVP* | Must |
| PR-CAP-111 | Informes esenciales del MVP: Statement of Applicability Report (#3), ISO/IEC 42001 Clause Compliance Report (#2), AI Risk Register Report (#5) y Audit Readiness Report (#13). | §27, A1 | PR | 6.1.3 f), 9.1, 9.2 [01] | MVP | Must |
| PR-CAP-112 | Resto de los 25 informes de §27 (#1, #4, #6-#12, #14-#18, #23) con el mismo motor. | §27 | PR | — | F2 | Should |
| PR-CAP-113 | Constructor dinámico de informes (fuente, dimensiones, métricas, filtros, visualización, guardar, programar, compartir, exportar) sobre una abstracción de consulta, sin SQL arbitrario de usuario. | §28 | PR | — | F3 | Could |
| PR-CAP-114 | Programación de informes (diaria, semanal, mensual, trimestral, personalizada) con destinatarios usuarios/equipos/roles y entrega in-app, email o paquete descargable. | §52 | PR | — | F2 | Should |
| PR-CAP-115 | Certification Readiness Pack (#24) y Audit Evidence Pack (#25) con índice de requisitos, controles y evidencia y referencias trazables. | §27 | PR | 9.2, 7.5 [01] | F2 | Should |
| PR-CAP-116 | Resúmenes narrativos ejecutivos generados por IA en los informes, marcados como generados. | §19 | PR | — | F2 | Could |
| PR-CAP-117 | Informes dependientes de conectores, regulación, supervisión humana y cambios de modelo (#19, #20, #21, #22). | §27 | PR | A.8.5, A.9.3 [02] | F3 | Could |

### 1.17 Preparación para certificación (§53)

| ID | Enunciado | § | Tipo | ISO | Fase | Prioridad |
|---|---|---|---|---|---|---|
| PR-CAP-120 | Espacio de trabajo de preparación para certificación que calcula requisitos, controles, evidencia, riesgos, auditorías, hallazgos, CAPA y revisión por la dirección, y ofrece la lista de comprobación de 13 preguntas (contexto, alcance aprobado, política aprobada, metodología de riesgo aprobada, evaluaciones de riesgo e impacto completas, SoA aprobada, controles implementados, evidencia suficiente, seguimiento activo, auditoría interna, revisión por la dirección, CAPA cerradas) con estados `Ready for internal review`, `Audit-ready`, `Certification preparation status`. | §53 | PR | 4.3, 5.2, 6.1.2, 6.1.3, 6.1.4, 9.1, 9.2, 9.3, 10.2 [01 tabla información documentada] | MVP | Should |

### 1.18 Integraciones y conectores (§16, §17, §18, §61, §62, A9)

| ID | Enunciado | § | Tipo | ISO | Fase | Prioridad |
|---|---|---|---|---|---|---|
| PR-CAP-125 | Marco de conectores extensible (SDK) con `Integration`, `Provider`, `Credentials`, `Connection Test`, `Sync Configuration`, `Scheduler`, `Data Mapping`, `Evidence Mapping`, `Risk Mapping`, `Finding Mapping`, `Sync Logs`, `Errors`, sin acoplar la lógica de negocio a ningún proveedor. | §16 | PR | A.10.3 (proveedores) [02 §4.12] | MVP | Must |
| PR-CAP-126 | Conector Microsoft Azure / Azure AI Foundry que recupera, donde la API y los permisos lo permitan, proyectos, modelos, despliegues, versiones, endpoints, recursos, configuraciones, metadatos de seguimiento, evaluaciones, configuración de *safety*, artefactos de IA responsable, uso, logs, propietarios/etiquetas y relaciones. | §16 | PR | A.4.2, A.6.2.6, A.10.3 [02] | F2 | Should |
| PR-CAP-127 | Conector OpenAI para información de proveedor, cuenta, modelos, despliegues, uso y configuración expuesta por la API autorizada. | §16 | PR | A.10.3 [02] | F2 | Should |
| PR-CAP-128 | Conector Anthropic para información de organización, API y modelos donde el proveedor la exponga. | §16 | PR | A.10.3 [02] | F2 | Should |
| PR-CAP-129 | Estado de disponibilidad por dato y conector visible en la UI: `Available`, `Not Available`, `Permission Required`, `Not Supported by Provider API`, `Not Configured`. | §16 | PR | — | F2 | Must |
| PR-CAP-130 | Taxonomía de errores de conector (`Authentication Failed`, `Authorization Failed`, `Rate Limited`, `Provider Unavailable`, `Invalid Configuration`, `Unsupported Endpoint`, `Partial Sync`, `Data Mapping Error`, `Timeout`, `Unknown Error`) con guía de remediación. | §62 | PR | — | F2 | Must |
| PR-CAP-131 | Adaptador mock etiquetado para modo local/demo y pruebas de contrato del SDK de conectores; el MVP entrega SDK + mock, no proveedores reales. | §61, A1, A9 | PR | — | MVP | Must |
| PR-CAP-132 | Conectores futuros (AWS AI, Vertex AI, GitHub, Azure DevOps, Jira, ServiceNow, Datadog, Splunk, Purview, Entra ID, SharePoint, almacenamiento en nube, SIEM, ticketing, registros de modelos, plataformas ML) añadibles sin cambiar el modelo de dominio. | §16 | PR | — | F3 | Could |

### 1.19 Excepciones (§34)

| ID | Enunciado | § | Tipo | ISO | Fase | Prioridad |
|---|---|---|---|---|---|---|
| PR-CAP-135 | Excepciones formales sobre requisito o control con motivo, justificación de negocio, riesgo, control compensatorio, propietario, aprobador, expiración y fecha de revisión. | §34 | PR | 6.1.3 (justificación de exclusiones) [01]; B.2.2 excepciones a la política [02 §4.12] | F2 | Should |

### 1.20 Formación y competencia (§36)

| ID | Enunciado | § | Tipo | ISO | Fase | Prioridad |
|---|---|---|---|---|---|---|
| PR-CAP-140 | Gestión de competencia y formación: roles, requisitos de competencia mapeados a roles de IA, requisitos de formación, cursos, completitud, evidencia, expiración y brechas de competencia. | §36 | PR | 7.2, 7.3 [01 §4]; A.4.6 [02 §4.12] | F2 | Should |

### 1.21 Madurez (§51)

| ID | Enunciado | § | Tipo | ISO | Fase | Prioridad |
|---|---|---|---|---|---|---|
| PR-CAP-145 | Modelo de madurez opcional con 12 dimensiones y niveles `Initial, Developing, Defined, Managed, Optimized`, presentado como métrica de gestión y nunca como puntuación de certificación. | §51 | PR | — | F3 | Could |

### 1.22 Importación y exportación (§54)

| ID | Enunciado | § | Tipo | ISO | Fase | Prioridad |
|---|---|---|---|---|---|---|
| PR-CAP-150 | Importación con flujo `Upload → Validate → Preview → Map Fields → Detect Duplicates → Confirm → Import → Report` y exportación en CSV, XLSX, JSON, PDF, DOCX y Markdown; el MVP incluye solo exportación CSV/JSON de registros. | §54 | PR | — | F2 (MVP*: exportación) | Should |

### 1.23 Calidad de datos (§55)

| ID | Enunciado | § | Tipo | ISO | Fase | Prioridad |
|---|---|---|---|---|---|---|
| PR-CAP-155 | Motor de calidad de datos con comprobaciones de propietarios ausentes, controles huérfanos, riesgos sin tratamiento, controles sin evidencia, sistemas sin evaluación, evidencia expirada, relaciones de alcance ausentes, sistemas y proveedores duplicados, evaluaciones obsoletas; con dashboard propio. | §55 | PR | — | F2 | Should |

### 1.24 Búsqueda (§31)

| ID | Enunciado | § | Tipo | ISO | Fase | Prioridad |
|---|---|---|---|---|---|---|
| PR-CAP-160 | Búsqueda empresarial léxica sobre sistemas de IA, activos, requisitos, controles, riesgos, evidencia, políticas, auditorías, hallazgos, incidentes e informes, mostrando por qué coincidió cada resultado. | §31 | PR | — | MVP | Should |
| PR-CAP-161 | Búsqueda semántica (embeddings) sobre los mismos tipos. | §31, A1 | PR | — | F3 | Could |

### 1.25 Trazabilidad y explicación contextual (§32, §67)

| ID | Enunciado | § | Tipo | ISO | Fase | Prioridad |
|---|---|---|---|---|---|---|
| PR-CAP-165 | Grafo de trazabilidad navegable desde cualquier nodo (`Requirement → Control → Risk → AI System → Asset/Data/Model → Evidence → Audit → Finding → Corrective Action`) en ambos sentidos; el MVP entrega la navegación por enlaces y paneles de relación, la visualización gráfica llega en Fase 2. | §32, §73 | PR | — | MVP* | Should |
| PR-CAP-166 | Explicación contextual "¿por qué es requerido?" en cada objeto de cumplimiento (por qué existe el requisito, por qué se activó el control, qué riesgo lo seleccionó, qué sistemas afecta, qué evidencia lo demuestra, qué pasa si expira, qué criterio de auditoría lo probará); el MVP usa textos de la semilla, la IA generativa llega en Fase 2. | §67 | PR | — | MVP* | Should |

### 1.26 Motor de gobernanza asistida por IA (§19, A9)

| ID | Enunciado | § | Tipo | ISO | Fase | Prioridad |
|---|---|---|---|---|---|---|
| PR-CAP-170 | Motor de asistencia por IA con capacidades de interpretación de requisitos, análisis de brechas, sugerencia de riesgos, asistencia en impacto, mapeo de controles, generación de políticas, análisis de evidencia, preparación de auditoría, sugerencia de acciones correctivas y resúmenes ejecutivos. | §19 | PR | — | F2 | Should |
| PR-CAP-171 | Todo artefacto generado por IA lleva `generated_by_ai`, modelo, hora de generación, identificador de prompt/tarea, datos fuente, estado de revisión humana, revisor y aprobación. | §19 | PR | — | F2 | Must |
| PR-CAP-172 | Abstracción de proveedor de LLM independiente del dominio (Azure OpenAI, OpenAI, Anthropic intercambiables) con registro de tokens por llamada. | A9 | TDD | — | F2 | Must |

### 1.27 Navegación y UI (§37)

| ID | Enunciado | § | Tipo | ISO | Fase | Prioridad |
|---|---|---|---|---|---|---|
| PR-CAP-175 | Navegación principal según §37: Dashboard; AI Portfolio (sistemas, modelos, datasets, proveedores, ciclo de vida); ISO 42001 (requisitos, SoA, controles, políticas, objetivos); Risk; Impact; Evidence; Audit (programa, auditorías, hallazgos, CAPA); Operations (incidentes, tareas, cambios, excepciones); Reports; Integrations; Administration. Las entradas de módulos no implementados en la fase no se muestran (sin botones falsos). | §37, §61 | PR | — | MVP* | Should |

---

## 2. Capacidades transversales (PR-XC)

| ID | Enunciado | § | Tipo | ISO | Fase | Prioridad |
|---|---|---|---|---|---|---|
| PR-XC-001 | Autenticación fuerte con arquitectura compatible con MFA y preparada para SSO/OIDC/SAML, gestión de sesiones y cierre remoto. | §4, §40 | PR | — | MVP* (OIDC-ready; SAML en F2) | Must |
| PR-XC-002 | Modelo multi-tenant con `Organization`, `Tenant`, `BusinessUnit`, `Department`, `Location`, `User`, `Role`, `Permission`, `Team`, `Membership`. | §4 | PR | 4.3 (alcance por unidades y ubicaciones) [01] | MVP | Must |
| PR-XC-003 | Aislamiento de datos por tenant mediante `tenant_id` en toda tabla de negocio, Row Level Security obligatoria en PostgreSQL y prefijo de tenant en el almacenamiento de objetos (ADR-002). | §4, §40, A8 | TDD | — | MVP | Must |
| PR-XC-004 | RBAC con los 12 roles por defecto de §4 y el catálogo de permisos granulares (`organization.*`, `users.*`, `roles.manage`, `standards.*`, `requirements.*`, `controls.*` incl. `activate`/`deactivate`, `soa.*` incl. `approve`, `risks.*` incl. `approve`/`accept`, `assets.*`, `ai_systems.*`, `assessments.*`, `evidence.*` incl. `upload`/`approve`/`delete`, `audits.*` incl. `execute`, `reports.*`, `integrations.*` incl. `sync`, `policies.*` incl. `generate`/`approve`, `management_review.*`); roles a nivel de organización en MVP, a nivel de unidad de negocio y de sistema en Fase 2, ABAC opcional en Fase 3. | §4 | PR | 5.3 [01] | MVP* | Must |
| PR-XC-005 | Ciclo de vida de usuario: invitación, activación, desactivación, revisiones de acceso periódicas y principio de mínimo privilegio. | §4 | PR | — | MVP* (revisiones de acceso en F2) | Must |
| PR-XC-006 | Registro de auditoría de la aplicación append-only con cadena de hashes por tenant (ADR-005), que almacena actor, marca temporal, acción, objeto, estado anterior, estado nuevo, motivo, metadatos de IP/sesión y `correlation_id`, para al menos los eventos de §41. | §41, §63, A8 | PR | 7.5.3, 9.2 (evidencia auditable) [01] | MVP | Must |
| PR-XC-007 | Bus de eventos de dominio interno con patrón outbox (ADR-003) que publica el catálogo de §65 y los eventos de credenciales de §17. | §65, §17, A8 | TDD | — | MVP | Must |
| PR-XC-008 | Motor de workflow y aprobaciones genérico (`Workflow`, `Approval`, `Comment`) reutilizable por todos los contextos, con aprobaciones multinivel, delegación y registro de decisión. | §21, §5 | PR | — | MVP* | Must |
| PR-XC-009 | Motor de notificaciones por email, in-app y preparado para webhooks, con reglas configurables para los 13 disparadores de §30 (tareas vencidas, evidencia por expirar, riesgos altos, expiración de aceptaciones, puertas fallidas, incidentes, sincronizaciones fallidas, hallazgos, auditorías próximas, revisión por la dirección, expiración de políticas, revisión de controles, cambios regulatorios). | §30 | PR | 7.4 (comunicación) [01] | MVP* (in-app + email básicas) | Should |
| PR-XC-010 | Servicio de indexación de búsqueda al que cada contexto publica una proyección de sus entidades. | §31 | TDD | — | MVP | Should |
| PR-XC-011 | Servicio compartido de generación y renderizado de documentos (HTML → PDF, DOCX, Markdown) usado por informes, políticas y paquetes de evidencia. | §20, §27 | TDD | — | MVP* (PDF/HTML) | Should |
| PR-XC-012 | Internacionalización ES/EN desde la fundación: UI bilingüe, enumeraciones canónicas en inglés traducidas en la capa i18n, terminología oficial tomada de `resumenes/13` (*safety* = protección, *security* = seguridad, *accountability* = rendición de cuentas, *statement of applicability* = declaración de aplicabilidad). | A5 | PR | [13 §4.4, §4.5] | MVP | Must |
| PR-XC-013 | API documentada organizada por dominio (`/api/organizations` … `/api/management-reviews`, 27 recursos de §43) con esquemas tipados, errores consistentes, paginación, filtrado, ordenación, autorización y registro de auditoría. | §43 | PR | — | MVP | Must |
| PR-XC-014 | Procesamiento en segundo plano (pg-boss, ADR-004) para sincronización, descubrimiento, recogida de evidencia, informes, documentos, notificaciones, evaluaciones programadas, cálculo de KPI, reevaluación de riesgo y detección de evidencia obsoleta; jobs reintentables, idempotentes, observables y auditables. | §44, A8 | TDD | — | MVP | Must |
| PR-XC-015 | Gestión de credenciales de integración mediante abstracción de gestor de secretos, cifrado en reposo, referencias a secretos, rotación, scopes mínimos, prueba de conexión, registro de acceso, revocación y aislamiento por tenant, con eventos `CredentialCreated`, `CredentialUpdated`, `CredentialRotated`, `CredentialAccessed`, `CredentialRevoked`. | §17 | PR | — | MVP* (rotación en F2) | Must |
| PR-XC-016 | Versionado e historial de todo objeto importante (`version`, estados anteriores) con auditoría estándar `id, tenant_id, created_at, created_by, updated_at, updated_by, status, version, deleted_at`. | §5, §33 | TDD | — | MVP | Must |
| PR-XC-017 | Comentarios y colaboración contextual sobre cualquier objeto de cumplimiento. | §5 | PR | — | MVP | Could |
| PR-XC-018 | Política de retención, expiración, archivado, `legal hold` y borrado gobernado como servicio transversal por clase de documento y evidencia, sin borrado silencioso. | §42 | PR | 7.5.3 [01] | F2 (MVP*: sin borrado físico) | Should |
| PR-XC-019 | `correlation_id` en toda operación asíncrona y trazas distribuidas de extremo a extremo. | §45 | TDD | — | MVP | Must |
| PR-XC-020 | Manejo de errores uniforme con mensajes útiles y guía de remediación en toda operación. | §62 | PR | — | MVP | Must |

---

## 3. Requisitos no funcionales (PR-NFR)

| ID | Enunciado | § | Tipo | ISO | Fase | Prioridad |
|---|---|---|---|---|---|---|
| PR-NFR-001 | Seguridad de aplicación: cifrado en tránsito y en reposo, cabeceras seguras, protección CSRF, rate limiting, validación de toda entrada con Zod, codificación de salida, URLs de descarga firmadas y de corta vida, mínimo privilegio, diseño seguro de API. | §40, C.md | PR | 3.23 seguridad de la información [13 §4.2] | MVP | Must |
| PR-NFR-002 | Escaneo antimalware de archivos subidos y procesamiento de documentos aislado. | §40 | PR | — | F2 (MVP*: validación de tipo y tamaño) | Should |
| PR-NFR-003 | Cadena de suministro segura en CI: escaneo de dependencias, SAST y detección de secretos en cada cambio. | §40 | PR | — | MVP | Must |
| PR-NFR-004 | Nunca exponer al cliente claves de API, secretos de proveedor, credenciales internas, evidencia sensible ni datos de otro tenant. | §40, §17 | PR | — | MVP | Must |
| PR-NFR-005 | Escalabilidad de diseño para miles de sistemas de IA, decenas de miles de riesgos, cientos de miles de evidencias, historiales de auditoría grandes, muchos usuarios concurrentes y sincronizaciones programadas. | §60 | PR | — | MVP (diseño) / F2 (verificación con carga) | Must |
| PR-NFR-006 | Rendimiento: paginación y filtrado en servidor, caché justificada, generación asíncrona de informes, índices en `tenant_id`, `status`, `owner_id`, `requirement_id`, `control_id`, `risk_id`, `ai_system_id`, `created_at`, `due_date`. | §59, §60 | TDD | — | MVP | Must |
| PR-NFR-007 | Accesibilidad WCAG 2.2 AA, navegación completa por teclado, diseño responsive y UI densa pero legible. | §37, A9 | PR | — | MVP | Must |
| PR-NFR-008 | Observabilidad con OpenTelemetry: logs estructurados, métricas, trazas, seguimiento de jobs, salud de conectores y aplicación, seguimiento de errores. | §45 | PR | — | MVP | Must |
| PR-NFR-009 | Disponibilidad, copias de seguridad y recuperación ante desastres con RPO y RTO fijados en el PRD. [ASSUMPTION: valores objetivo iniciales RPO ≤ 1 h, RTO ≤ 8 h para el MVP, a validar.] | A9 | OD | — | MVP | Should |
| PR-NFR-010 | Auditabilidad de la aplicación: toda operación de gobernanza es atribuible, con marca temporal, trazable, revisable y exportable; el usuario puede responder "quién cambió la aplicabilidad de este control, cuándo, por qué, cuál era el valor anterior y quién lo aprobó". | §63 | PR | 9.2 [01] | MVP | Must |
| PR-NFR-011 | Residencia de datos por tenant (región de despliegue y almacenamiento) declarada en la configuración del tenant. | A9 | PR | — | F2 (MVP: diseño y campo `data_region`) | Should |
| PR-NFR-012 | Retención configurable por clase de registro y prohibición de borrado silencioso de registros de cumplimiento. | §42 | PR | 7.5.3 [01] | MVP | Must |
| PR-NFR-013 | Cambios de esquema solo por migración, con compatibilidad de semilla, consideración de reversión, pruebas, claves foráneas e índices; nunca modificación manual de producción. | §59 | TDD | — | MVP | Must |
| PR-NFR-014 | Pirámide de pruebas: unitarias (puntuación, cálculo de riesgo, aplicabilidad, permisos, transiciones, cálculos de informes), integración (BD, API, conectores, ingestión de evidencia, workflow), E2E de los 18 pasos de §46 y seguridad (acceso cross-tenant, escalada de privilegios, acceso no autorizado a evidencia, exposición de secretos, autorización rota a nivel de objeto). | §46 | PR | — | MVP | Must |
| PR-NFR-015 | Pruebas de dominio de cumplimiento con fixtures derivados de `resumenes/`: exactamente 38 controles con distribución A.2:3, A.3:2, A.4:5, A.5:4, A.6:9, A.7:5, A.8:4, A.9:3, A.10:3; 26 términos en la cláusula 3; 11 objetivos y 7 fuentes de riesgo en el Anexo C. | A9, C.md | PR | [02 §3.2] [13 §4.2] [03 §3] | MVP | Must |
| PR-NFR-016 | Pruebas de contrato de conectores contra el adaptador mock etiquetado; ninguna prueba afirma integración validada sin credenciales reales. | A9, §61 | PR | — | MVP | Must |
| PR-NFR-017 | Sin implementación solo mock: toda acción importante recorre `UI → API → Domain Service → Database → Audit Log`. | §61 | PR | — | MVP | Must |
| PR-NFR-018 | Calidad UX en cada módulo: estados vacíos, carga, error, paginación, accesibilidad y comportamiento responsive verificados antes de declararlo completo. | §68 | PR | — | MVP | Must |
| PR-NFR-019 | Coste y límites de uso de LLM: presupuesto por tenant, caché de respuestas, registro de tokens y bloqueo suave al superar el límite. | A9 | PR | — | F2 | Must |
| PR-NFR-020 | Derechos de autor: la semilla contiene solo identificador, título, paráfrasis propia, tipo, evidencia esperada y referencias; nunca texto literal de ISO/IEC/UNE; cada registro cita archivo y sección de `resumenes/`. | §1.8, A6 | OD | — | MVP | Must |
| PR-NFR-021 | Mantenibilidad y extensibilidad: nueva revisión de norma sin reescribir la aplicación, nuevo conector sin tocar el dominio, nuevo informe sin nuevo code path, nueva jurisdicción sin hardcoding. | §1.10, §16, §27, §35 | TDD | — | MVP | Must |
| PR-NFR-022 | Documentación viva en `docs/` (architecture, security, data model, API, integrations, compliance model, reporting, audit model, risk model, AI system model, deployment, operations, testing, threat model) según el árbol único del ajuste A3. | §58, A3 | OD | — | MVP | Should |

---

## 4. Reglas de dominio (PR-DR)

Las reglas de dominio son invariantes que el código debe hacer imposibles de violar, no preferencias de UI. Cada una debe tener al menos una prueba unitaria que la demuestre.

| ID | Enunciado | § | Tipo | ISO | Fase | Prioridad |
|---|---|---|---|---|---|---|
| PR-DR-001 | La SoA gobierna los flujos: un control `Not Applicable` no genera tareas operativas, no aparece como requisito activo en dashboards, no solicita evidencia recurrente ni entra en métricas de controles activos; pero permanece visible en el catálogo, su decisión y justificación son auditables y se incluye en los informes de completitud de la SoA. | §13, C.md | PR | 6.1.3 f); 3.26 [01 §4] [13 §4.2] | MVP | Must |
| PR-DR-002 | Cuando un control pasa a `Applicable`, se activan sus flujos, se crean las tareas requeridas y se activan requisitos de evidencia, seguimiento y métricas mediante evento. | §13 | PR | 6.1.3, 8.1 [01] | MVP | Must |
| PR-DR-003 | Un control `Not Applicable` exige justificación documentada; sin ella la transición se rechaza. | §13 | PR | 6.1.3 f) (justificación de exclusiones) [01] | MVP | Must |
| PR-DR-004 | Un control `Applicable` exige propietario, estado de implementación, expectativas de evidencia, riesgos mapeados y requisitos mapeados antes de aprobar la SoA. | §13 | PR | 6.1.3 d)-e); 3.26 nota 2 (todo riesgo y control reflejado en la SoA) [13 §4.2] | MVP | Must |
| PR-DR-005 | Un riesgo no se considera tratado por tener plan de tratamiento; requiere plan aprobado, controles implementados y riesgo residual evaluado y aceptado o dentro de criterios. | C.md, A | PR | 6.1.3 g), 8.3 [01] | MVP | Must |
| PR-DR-006 | La aceptación de riesgo requiere rol autorizado, justificación, riesgo residual y fecha de expiración/revisión; al aproximarse la expiración se emite `RISK_ACCEPTANCE_EXPIRING` y, vencida, el riesgo vuelve a estado de revisión requerida. | §9, §30, §65 | PR | 6.1.3 (aceptación por la gerencia designada) [01 §5] | MVP | Must |
| PR-DR-007 | Un cambio significativo (modelo, versión de modelo, dataset, proveedor, caso de uso, despliegue, incidente, requisito regulatorio, degradación detectada) marca el riesgo y la evaluación de impacto del sistema como "reevaluación requerida" y abre revisión de controles. | §9, §33, §65 | PR | 8.2, 8.4 (ante cambios significativos) [01] | MVP* (cambio de modelo/versión) | Must |
| PR-DR-008 | El contenido generado por IA se marca explícitamente como generado, requiere revisión y aprobación humana, y nunca cambia por sí mismo un estado de cumplimiento a implementado o conforme. | §8, §19, C.md | PR | — | F2 | Must |
| PR-DR-009 | La plataforma nunca declara que una organización está "certificada ISO"; usa `Readiness`, `Coverage`, `Implementation`, `Effectiveness`, `Exposure` y los estados `Ready for internal review`, `Audit-ready`, `Certification preparation status`. | §39, §53, C.md | PR | Certificación la ejecutan organismos acreditados [13 §4.4] | MVP | Must |
| PR-DR-010 | Toda puntuación expone fórmula, numerador, denominador, exclusiones, registros fuente y marca temporal; nunca se muestra un porcentaje único de "cumplimiento" sin explicación. | §38, §39 | PR | — | MVP | Must |
| PR-DR-011 | Evidencia subida no es evidencia aceptada: solo un revisor autorizado, distinto de quien la subió cuando sea posible, la pasa a `Accepted`. [ASSUMPTION: la separación de funciones subidor/revisor es inferencia de buenas prácticas de auditoría.] | §15, C.md | PR | ISO 19011 3.10 (evidencia verificable) [05] | MVP | Must |
| PR-DR-012 | El descubrimiento automático nunca marca cumplimiento; solo crea `CandidateAISystem` pendiente de validación humana. | §18 | PR | — | F2 | Must |
| PR-DR-013 | Un sistema de IA no avanza de etapa con puertas obligatorias incompletas salvo excepción autorizada y registrada. | §12 | PR | A.6.2.5 [02] | F2 | Must |
| PR-DR-014 | Ninguna excepción es permanente: toda excepción tiene expiración y fecha de revisión obligatorias. | §34 | PR | — | F2 | Must |
| PR-DR-015 | Ningún registro de cumplimiento se borra en silencio: el borrado pasa por flujo aprobado, respeta retención y `legal hold`, y deja rastro en el registro de auditoría. | §42, §54, C.md | PR | 7.5.3 [01] | MVP | Must |
| PR-DR-016 | Las importaciones nunca sobreescriben datos de cumplimiento sin previsualización, detección de duplicados y confirmación explícita. | §54 | PR | — | F2 | Must |
| PR-DR-017 | Toda decisión de cumplimiento (aplicabilidad, aceptación de riesgo, aprobación de evidencia o política, aprobación de SoA, alcance, hallazgo, cierre de CAPA, revisión por la dirección, cambio de permisos) genera un registro inmutable con actor, momento, motivo, valor anterior, valor nuevo y aprobador. | §5, §41, §63 | PR | 7.5.3, 9.2 [01] | MVP | Must |
| PR-DR-018 | La UI y los datos distinguen siempre `normative_requirement` de `implementation_guidance`, `recommended_practice` y `organizational_decision`; la orientación del Anexo B nunca se presenta como requisito ni exige justificación en la SoA. | §1, C.md | PR | B.1 [02 §3.1]; *shall* vs *should* [13 §4.5] | MVP | Must |
| PR-DR-019 | Nunca se inventan requisitos, controles, identificadores, texto normativo, obligaciones regulatorias ni capacidades de API; el detalle ausente se marca `SOURCE_DETAIL_REQUIRED` y se muestra como tal. | §1, §47, C.md | PR | — | MVP | Must |
| PR-DR-020 | Los datos de demostración y mock se etiquetan visiblemente y nunca representan cumplimiento real de una organización. | §49, §61 | PR | — | MVP | Must |
| PR-DR-021 | Nunca se afirma que un conector funciona hasta validarlo contra la API real del proveedor; el estado del conector distingue `mock`, `configured`, `validated`. | §61, C.md | PR | — | MVP | Must |
| PR-DR-022 | La clasificación y graduación de hallazgos de auditoría es configurable por organización; la plataforma no impone una única taxonomía. | §23 | PR | ISO 19011 6.4.8 (clasificación de no conformidades) [05 §5] | MVP | Should |
| PR-DR-023 | La conclusión de auditoría se deriva de hallazgos y evidencia registrados; no puede emitirse sin al menos un hallazgo o una declaración de conformidad por criterio evaluado. | §23 | PR | ISO 19011 3.12 [05] | MVP | Must |
| PR-DR-024 | Una CAPA solo se cierra tras registrar la verificación de eficacia de la acción correctiva. | §24 | PR | 10.2 d) [01] | MVP | Must |
| PR-DR-025 | Los paquetes sectoriales se etiquetan como guía organizacional o de dominio y nunca como requisitos ISO. | §57 | PR | Anexo D informativo [03] | F3 | Must |
| PR-DR-026 | Las funciones LLM de la plataforma son sistemas de IA registrados en su propio inventario, con evaluación de impacto, información a usuarios y uso responsable documentados. | A9 | OD | A.6, A.8, A.9 [02] | F2 | Should |
| PR-DR-027 | Cada elemento de la tabla de información documentada obligatoria (21 filas: alcance 4.3, SGIA 4.4, política 5.2, acciones 6.1.1, proceso de riesgo 6.1.2, controles y SoA 6.1.3, plan de tratamiento 6.1.3 g), resultado de impacto 6.1.4, objetivos 6.2, competencia 7.2, 7.5.1, evidencia operativa 8.1, resultados 8.2-8.4, seguimiento 9.1, auditorías 9.2.2, revisión 9.3.3, no conformidades 10.2) tiene un registro identificable en la plataforma y aparece en la lista de preparación. [ASSUMPTION: inferido de la tabla de `resumenes/01`; P2 no enumera esta lista.] | §53 | PR | Tabla de información documentada obligatoria [01 §4] | MVP | Must |
| PR-DR-028 | Los resultados de la evaluación de impacto de un sistema se consideran en su evaluación de riesgo: una evaluación de riesgo no puede marcarse completa si existe una evaluación de impacto posterior no revisada. | §10 | PR | 6.1.4 último párrafo [01 tabla riesgo vs impacto] | MVP | Should |
| PR-DR-029 | La organización declara sus roles respecto a la IA (proveedor, productor, cliente, socio, sujeto, autoridad pertinente) y estos condicionan la aplicabilidad sugerida de requisitos y controles. [ASSUMPTION: la nota 1 de 4.1 lo describe; el uso como filtro de aplicabilidad es inferencia.] | §7 | PR | 4.1 nota 1 [01 §5] [13 §4.2 nota] | MVP | Should |
| PR-DR-030 | Los objetivos de la IA deben ser coherentes con la política de IA, medibles cuando sea posible y tener responsable, recursos, plazo y método de evaluación. | §7 | PR | 6.2 [01 §4] | MVP | Should |

---

## 5. Dependencias externas (PR-EXT)

| ID | Enunciado | § | Tipo | ISO | Fase | Prioridad |
|---|---|---|---|---|---|---|
| PR-EXT-001 | Microsoft Azure / Azure AI Foundry: APIs de gestión y de proyectos para recuperar los datos de §16; no se asume disponibilidad de ningún dato hasta verificarlo. | §16 | PR | — | F2 | Should |
| PR-EXT-002 | OpenAI API (organización, modelos, uso, configuración). | §16 | PR | — | F2 | Should |
| PR-EXT-003 | Anthropic API (organización, modelos, uso). | §16 | PR | — | F2 | Should |
| PR-EXT-004 | Proveedor de LLM para las funciones asistidas (tras la abstracción PR-CAP-172); ningún proveedor está fijado por el dominio. | §19, A9 | TDD | — | F2 | Must |
| PR-EXT-005 | Almacenamiento de objetos compatible con S3 o Azure Blob para evidencia y documentos, con prefijo por tenant y URLs firmadas. | §2, §40 | TDD | — | MVP | Must |
| PR-EXT-006 | Servicio de correo transaccional para notificaciones e invitaciones. | §30 | TDD | — | MVP | Should |
| PR-EXT-007 | Proveedor de identidad externo vía OIDC (SAML en Fase 2); en MVP se admite además autenticación local. | §4 | TDD | — | MVP* | Should |
| PR-EXT-008 | Gestor de secretos (Azure Key Vault, AWS Secrets Manager o HashiCorp Vault) detrás de una abstracción; en local, un backend cifrado de desarrollo. | §17 | TDD | — | MVP | Must |
| PR-EXT-009 | PostgreSQL con Row Level Security como base de datos única del MVP (ADR-002). | A8 | TDD | — | MVP | Must |
| PR-EXT-010 | Motor antimalware (ClamAV o servicio gestionado) para archivos subidos. | §40 | TDD | — | F2 | Should |
| PR-EXT-011 | Bibliotecas de renderizado PDF/DOCX/XLSX con licencia compatible (no copyleft sin aprobación, A12). | §20, §27, A12 | TDD | — | MVP* | Should |
| PR-EXT-012 | Backend de observabilidad compatible con OpenTelemetry (colector y almacenamiento de trazas/métricas). | §45 | TDD | — | MVP | Should |
| PR-EXT-013 | ISO/IEC 42006 (requisitos para organismos que auditan SGIA): no está en el corpus; dependencia de conocimiento pendiente para auditoría de tercera parte. | A7 | OD | — | F3 | Could |
| PR-EXT-014 | Conectores futuros: AWS AI, Google Vertex AI, GitHub, Azure DevOps, Jira, ServiceNow, Datadog, Splunk, Microsoft Purview, Microsoft Entra ID, SharePoint, almacenamiento en nube, SIEM, ticketing, registros de modelos, plataformas ML. | §16 | PR | — | F3 | Could |
| PR-EXT-015 | Fuentes de requisitos regulatorios externos (feeds o carga manual), sin jurisdicción fijada. | §35 | PR | — | F2 | Could |

---

## 6. Secciones de P2 que no generan requisito de producto

Las siguientes secciones son instrucciones de proceso o entrega para el equipo, no requisitos de la plataforma. Se registran para que la trazabilidad sea completa.

| § | Contenido | Tratamiento |
|---|---|---|
| ROLE, §2 | Rol del agente e inspección del repositorio | Procedimiento; cubierto por P1 y CLAUDE.md. |
| §3 | Visión de producto y personas (CIO, CISO, CAIO, …) | Entrada del *product brief* BMAD; alimenta PRD. |
| §58 | Lista de documentos a producir | Reubicada por A3 en `docs/` (PR-NFR-022). |
| §64 | Fases de implementación (13) | Sustituidas por hoja de ruta única (A2) en `PRODUCT_SCOPE.md` y `ROADMAP.md`. |
| §68 | Puertas de calidad por módulo | Criterios de *definition of done* en OpenSpec `tasks.md` (PR-NFR-018). |
| §69 | Criterios de aceptación finales (29 pasos) | Acotados por A1 al flujo E2E del MVP en `PRODUCT_SCOPE.md`. |
| §70 | Principio de producto: operar el SGIA, no marcar casillas | Principio rector del PRD; no es requisito atómico. |
| §71, §72, §73 | Reglas de ingeniería, entrega y "no reduzcas el alcance" | Ajustadas por A1, A12, A13. |

---

## 7. Conflictos y ambigüedades detectadas

| # | Conflicto o ambigüedad | Fuentes | Resolución propuesta |
|---|---|---|---|
| C1 | **Alcance total vs. MVP.** P2 §73 prohíbe reducir el alcance; A1 define un MVP vertical. | §73, A1 | Se sigue A1: arquitectura de dominio completa (todos los contextos y entidades en el modelo) con implementación por fases. Este documento asigna fase a cada PR. |
| C2 | **Política de IA en el MVP.** La lista MVP de A1 no menciona políticas, pero la política de IA es información documentada obligatoria (5.2), aparece en la lista de preparación (§53) y en el flujo E2E de §69. | A1, §8, §53, [01 tabla] | El MVP incluye la gestión de la política como documento con ciclo de vida y aprobación (PR-CAP-015, PR-CAP-070). La **generación** por LLM (PR-CAP-016) se difiere a Fase 2. Se registra como decisión pendiente de confirmación humana. |
| C3 | **Objetivos de la IA.** Igual que C2: 6.2 exige objetivos documentados y §69 los incluye, pero A1 no los nombra. | A1, §7, [01 §4] | Se incluye PR-CAP-018 en MVP con prioridad Should (registro simple y vínculo a métricas); el motor de KPI completo es Fase 2. |
| C4 | **Dos ciclos de vida documentales.** §8 define 8 estados para la política (con `AI Generated`) y §20 define 6 estados para documentos en general. | §8, §20 | Un único ciclo canónico para `DocumentVersion` con los 8 estados de §8; `AI Generated` solo se usa cuando el origen es el motor de IA (Fase 2). Los 6 estados de §20 son un subconjunto. |
| C5 | **Estados de requisito con `Not Applicable`.** §6 permite marcar un requisito de las cláusulas 4-10 como `Not Applicable`, pero ISO/IEC 42001 no admite excluir requisitos de las cláusulas 4-10 para declarar conformidad (solo los controles del Anexo A admiten exclusión justificada). | §6, [01 §1] [02 §3.1] | `Not Applicable` se conserva en la enumeración por compatibilidad, pero la UI lo restringe a requisitos de tipo `implementation_guidance` y a normas de apoyo; para `normative_requirement` de 42001 se exige justificación y se marca advertencia. [ASSUMPTION] pendiente de confirmación por interpretación normativa (punto de aprobación humana). |
| C6 | **Nomenclatura de estados de evidencia de auditoría.** §23 usa `Evidence Collection` como fase de la auditoría, y §15 usa `Collected` como estado de la evidencia; ambos conviven con `AuditEvidence` (entidad §5). | §15, §23, §5 | `AuditEvidence` es un vínculo entre `Audit` y `Evidence` con el criterio evaluado; no duplica el archivo. Se documenta en `DOMAIN_MAP.md`. |
| C7 | **`Finding` vs `AuditFinding` vs `ImpactAssessmentFinding`.** §5 lista tres entidades de hallazgo sin definir su relación. | §5, §23, §24 | `Finding` es la entidad genérica (propiedad del contexto CAPA) con `source_type`; `AuditFinding` e `ImpactAssessmentFinding` son especializaciones que la referencian. [ASSUMPTION] |
| C8 | **Fases incompatibles.** P1 §17 (16 fases) y P2 §64 (13 fases). | A2 | Hoja de ruta única en `PRODUCT_SCOPE.md`: MVP / Fase 2 / Fase 3 con mapeo a ambas listas. |
| C9 | **"Immutable audit records" no definido.** | §5, §41 | A8 lo interpreta como tabla append-only con cadena de hashes (ADR-005). |
| C10 | **Conectores en el flujo de aceptación.** §69 exige "Connect Azure/OpenAI/Anthropic → Discover → Validate" como parte del flujo completo; A1 difiere los conectores reales. | §69, A1 | El flujo E2E del MVP sustituye estos pasos por "inventariar manualmente" y prueba el SDK con el adaptador mock; los pasos vuelven al flujo en Fase 2. |
| C11 | **Cobertura parcial de ISO/IEC 23894.** P2 pide sembrar la "estructura de riesgo de 23894"; el corpus solo cubre cláusulas 1-4 y 5.1-5.3 (23 %). | §47, A7, [04 §8] | Se siembra lo disponible; el resto lleva `SOURCE_DETAIL_REQUIRED`. La taxonomía de fuentes de riesgo se toma del Anexo C de 42001 [03], que sí está completo. |
| C12 | **Terminología ES.** P2 en inglés usa *safety* y *security* como dimensiones separadas de impacto (§10); la traducción automática del corpus los confunde. | §10, A5, [13 §4.5] | Enumeraciones canónicas en inglés (`safety`, `security`); etiquetas ES "protección (safety)" y "seguridad (security)". |
| C13 | **"Monitoring" como etapa de ciclo de vida y como proceso.** §12 lo usa como etapa; 9.1 y A.6.2.6 como actividad continua. | §12, [01], [02] | Etapa `Monitoring` del ciclo de vida se renombra internamente `Operation & Monitoring` en la plantilla por defecto para alinearse con A.6.2.6 (etiqueta ES "operación y seguimiento"). [ASSUMPTION] |
| C14 | **Ubicación de Supplier/Customer/Contract.** §5 los lista sin módulo propio; §16 habla de proveedores tecnológicos (conectores) y A.10 de proveedores y clientes como terceros. | §5, §16, [02 A.10] | `AIProvider` (proveedor tecnológico de modelos) y `Supplier` (tercero contractual, A.10.3) son entidades distintas, ambas en el contexto AI Portfolio; `Integration`/`Provider` del hub de conectores es otra cosa. Ver `DOMAIN_MAP.md`. |
| C15 | **Permisos sin rol asignado.** §4 lista permisos como `controls.activate` pero no dice qué rol por defecto los tiene. | §4 | La matriz rol × permiso se fija en el PRD; propuesta inicial: `controls.activate/deactivate` y `soa.approve` para Compliance Manager y AI Governance Manager; `risks.accept` para Risk Manager y Executive. [ASSUMPTION] |

---

## 8. Requisitos añadidos por los ajustes A5-A10

Estos requisitos no están en P2 y proceden íntegramente de `PROMPT_REVIEW_AND_ADJUSTMENTS.md`.

| Ajuste | Requisitos generados | Resumen |
|---|---|---|
| A5 Idioma y bilingüismo | PR-XC-012 | UI bilingüe ES/EN desde la fundación; enumeraciones canónicas en inglés; glosario oficial `resumenes/13`. |
| A6 Derechos de autor | PR-NFR-020, PR-CAP-007 | Semilla solo con paráfrasis y tipos de registro; cita a `resumenes/`. |
| A7 Versionado normativo | PR-CAP-006, PR-EXT-013 | `StandardVersion` multi-norma (42001 + Amd 1, UNE 2025, 19011:2026, 23894:2023); 42006 pendiente. |
| A8 Decisiones técnicas por ADR | PR-XC-003, PR-XC-006, PR-XC-007, PR-XC-014, PR-EXT-009 | RLS, monolito modular con outbox, pg-boss, cadena de hashes, stack por defecto. |
| A9 La plataforma es un sistema de IA | PR-CAP-027, PR-DR-026 | Las funciones LLM se inventarían y quedan bajo A.6, A.8, A.9. |
| A9 Accesibilidad | PR-NFR-007 | WCAG 2.2 AA explícita. |
| A9 Residencia, copias, DR, retención | PR-NFR-009, PR-NFR-011, PR-XC-018 | Región por tenant, RPO/RTO en PRD, `legal hold`. |
| A9 Coste LLM y abstracción de proveedor | PR-NFR-019, PR-CAP-172, PR-EXT-004 | Presupuesto por tenant, caché, tokens; sin acoplar el dominio. |
| A9 Pruebas de dominio | PR-NFR-015 | Fixtures de `resumenes/`: 38 controles, 26 términos, 11 objetivos, 7 fuentes. |
| A9 Pruebas de contrato de conectores | PR-NFR-016, PR-CAP-131 | Mock etiquetado; nunca "validado" sin credenciales reales. |
| A10 Nodo "resumen fuente" en trazabilidad | Ver `TRACEABILITY_MODEL.md` | Cada requisito sembrado se remonta a `resumenes/NN, sección`. |

---

## 9. Recuento

| Categoría | Requisitos | MVP (incl. MVP*) | Fase 2 | Fase 3 |
|---|---|---|---|---|
| PR-CAP | 118 | 73 | 38 | 7 |
| PR-XC | 20 | 19 | 1 | 0 |
| PR-NFR | 22 | 19 | 3 | 0 |
| PR-DR | 30 | 23 | 6 | 1 |
| PR-EXT | 15 | 7 | 6 | 2 |
| **Total** | **205** | **141** | **54** | **10** |

Nota: los requisitos marcados `MVP*` cuentan en MVP porque su núcleo entra en el primer incremento, aunque parte de su alcance se complete en fases posteriores. El detalle por fase está en `PRODUCT_SCOPE.md`.
