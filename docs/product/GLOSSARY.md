---
titulo: "Glosario del producto (ES/EN)"
fecha: "2026-09-17"
estado: "borrador para revisión"
autor: "Agente Analista de Requisitos (Claude) para Andrés Mauricio Pardo"
nivel_fuente_de_verdad: 2
fuentes:
  - resumenes/13_Glosario-Bilingue-ES-EN_ISO42001_y_Nota-Version-Inglesa.md (tablas 4.2, 4.4, 4.5; fuente de terminología oficial según A5)
  - resumenes/01_ISO-IEC-42001-2023_Requisitos_Clausulas-4-10.md (§5 conceptos clave)
  - resumenes/05_ISO-19011-2026_Directrices-Auditoria-Sistemas-de-Gestion.md (cláusula 3 y §5)
  - resumenes/04_ISO-IEC-23894-2023_Gestion-de-Riesgos-de-IA.md (§5)
  - resumenes/03_ISO-IEC-42001-2023_Anexos-C-D_Objetivos-Fuentes-de-Riesgo-y-Sectores.md (§3)
  - docs/product/MASTER_ISO42001_PLATFORM_REQUIREMENTS.md (términos de producto)
  - docs/product/PROMPT_REVIEW_AND_ADJUSTMENTS.md (A5, A8, A10)
---

# Glosario del producto (ES/EN)

## 1. Reglas de uso

1. **Término canónico en código**: siempre el inglés de la columna EN, en `snake_case` o `PascalCase` según el contexto (`statement_of_applicability`, `StatementOfApplicability`). Las enumeraciones de estado se almacenan en inglés y se traducen en la capa i18n (A5, decisión §4 de A).
2. **Término en UI y documentos en español**: el de la columna ES, que sigue la traducción oficial UNE-ISO/IEC 42001:2025 cuando existe. Donde el español sea ambiguo se añade el inglés entre paréntesis: "protección (safety)".
3. **Fuente**: indica de dónde procede la definición. `42001 cl. 3.n` significa cláusula 3 de ISO/IEC 42001 vía `resumenes/13 §4.2`; `42001 texto` significa término usado en la norma sin definición formal, vía `resumenes/01 §5` o `resumenes/13 §4.4`; `19011 cl. 3.n` vía `resumenes/05`; `23894` vía `resumenes/04 §5`; `Anexo C` vía `resumenes/03`; `producto` significa definición propia de la plataforma tomada del prompt maestro o de los ajustes. Las definiciones normativas están parafraseadas, nunca copiadas.
4. Las siglas SGIA y AIMS no aparecen en la norma (que siempre escribe "sistema de gestión de la IA"); el producto las usa en UI por brevedad, con el término completo en la primera aparición.

## 2. Términos definidos en la cláusula 3 de ISO/IEC 42001 (26 términos)

| Término ES | Término EN | Definición breve (paráfrasis) | Fuente |
|---|---|---|---|
| organización | organization | Persona o grupo con funciones, responsabilidades, autoridades y relaciones propias para alcanzar sus objetivos; si es parte de una entidad mayor, solo la parte dentro del alcance del SGIA. | 42001 cl. 3.1 |
| parte interesada | interested party | Persona u organización que puede afectar, verse afectada o percibirse afectada por una decisión o actividad. En inglés coloquial *stakeholder*, término que la norma no usa. | 42001 cl. 3.2 |
| alta dirección | top management | Persona o grupo que dirige y controla la organización al más alto nivel; puede delegar autoridad y asignar recursos. No traducir como "alta gerencia". | 42001 cl. 3.3 |
| sistema de gestión | management system | Conjunto de elementos interrelacionados para establecer políticas, objetivos y procesos que los logren. | 42001 cl. 3.4 |
| política | policy | Intenciones y dirección de la organización expresadas formalmente por la alta dirección. | 42001 cl. 3.5 |
| objetivo | objective | Resultado a lograr; los objetivos de IA los fija la organización en coherencia con la política de IA. | 42001 cl. 3.6 |
| riesgo | risk | Efecto de la incertidumbre; desviación de lo esperado, positiva o negativa. | 42001 cl. 3.7 |
| proceso | process | Conjunto de actividades interrelacionadas que usan o transforman entradas para entregar un resultado. | 42001 cl. 3.8 |
| competencia | competence | Capacidad de aplicar conocimientos y habilidades para lograr los resultados previstos. | 42001 cl. 3.9 |
| información documentada | documented information | Información que la organización debe controlar y mantener, y el medio que la contiene; abarca documentos vivos y registros. | 42001 cl. 3.10 |
| desempeño; rendimiento | performance | Resultado medible. La UNE asigna "desempeño" al sistema de gestión y "rendimiento" al sistema de IA; es una convención de traducción, no de la norma. | 42001 cl. 3.11 |
| mejora continua | continual improvement | Actividad recurrente para mejorar el desempeño (*continual*, no *continuous*). | 42001 cl. 3.12 |
| eficacia | effectiveness | Grado en que se realizan las actividades planificadas y se logran los resultados planificados. No confundir con eficiencia. | 42001 cl. 3.13 |
| requisito | requirement | Necesidad o expectativa establecida, generalmente implícita u obligatoria. | 42001 cl. 3.14 |
| conformidad | conformity | Cumplimiento de un requisito. | 42001 cl. 3.15 |
| no conformidad | nonconformity | Incumplimiento de un requisito. | 42001 cl. 3.16 |
| acción correctiva | corrective action | Acción para eliminar la causa de una no conformidad y prevenir su recurrencia; distinta de la corrección. | 42001 cl. 3.17 |
| auditoría | audit | Proceso sistemático e independiente para obtener evidencias y evaluarlas objetivamente frente a criterios; interna (primera parte) o externa (segunda o tercera parte). | 42001 cl. 3.18 |
| medición | measurement | Proceso para determinar un valor. | 42001 cl. 3.19 |
| seguimiento | monitoring | Determinación del estado de un sistema, proceso o actividad. La UNE usa "seguimiento", no "monitoreo" ni "monitorización". | 42001 cl. 3.20 |
| control | control | Medida que mantiene o modifica el riesgo (procesos, políticas, dispositivos, prácticas). Los controles no siempre producen el efecto previsto. | 42001 cl. 3.21 |
| órgano de gobierno | governing body | Persona o grupo responsable (*accountable*) del desempeño y la conformidad de la organización (consejo de administración, etc.). | 42001 cl. 3.22 |
| seguridad de la información | information security | Preservación de la confidencialidad, integridad y disponibilidad de la información. | 42001 cl. 3.23 |
| evaluación del impacto del sistema de IA | AI system impact assessment | Proceso formal y documentado para identificar, evaluar y abordar los impactos de un sistema de IA sobre las personas, los grupos de personas y la sociedad. Sigla informal AIIA. | 42001 cl. 3.24 |
| calidad de los datos | data quality | Característica de los datos consistente en que cumplen los requisitos de datos de la organización para un contexto específico. | 42001 cl. 3.25 |
| declaración de aplicabilidad | statement of applicability (SoA) | Documentación de todos los controles necesarios y de la justificación de su inclusión o exclusión; todo riesgo identificado y sus controles deben reflejarse en ella. | 42001 cl. 3.26 |

## 3. Términos de ISO/IEC 42001 usados sin definición formal

| Término ES | Término EN | Definición breve | Fuente |
|---|---|---|---|
| sistema de gestión de la inteligencia artificial (SGIA) | AI management system (AIMS) | Sistema de gestión cuyo objeto es que la organización desarrolle, provea o use sistemas de IA de forma responsable. | 42001 texto (resumenes/01 §5, 13 §4.4) |
| política de IA | AI policy | Intenciones y dirección de la alta dirección sobre la IA; marco de los objetivos de IA. | 42001 texto, 5.2 y A.2.2 |
| objetivos de la IA | AI objectives | Resultados a lograr respecto a la IA, coherentes con la política y medibles si es posible. | 42001 texto, 6.2 |
| roles respecto a la IA | AI roles | Papeles que una organización desempeña frente a un sistema de IA: proveedor, productor, cliente, socio, sujeto, autoridad pertinente; condicionan la aplicabilidad. Definidos en ISO/IEC 22989. | 42001 texto, 4.1 nota 1 |
| criterios de riesgo de la IA | AI risk criteria | Referencia para distinguir riesgos aceptables de no aceptables. | 42001 texto, 6.1.1 |
| apetito de riesgo | risk appetite | Cantidad y tipo de riesgo que la organización está dispuesta a asumir; lo fija el órgano de gobierno. La norma describe el concepto sin usar el término. | 42001 texto 6.1.1 nota 1; 23894 |
| evaluación del riesgo de la IA | AI risk assessment | Identificar, analizar (consecuencias, probabilidad, nivel) y evaluar (comparar con criterios, priorizar) los riesgos de la IA. | 42001 texto, 6.1.2 y 8.2 |
| tratamiento del riesgo de la IA | AI risk treatment | Seleccionar opciones y controles, compararlos con el Anexo A, producir la SoA y el plan. | 42001 texto, 6.1.3 y 8.3 |
| plan de tratamiento del riesgo de la IA | AI risk treatment plan | Plan que formaliza cómo se implementan las opciones y controles elegidos; lo aprueba la gerencia designada. | 42001 texto, 6.1.3 g) |
| riesgo residual | residual risk | Riesgo que permanece tras el tratamiento; su aceptación debe aprobarla la gerencia designada. | 42001 texto, 6.1.3 |
| gerencia designada | designated management | Nivel directivo que aprueba el plan de tratamiento y acepta los riesgos residuales. | 42001 texto, 6.1.3 |
| uso previsto / mal uso razonablemente previsible | intended use / reasonably foreseeable misuse | Finalidad del sistema de IA y usos indebidos anticipables; ambos se evalúan en la evaluación de impacto. | 42001 texto, 6.1.4, A.9.4 |
| supervisión humana | human oversight | Intervención o vigilancia humana sobre decisiones automatizadas. *Human-in-the-loop* no es término de la norma. | 42001 texto B.3.2, B.9.3; 23894 |
| ciclo de vida del sistema de IA | AI system life cycle | Etapas desde la concepción hasta la retirada; modelo genérico en ISO/IEC 22989 y 5338. | 42001 texto, A.6 |
| despliegue | deployment | Puesta en producción de un sistema de IA (A.6.2.5). No confundir con "implementación" del sistema de gestión. | 42001 texto, A.6.2.5 |
| retirada del servicio | decommissioning | Fin de vida del sistema de IA. | 42001 texto, A.4.6, C.3.6 |
| procedencia de los datos | data provenance | Origen, creación, transformación y transferencia de control de los datos (linaje). | 42001 texto, A.7.5 |
| deriva de datos / de concepto | data drift / concept drift | Cambio en la distribución de los datos o en la relación que el modelo aprendió, detectable en operación. | 42001 texto, B.6.2.6 |
| registros de eventos | event logs | Registros automáticos del sistema de IA en uso para trazabilidad. | 42001 texto, A.6.2.8 |
| notificación de inquietudes | reporting of concerns | Canal para que personas notifiquen inquietudes sobre la IA (A.3.3) o partes externas notifiquen impactos adversos (A.8.3). | 42001 texto, A.3.3, A.8.3 |
| rendición de cuentas | accountability | Responder por los resultados ante terceros; distinta de responsabilidad (*responsibility*, tener asignada una tarea). | 42001 texto A.3, A.10, C.2.1 |
| protección | safety | Ausencia de daño a la vida, la salud, la propiedad o el entorno. | 42001 texto, C.2.9 |
| seguridad | security | Protección frente a ataques y accesos no autorizados; incluye seguridad de la información. | 42001 texto, C.2.10 |
| equidad | fairness | Ausencia de decisiones automatizadas injustas para personas o grupos; antónimos: inequidad, sesgo no deseado. | 42001 texto, C.2.5 |
| transparencia y explicabilidad | transparency and explainability | Información sobre la organización y el sistema, y explicaciones comprensibles de los factores que influyen en los resultados. | 42001 texto, C.2.11 |
| fiabilidad (*reliability*) / confiabilidad de la IA (*trustworthiness*) | reliability / trustworthiness | La UNE usa "fiabilidad" para ambos; el producto desambigua con el inglés entre paréntesis. *Trustworthiness* agrupa protección, seguridad, equidad, transparencia, calidad. | 42001 texto, Introducción, B.6.2.4 |
| corrección | correction | Acción que elimina la no conformidad detectada, sin actuar sobre la causa. | 42001 texto, 10.2 a) 1) |
| revisión por la dirección | management review | Revisión periódica de la alta dirección sobre idoneidad, adecuación y eficacia del SGIA. | 42001 texto, 9.3 |
| auditoría interna | internal audit | Auditoría de primera parte que verifica conformidad y eficacia del SGIA. | 42001 texto, 9.2 |
| fuente de riesgo | risk source | Elemento que, solo o combinado, puede dar lugar a riesgo; el Anexo C lista siete (C.3.1-C.3.7). | Anexo C |
| objetivo relacionado con la IA (Anexo C) | AI-related objective | Once objetivos potenciales (C.2.1-C.2.11): rendición de cuentas, expertos en IA, datos, impacto ambiental, equidad, mantenibilidad, privacidad, robustez, protección, seguridad, transparencia y explicabilidad. | Anexo C |
| análisis de brechas | gap analysis | Comparación del estado actual con los requisitos; herramienta de implementación, no término de la norma. | resumenes/08, 13 §4.4 |
| certificación | certification | Evaluación de la conformidad por un organismo acreditado (ISO/IEC 17021-1); la norma no la regula y la plataforma nunca la declara. | 13 §4.4; producto |

## 4. Términos de ISO 19011:2026 (auditoría)

| Término ES | Término EN | Definición breve | Fuente |
|---|---|---|---|
| programa de auditoría | audit programme | Disposiciones para una o más auditorías planificadas para un periodo y propósito específicos. | 19011 cl. 3.5 |
| alcance de la auditoría | audit scope | Extensión y límites: ubicaciones físicas y virtuales, funciones, unidades, procesos y periodo. | 19011 cl. 3.6 |
| plan de auditoría | audit plan | Descripción de las actividades y disposiciones de una auditoría. | 19011 cl. 3.7 |
| criterios de auditoría | audit criteria | Requisitos de referencia frente a los que se compara la evidencia (políticas, procedimientos, requisitos legales, normas). | 19011 cl. 3.8 |
| evidencia objetiva | objective evidence | Datos que respaldan la existencia o veracidad de algo. | 19011 cl. 3.9 |
| evidencia de auditoría | audit evidence | Registros, declaraciones de hechos u otra información pertinente a los criterios y verificable. | 19011 cl. 3.10 |
| hallazgo de auditoría | audit finding | Resultado de evaluar la evidencia frente a los criterios: conformidad o no conformidad; puede señalar riesgos, oportunidades de mejora o buenas prácticas. | 19011 cl. 3.11 |
| conclusión de la auditoría | audit conclusion | Resultado de la auditoría tras considerar los objetivos y todos los hallazgos. | 19011 cl. 3.12 |
| auditado | auditee | Organización, total o parcialmente, que es auditada. | 19011 cl. 3.14 |
| equipo auditor | audit team | Personas que realizan la auditoría, con un líder y posibles expertos técnicos. | 19011 cl. 3.15 |
| método de auditoría remota | remote auditing method | Auditoría desde un lugar distinto del auditado, incluidas ubicaciones virtuales. | 19011 cl. 3.4 |
| seguimiento de la auditoría | audit follow-up | Verificación de correcciones y acciones correctivas del auditado tras la auditoría. | 19011 §5 (6.7) |
| clasificación de no conformidades | grading of nonconformities | Cuantitativa (1-5) o cualitativa (mayor/menor), con criterios definidos y comunicados. | 19011 §5 (6.4.8) |
| observación / oportunidad de mejora | observation / opportunity for improvement | Tipos de hallazgo distintos de la no conformidad; la plataforma los ofrece como valores por defecto configurables. | producto (P2 §23) sobre 19011 cl. 3.11 |

## 5. Términos de ISO/IEC 23894 (gestión del riesgo de IA)

| Término ES | Término EN | Definición breve | Fuente |
|---|---|---|---|
| marco de gestión de riesgos | risk management framework | Componentes que integran la gestión de riesgos en la organización. | 23894 |
| proceso de gestión de riesgos | risk management process | Comunicar, establecer contexto, evaluar, tratar, seguir y registrar. | 23894 |
| aprendizaje continuo | continuous learning | Adaptación del sistema en operación; fuente de dinamismo y de riesgo. | 23894 |
| gobernanza de IA | governance of AI | Consideraciones del órgano de gobierno sobre la IA (ISO/IEC 38507). | 23894 |

## 6. Términos de producto

| Término ES | Término EN | Definición breve | Fuente |
|---|---|---|---|
| tenant (inquilino) | tenant | Unidad de aislamiento de datos de la plataforma; toda tabla de negocio lleva `tenant_id` y RLS. Un tenant aloja una organización cliente. Se mantiene el anglicismo en UI. | producto (P2 §4, A8) |
| bounded context (contexto delimitado) | bounded context | Frontera de modelo dentro de la cual los términos tienen un único significado; en la plataforma, un módulo con interfaces explícitas y entidades propias. | producto (P1 §15) |
| evento de dominio | domain event | Hecho de negocio ocurrido (`CONTROL_ACTIVATED`), publicado por el contexto propietario vía outbox y consumido por otros. | producto (P2 §65, A8) |
| outbox | outbox pattern | Tabla transaccional donde se escriben los eventos junto con el cambio de estado, para publicarlos después de forma fiable. | producto (A8, ADR-003) |
| cadena de hashes | hash chain | Registro de auditoría append-only en que cada entrada incluye el hash de la anterior; "inmutable" se interpreta así. | producto (A8, ADR-005) |
| registro de auditoría de la aplicación | application audit log | Traza técnica de quién hizo qué, cuándo, por qué, con valor anterior y nuevo. No confundir con auditoría del SGIA (cláusula 9.2). | producto (P2 §41, §63) |
| catálogo de controles | control catalog | Los 38 controles del Anexo A sembrados y versionados; nivel de norma, común a todos los tenants. | producto (P2 §13) |
| elemento de la SoA | SoA item | Decisión de aplicabilidad de un tenant sobre un control del catálogo, con justificación, propietario y estado. | producto (P2 §5, §13) |
| activación de control | control activation | Transición de un SoA item a `Applicable` que dispara flujos, tareas, evidencia y métricas por evento. | producto (P2 §13) |
| implementación de control | control implementation | Estado operativo de un control activo en un tenant (`Planned` … `Retired`), con propietario, plan, tareas y pruebas. | producto (P2 §14) |
| eficacia del control | control effectiveness | Resultado de las pruebas de diseño, implementación y operación de un control; entrada del riesgo residual. Nunca se presume por existir el control. | producto (P2 §9, §14) sobre 42001 cl. 3.13 |
| implementación de requisito | requirement implementation | Estado de un requisito normativo en un tenant (propietario, estado, evidencia, tareas); separa el catálogo (norma) del cumplimiento (tenant). | producto [ASSUMPTION] |
| puerta de ciclo de vida | lifecycle gate | Conjunto de condiciones (evaluaciones, controles, evidencia, aprobaciones) que un sistema de IA debe cumplir para pasar de etapa; bloquea salvo excepción autorizada. | producto (P2 §12) |
| procedencia de la evidencia | evidence provenance | Metadatos que permiten defender una evidencia ante un auditor: conector, endpoint, hora, petición, objeto fuente, transformación, versión del colector, hash. | producto (P2 §15) |
| solicitud de evidencia | evidence request | Petición con vencimiento a un propietario para aportar evidencia de un control o requisito. | producto (P2 §15) |
| evidencia aceptada | accepted evidence | Evidencia revisada y aprobada por un revisor autorizado; subir no es aceptar. | producto (P2 §15, C.md) |
| CAPA (acción correctiva y preventiva) | CAPA (corrective and preventive action) | Registro que une hallazgo, no conformidad, corrección, causa raíz, acción correctiva, verificación de eficacia y cierre. La estructura armonizada no tiene "acción preventiva"; su función la cumple 6.1. | producto (P2 §24) sobre 42001 cl. 10.2 |
| hallazgo | finding | Resultado de evaluar algo frente a un criterio; genérico (`source_type`: auditoría, prueba de control, impacto, calidad de datos). | producto (P2 §5) sobre 19011 cl. 3.11 |
| preparación para certificación | certification readiness | Estado de la organización frente a la lista de comprobación de información documentada y procesos exigidos; se expresa como `Ready for internal review`, `Audit-ready`, `Certification preparation status`, nunca como "certificado". | producto (P2 §53) |
| puntuación transparente | transparent score | Métrica que expone fórmula, numerador, denominador, exclusiones, registros fuente y marca temporal. | producto (P2 §39) |
| preparación / cobertura / implementación / eficacia / exposición | readiness / coverage / implementation / effectiveness / exposure | Vocabulario permitido para puntuaciones; sustituye a "cumplimiento %". | producto (P2 §39) |
| sistema de IA candidato | candidate AI system | Registro creado por descubrimiento automático pendiente de validación humana; no es un sistema de IA del inventario hasta confirmarse. | producto (P2 §18) |
| proveedor de IA | AI provider | Proveedor tecnológico del modelo o servicio de IA (Azure, OpenAI, Anthropic…). Distinto de `Supplier` (tercero contractual, A.10.3) y de `Integration` (conector). | producto (P2 §5, §16) |
| artefacto generado por IA | AI-generated artifact | Contenido producido por el motor de asistencia, marcado con modelo, hora, prompt, fuentes, revisor y aprobación; nunca cambia estados por sí mismo. | producto (P2 §19) |
| excepción | exception | Autorización temporal y justificada para no cumplir un requisito o control, con control compensatorio, aprobador y expiración obligatoria. | producto (P2 §34) |
| solicitud de cambio | change request | Registro de un cambio significativo que dispara análisis de impacto, reevaluaciones y revisión de controles. | producto (P2 §33) |
| tipo de registro normativo | record type | `normative_requirement`, `implementation_guidance`, `recommended_practice`, `evidence_example`; distingue lo obligatorio de lo orientativo. | producto (P2 §1, A6) |
| SOURCE_DETAIL_REQUIRED | SOURCE_DETAIL_REQUIRED | Marcador de la semilla para detalle normativo ausente del corpus; nunca se rellena inventando. | producto (P2 §47) |
| semilla de normas | standards seed | Dataset estructurado y versionado (`packages/standards-seed/`) construido a partir de `resumenes/` y revisado por humanos; única vía por la que el conocimiento normativo entra en la base de datos. | producto (A4, A6) |
| documento fuente | source document | Archivo de `resumenes/NN` y sección de la que procede un registro de la semilla; primer nodo de la trazabilidad. | producto (A10) |
| retención legal | legal hold | Bloqueo de borrado o expiración de registros por obligación legal o litigio. | producto (P2 §42, A9) |
| región de datos | data region | Región geográfica donde residen los datos de un tenant. | producto (A9) |
| Row Level Security (RLS) | Row Level Security | Mecanismo de PostgreSQL que filtra filas por `tenant_id` en la base de datos, no solo en la aplicación. | producto (A8, ADR-002) |
| MVP / Fase 2 / Fase 3 | MVP / Phase 2 / Phase 3 | Cortes de la hoja de ruta única definidos en `PRODUCT_SCOPE.md`. | producto (A1, A2) |
| requisito de producto (PR-*) | product requirement | Enunciado atómico con ID estable en `REQUIREMENTS_ANALYSIS.md` (`PR-CAP`, `PR-XC`, `PR-NFR`, `PR-DR`, `PR-EXT`). | producto |
| decisión organizacional / decisión técnica de diseño | organizational decision / technical design decision | Clasificaciones obligatorias que separan lo que la organización decide y lo que el equipo técnico decide de lo que la norma exige. | producto (P2 §1, C.md) |

## 7. Trampas de traducción

Tomadas de `resumenes/13 §4.5`. La columna "Regla del producto" es vinculante para UI, documentos y semilla.

| Inglés | Traducción correcta (UNE) | Error frecuente | Regla del producto |
|---|---|---|---|
| safety | protección | "seguridad" | Enumeración `safety`; etiqueta ES "protección (safety)". Dimensión de impacto y objetivo C.2.9. |
| security | seguridad | se confunde con safety | Enumeración `security`; etiqueta ES "seguridad (security)". Objetivo C.2.10. |
| accountability | rendición de cuentas | "responsabilidad" | Usar siempre "rendición de cuentas"; *responsibility* = "responsabilidad" (tarea asignada). Objetivo de A.3 y A.10. |
| account for (A.4) | contabilizar / inventariar | "rendir cuentas de" | El objetivo de A.4 es inventariar recursos, no rendir cuentas. |
| deployment | despliegue | "implementación" | `deployment` = puesta en producción del sistema de IA (A.6.2.5); "implementación" se reserva para el SGIA y los controles. |
| shall / should | debe / debería | ambos como "debe" | `shall` → `normative_requirement`; `should` (Anexo B) → `implementation_guidance`. La UI nunca muestra orientación como requisito. |
| monitoring | seguimiento | "monitoreo", "monitorización" | Usar "seguimiento" (3.20, 9.1, A.6.2.6). |
| awareness | toma de conciencia | "conciencia" | Cláusula 7.3. |
| fairness | equidad | "justicia", "imparcialidad" | Siempre "equidad"; *unwanted bias* = "sesgo no deseado". |
| performance | desempeño (SGIA) / rendimiento (sistema de IA) | uno solo para ambos | Seguir la convención UNE y aclararlo en el glosario de UI. |
| interested party / relevant | parte interesada pertinente | "relevante", "stakeholder" | UI: "parte interesada"; la entidad se llama `InterestedParty`, no `Stakeholder`. |
| reporting of concerns / external reporting | notificación de inquietudes / notificación externa | "informes externos" | A.3.3 y A.8.3 son canales de notificación, no informes formales. |
| AI expertise | expertos en IA / pericia en IA | "experiencia en IA" | Objetivo C.2.2. |
| technology readiness | disponibilidad (madurez) de la tecnología | "preparación tecnológica" | Fuente de riesgo C.3.7; glosar como "madurez tecnológica". |
| statement of applicability | declaración de aplicabilidad | "declaración de aplicación" | Sigla SoA en UI; entidad `StatementOfApplicability`. |
| scope | objeto y campo de aplicación (cláusula 1) / alcance (4.3) | mezclar ambos | En el producto "alcance" siempre significa alcance del SGIA (4.3). |
| top management | alta dirección | "alta gerencia" | Cláusulas 5 y 9.3. |
| data subject / AI subject | interesado (protección de datos) / sujeto de la IA | "sujeto de datos" | Roles de IA de 4.1 nota 1. |
| life cycle | ciclo de vida | "ciclo vital" | Dominio A.6. |
| continual improvement | mejora continua | "mejora continuada" | Cláusula 10.1. |
