---
title: "PRD: Plataforma SaaS de gobernanza de IA ISO/IEC 42001"
status: draft
created: 2026-09-17
updated: 2026-09-17
project: ISO42001
language: es
author: "Andrés Mauricio Pardo (promotor) — redactado en modo headless por bmad-prd (Fast path)"
level: 2
inputs:
  - _bmad-output/planning-artifacts/briefs/brief-ISO42001-2026-09-17/brief.md (+ addendum.md)
  - docs/product/REQUIREMENTS_ANALYSIS.md (205 requisitos PR-*)
  - docs/product/PRODUCT_SCOPE.md
  - docs/product/GLOSSARY.md
  - docs/product/DOMAIN_MAP.md (conflictos C1-C15)
  - docs/product/PROMPT_REVIEW_AND_ADJUSTMENTS.md (ajustes A1-A13)
  - docs/product/MASTER_ISO42001_PLATFORM_REQUIREMENTS.md (P2, por secciones)
  - docs/product/MASTER_ORCHESTRATION_BMAD_OPENSPEC.md (P1 §13, §17, §37-38)
  - CLAUDE.md
  - resumenes/01, resumenes/02 (verificación de cláusulas y controles)
---

# PRD: Plataforma SaaS de gobernanza de IA ISO/IEC 42001

*Título de trabajo — el nombre comercial está pendiente (pregunta abierta Q1).*

> Documento de nivel 2 en la jerarquía de fuentes de verdad. Redactado sin usuario disponible (modo headless, Fast path): cada inferencia no respaldada por los insumos lleva la etiqueta `[ASSUMPTION]`, indexada en §19; toda cifra objetivo lleva `[ESTIMACIÓN]`. Las decisiones que este PRD toma provisionalmente y que un humano debe ratificar están en §17. El detalle que no cabe aquí (matrices completas, alternativas descartadas, mecanismos) está en `addendum.md`; el registro cronológico de decisiones, en `.memlog.md`.

## 0. Propósito del documento

Este PRD convierte la línea base de requisitos de la plataforma (P2 + ajustes A1-A13, ya descompuesta en 205 requisitos atómicos `PR-*` en `REQUIREMENTS_ANALYSIS.md`) en requisitos de producto formales, priorizados y trazables, para que los consuman en orden: `bmad-ux` (especificación de experiencia), `bmad-architecture` (arquitectura y ADRs), `bmad-create-epics-and-stories` (épicas e historias) y, después, cada cambio OpenSpec. Lo leen también el promotor (decisiones de §17), los futuros implementadores piloto y los auditores que quieran entender qué promete el producto y qué no.

**Cómo está organizado.** El vocabulario está anclado en el glosario del producto (§5 y `docs/product/GLOSSARY.md`). Los requisitos funcionales (§6) van agrupados por capacidad con identificadores globales estables `FR-nnn`; cada FR cita los `PR-*` que cubre, su fase (`MVP`, `MVP*` = núcleo en MVP y resto en Fase 2, `F2`, `F3`) y consecuencias verificables. Los requisitos no funcionales (§7) usan `NFR-nnn`. Las reglas de dominio (§9) conservan su identificador `PR-DR-nnn` porque ya son estables y alimentan directamente la matriz de trazabilidad. Los `PR-*` **no se renumeran**: son el eslabón [2] de la cadena de `TRACEABILITY_MODEL.md`.

**Correspondencia con las 24 áreas exigidas por P1 §13.**

| Área P1 §13 | Dónde vive en este PRD |
|---|---|
| 1 Resumen ejecutivo | §1 |
| 2 Problema | §2 |
| 3 Visión | §3 |
| 4 Personas | Inline en cada trayectoria de §4.3 (sin sección de personas independiente, por disciplina del skill) |
| 5 Trayectorias de usuario | §4.3 (UJ-1 a UJ-7) |
| 6 Requisitos funcionales | §6 (FR-001 a FR-132) |
| 7 Requisitos no funcionales | §7 (NFR-001 a NFR-024) |
| 8 Requisitos de seguridad | §7.1 y §6.1 (FR-002 a FR-007), matriz rol × permiso en `addendum.md` §B |
| 9 Requisitos de gobernanza de IA | §6.21, §9 (PR-DR-008, PR-DR-026) |
| 10 Requisitos de integración | §6.19 |
| 11 Requisitos de informes | §6.17 |
| 12 Requisitos de auditoría | §6.13 (auditoría del SGIA) y §6.1 FR-007 + §7.7 (auditabilidad de la aplicación) |
| 13 Requisitos de evidencia | §6.9 |
| 14 Requisitos de riesgo | §6.5 |
| 15 Requisitos de ciclo de vida | §6.7 |
| 16 Requisitos de administración | §6.1 |
| 17 Requisitos de UX | §6.22 y §7.4 |
| 18 Requisitos de datos | §8 |
| 19 Métricas de éxito | §10 (con contramétricas) |
| 20 Fuera de alcance | §11.2, §11.3 y §12 |
| 21 Supuestos | §13 y §19 (índice) |
| 22 Riesgos | §14 |
| 23 Dependencias | §15 |
| 24 Criterios de aceptación | §16 |

**Lo que este PRD no repite.** No vuelve a describir el panorama competitivo (brief `addendum.md` §A), ni las decisiones de arquitectura ADR-001 a ADR-005 (brief `addendum.md` §E), ni el mapa de bounded contexts y eventos (`DOMAIN_MAP.md`), ni el esquema de la matriz de trazabilidad (`TRACEABILITY_MODEL.md`). Los referencia.

## 1. Resumen ejecutivo

ISO/IEC 42001:2023 es la primera norma internacional certificable para un sistema de gestión de la inteligencia artificial (SGIA / AIMS). Exige documentar el contexto y el alcance, inventariar los sistemas de IA, evaluar riesgos e impactos, seleccionar controles del Anexo A mediante una declaración de aplicabilidad (SoA), producir evidencia, auditarse internamente y revisar el sistema desde la alta dirección. Las organizaciones que lo intentan hoy `[SUPUESTO heredado del brief]` lo hacen con hojas de cálculo, plantillas de consultora y carpetas compartidas, o con plataformas GRC anglófonas que tratan la norma como una lista de casillas.

Este PRD especifica una **plataforma SaaS multi-tenant que opera el SGIA en lugar de describirlo**: la SoA activa o desactiva flujos, tareas, evidencia y métricas; cada evidencia lleva hash y procedencia y solo cuenta cuando un revisor autorizado la acepta; cualquier registro se recorre de ida y vuelta por la cadena *requisito → control → riesgo → sistema de IA → evidencia → auditoría → hallazgo → CAPA → revisión por la dirección*; toda puntuación expone fórmula, numerador, denominador, exclusiones y registros fuente; la IA asistente sugiere y una persona aprueba; y la plataforma nunca declara a nadie "certificado". Es bilingüe ES/EN desde la fundación con la terminología oficial UNE.

**Alcance.** El MVP (ajuste A1) entrega la arquitectura de dominio completa (21 bounded contexts, todas las entidades y eventos) con implementación por corte vertical: una organización real completa el flujo de 15 pasos de §16 —crear organización, definir contexto, partes interesadas y alcance, inventariar IA, evaluar riesgo e impacto, completar y aprobar la SoA, activar controles, recoger evidencia, auditar internamente, registrar no conformidad, gestionar CAPA, revisar por la dirección y emitir los cuatro informes esenciales— sin conectores reales (SDK + adaptador mock etiquetado) y sin funciones LLM. Fase 2 conecta y asiste (conectores Azure AI Foundry / OpenAI / Anthropic, descubrimiento, ciclo de vida con puertas, incidentes, IA asistida autogobernada, 25 informes, formación, calidad de datos). Fase 3 extiende (agentes, constructor de informes, búsqueda semántica, paquetes sectoriales, madurez).

**Cifras del documento.** 132 requisitos funcionales (FR), 24 no funcionales (NFR), 30 reglas de dominio (PR-DR), 7 trayectorias de usuario, 8 métricas de éxito con 8 contramétricas, 15 pasos de aceptación E2E, 141 requisitos `PR-*` en MVP de 205 totales.

## 2. Problema

### 2.1 El dolor

Implementar y sostener un SGIA exige coordinar decenas de artefactos vivos: alcance aprobado, política de IA, tres procesos recurrentes (evaluación del riesgo, tratamiento, evaluación del impacto), 38 controles del Anexo A con justificación de aplicabilidad, plan de tratamiento aprobado por la gerencia designada, aceptación de riesgos residuales, evidencia por control, programa de auditoría interna, no conformidades con causa raíz y verificación de eficacia, y una revisión por la dirección con entradas obligatorias (9.3.2) y salidas (9.3.3). La tabla de información documentada obligatoria de `resumenes/01 §4` tiene 21 filas. Quien lo intenta a mano sufre tres fallos documentados en el corpus (`resumenes/01 §7`):

1. **Desconexión.** El registro de riesgos dice una cosa, la SoA otra y la evidencia no se enlaza con ninguna; el auditor no puede seguir el hilo.
2. **Falsa seguridad.** Se confunde "tener plan de tratamiento" con "riesgo tratado" y "evidencia subida" con "evidencia aceptada". Un tablero con un 87 % verde oculta que nadie aprobó nada.
3. **Deterioro.** Las evaluaciones no se repiten ante cambios significativos (nueva versión del modelo, nuevo dataset, nuevo uso, incidente); el sistema envejece y la recertificación se convierte en proyecto de rescate.

### 2.2 Cómo se afronta hoy

Plantillas Excel/Word de consultoras y academias (el propio corpus contiene cinco: `resumenes/08-12`), SharePoint, tickets sueltos, o plataformas GRC generalistas que añadieron "ISO 42001" como un marco más de casillas, sin modelo de sistema de IA, sin evaluación de impacto como proceso distinto del riesgo, sin activación de flujos desde la SoA y sin español (brief `addendum.md` §A.3).

### 2.3 Quién sufre el problema

- La **oficial de gobernanza de IA** que reconcilia tres fuentes de verdad a mano y persigue evidencia por correo.
- El **propietario de sistema de IA** al que le piden "la evaluación de impacto" sin formulario, sin criterios y sin saber por qué.
- El **auditor interno** que no puede muestrear evidencia con procedencia ni derivar una conclusión trazable.
- La **alta dirección** que firma una revisión por la dirección sin ver las entradas agregadas.
- La **consultora** que replica su metodología cliente a cliente en documentos sueltos.

### 2.4 Por qué ahora

La norma se publicó en diciembre de 2023; su adopción idéntica UNE-ISO/IEC 42001:2025 es reciente; ISO/IEC 42006:2025 ya fija requisitos para los organismos certificadores, lo que activa el mercado de certificación; y la regulación (AI Act de la UE, marcos emergentes en Latinoamérica) empuja a clientes, licitaciones y socios a exigir evidencia objetiva de gobernanza de IA (brief §1, `addendum.md` §A.2, con marcas `[NO VERIFICADO]` que este PRD hereda). No hay oferta confirmada con UI en español nativa para 42001.

## 3. Visión y tesis

> **"Software que opera el sistema de gestión de IA de la organización y produce continuamente la evidencia de que funciona"** — no "software que me dice si marqué la casilla" (principio rector P2 §70; decisión D2 del brief).

**Tesis del producto.** Un SGIA es un sistema operativo, no un formulario. Si el software modela la norma como un conjunto de flujos gobernados por decisiones explícitas (aplicabilidad, aceptación, aprobación, verificación) y registra cada decisión con actor, motivo y valor anterior, entonces la evidencia de que el sistema funciona se produce como subproducto del trabajo diario, y la auditoría deja de ser un proyecto de rescate. La apuesta de diferenciación es de **ejecución y enfoque**, no de patente: nadie del panorama competitivo revisado opera la SoA como activadora de flujos, automatiza la revisión por la dirección con sus entradas 9.3.2, modela la auditoría interna conforme a ISO 19011:2026, ni ofrece trazabilidad bidireccional y UI ES/EN nativa (brief `addendum.md` §A.3).

**El ciclo del producto**, alineado con la estructura armonizada (cláusulas 4-10) y el PDCA:

| Etapa | Qué hace la plataforma | Cláusulas / anexo | Sección FR |
|---|---|---|---|
| **Discover** | Inventaria sistemas, modelos, datos, proveedores y agentes; determina el rol de la organización respecto a la IA; (F2) descubre activos desde plataformas conectadas | 4.1, A.4, A.10 | §6.4, §6.19 |
| **Govern** | Contexto, partes interesadas, alcance aprobado, política de IA, roles, objetivos medibles | 4.2-4.4, 5, 6.2 | §6.3 |
| **Assess** | Evaluación del riesgo y evaluación del impacto como procesos distintos y enlazados, repetibles ante cambios, con aceptación formal del residual | 6.1.2, 6.1.4, 8.2, 8.4, A.5 | §6.5, §6.6 |
| **Control** | SoA frente a los 38 controles del Anexo A; justificación obligatoria de exclusiones; activación de flujos solo para los aplicables | 6.1.3, Anexo A/B | §6.8 |
| **Evidence** | Evidencia con ciclo de vida, hash, procedencia y revisor; subida ≠ aceptada | 7.5, 8.1, 8.3 | §6.9, §6.10 |
| **Monitor** | Puntuaciones transparentes, KPI, alertas por vencimiento y cambio significativo | 9.1 | §6.16, §6.11 |
| **Audit** | Programa y auditorías internas conforme a ISO 19011:2026, hallazgos, no conformidades y CAPA con verificación de eficacia | 9.2, 10.2 | §6.13, §6.14 |
| **Improve** | Revisión por la dirección con entradas y salidas obligatorias; decisiones trazadas que realimentan contexto, riesgos y política | 9.3, 10.1 | §6.15 |

**Siete principios de producto** (heredados de `CLAUDE.md` y del brief §5; son requisitos, no aspiraciones; cada uno tiene reglas de dominio en §9):

1. La SoA gobierna los flujos (PR-DR-001 a PR-DR-004).
2. Riesgo tratado ≠ plan de tratamiento existente (PR-DR-005, PR-DR-006).
3. Evidencia subida ≠ evidencia aceptada (PR-DR-011).
4. La IA sugiere; una persona autorizada aprueba (PR-DR-008).
5. La plataforma nunca declara "certificado ISO"; toda puntuación expone fórmula y registros fuente (PR-DR-009, PR-DR-010).
6. Nunca se inventan requisitos, identificadores ni texto normativo; nunca se reproduce texto literal de la norma (PR-DR-019, NFR-020).
7. La plataforma se gobierna a sí misma: sus funciones LLM son sistemas de IA de su propio inventario (PR-DR-026).

**En tres años**, si funciona, la plataforma es el lugar donde una organización o su consultora *vive* su gobernanza de IA: conectada a sus proveedores de modelos, con paquetes sectoriales, gobierno de agentes autónomos, modelo de madurez y paquete de evidencia listo para el organismo de certificación. Nunca declarará a nadie "certificado".

## 4. Usuarios y trayectorias

### 4.1 Trabajos por hacer (JTBD)

- **Funcional:** operar un SGIA completo de punta a punta con un solo sistema de registro; saber qué falta, por qué se exige (P2 §67) y quién lo debe hacer; llegar a la auditoría interna y a la de certificación con evidencia defendible.
- **Social:** demostrar a clientes, licitaciones, reguladores y consejo que la gobernanza de IA existe y funciona, sin exagerar (nunca "certificado" antes de tiempo).
- **Emocional:** dejar de temer la pregunta del auditor "¿y esto de dónde sale?"; dejar de reconciliar tres archivos a mano la noche anterior.
- **Contextual:** trabajar en español o inglés con terminología oficial; operar varias organizaciones (consultora) con la misma metodología; formar Lead Implementers con datos realistas.

### 4.2 No usuarios (v1)

- Organizaciones que buscan una **checklist rápida** para "tener algo" sin operar el sistema: el producto les resultará exigente por diseño.
- Organismos de certificación que necesiten **auditar de tercera parte** conforme a ISO/IEC 42006: esa norma no está en el corpus y la plataforma no actúa como organismo (PR-EXT-013).
- Equipos que necesiten **ejecutar o alojar modelos**: la plataforma gobierna, no ejecuta inferencia.
- Organizaciones que exijan **portugués u otros idiomas** en el MVP (pregunta abierta Q5).

### 4.3 Trayectorias de usuario

*Protagonistas con nombre; la persona vive inline en su trayectoria. La organización "Financiera Altiplano" y la consultora "GovIA Consultores" son ficticias. Los FR referencian las trayectorias por identificador ("realiza UJ-1"). Las siete trayectorias cubren, en orden, el flujo de aceptación E2E de §16.*

---

**UJ-1. Lucía completa la declaración de aplicabilidad y la plataforma enciende solo lo que aplica.**

- **Persona + contexto:** Lucía Ferrer, *AI Governance Manager* de Financiera Altiplano (banca mediana, 800 empleados, usa IA de terceros para scoring y atención al cliente). Antes gestionaba ISO 27001 con Excel y SharePoint; conoce la norma, no confía en los tableros verdes.
- **Estado de entrada:** autenticada (OIDC corporativo), rol `AI Governance Manager`, escritorio. El alcance del SGIA (v1) ya está aprobado, hay dos sistemas de IA inventariados y siete riesgos con plan de tratamiento aprobado. Abre *ISO 42001 → Declaración de aplicabilidad*.
- **Camino:**
  1. Ve los 38 controles del Anexo A agrupados en los nueve dominios A.2 a A.10, cada uno en `Pending Assessment`, con el objetivo del dominio y la orientación del Anexo B marcada como `implementation_guidance` (nunca como requisito).
  2. Para A.6.2.4 (verificación y validación) marca `Applicable`, asigna propietaria (Carmen Ibáñez), enlaza los riesgos R-003 y R-005 y el requisito 8.1; la plataforma le muestra qué evidencia esperada trae la semilla.
  3. Para A.7.6 (preparación de los datos) marca `Not Applicable` porque la organización no entrena modelos; el formulario no le deja guardar sin justificación documentada (PR-DR-003). Escribe la justificación citando el rol declarado "cliente de IA" en el contexto 4.1.
  4. Al intentar aprobar la SoA v1, la plataforma bloquea: tres controles `Applicable` no tienen propietario ni riesgo mapeado (PR-DR-004). Los completa.
  5. Aprueba la SoA v1 con permiso `soa.approve`; queda versionada y comparable con futuras versiones.
- **Clímax:** al aprobar, la plataforma emite `SOA_APPROVED` y un `CONTROL_ACTIVATED` por cada control aplicable: aparecen 26 tareas de implementación y 26 solicitudes de evidencia con vencimiento, asignadas a sus propietarios; A.7.6 y los demás `Not Applicable` no generan nada, pero siguen visibles con su justificación y entran en el informe de completitud de la SoA. Lucía ve en el panel la puntuación *Control Implementation 0/26* con su fórmula desplegada y el denominador excluyendo explícitamente los 12 no aplicables.
- **Resolución:** el registro de auditoría de la aplicación guarda cada decisión con actor, motivo, valor anterior y hash encadenado. Lucía exporta el *Statement of Applicability Report* en PDF, en español, para el comité.
- **Caso límite:** seis meses después Financiera Altiplano decide afinar un modelo propio; Lucía cambia A.7.6 a `Applicable` en la SoA v2; se emite `CONTROL_APPLICABILITY_CHANGED`, se crean sus flujos y la comparación v1→v2 muestra el cambio con justificación.

---

**UJ-2. Rodrigo registra el motor de scoring y evalúa su riesgo y su impacto como dos procesos enlazados.**

- **Persona + contexto:** Rodrigo Salas, *AI System Owner* del "Motor de scoring crediticio", jefe de producto de crédito. No es experto en la norma; usa la plataforma unas horas al mes cuando le llegan tareas. Necesita formularios guiados sin jerga innecesaria y saber "por qué me piden esto".
- **Estado de entrada:** invitado por Diego (UJ-7), activado, rol `AI System Owner`. Recibe una tarea "Registrar sistema de IA en alcance" creada al aprobar el alcance. Entra desde el correo de notificación (ES).
- **Camino:**
  1. Crea el sistema de IA con la ficha de P2 §11: propósito de negocio, proveedor tecnológico (`AIProvider`: proveedor cloud del modelo), modelo y versión (`AIModelVersion`), despliegue, datos y clasificación (incluye datos personales), usuarios y partes afectadas (solicitantes de crédito), nivel de automatización, tipo de decisión, supervisión humana ("analista revisa denegaciones"), uso previsto y mal uso razonablemente previsible, propietarios de negocio y técnico.
  2. La plataforma le propone, desde la semilla, las fuentes de riesgo del Anexo C (C.3.1-C.3.7) y los objetivos C.2.1-C.2.11. Con la gestora de riesgos crea dos riesgos: "sesgo no deseado por variables proxy" (fuente C.3.x, objetivo equidad C.2.5) y "deriva de datos no detectada". Elige la escala 5×5 configurada por el tenant; la plataforma calcula inherente y, tras mapear controles A.6.2.6 y A.7.4, el residual a partir de la eficacia declarada.
  3. Abre la evaluación de impacto con la plantilla semilla (individuos, grupos, sociedad, privacidad, equidad, transparencia, protección (*safety*), seguridad (*security*), derechos humanos, accesibilidad, medio ambiente, impacto económico y social, decisiones automatizadas, supervisión humana, mal uso previsible). Registra un hallazgo de impacto "posible exclusión de colectivos sin historial crediticio" con mitigación y propietario.
  4. Al aprobar la evaluación de impacto, la plataforma la enlaza al riesgo del mismo sistema: la evaluación de riesgo no puede marcarse completa mientras exista un impacto posterior no revisado (PR-DR-028).
  5. Propone tratamiento `Mitigate` con plan; la gestora de riesgos lo aprueba; el residual queda dentro de criterios para R-003 y requiere aceptación formal para R-005.
- **Clímax:** la directora de riesgos acepta R-005 con justificación y fecha de expiración a 12 meses; la plataforma emite `RISK_ACCEPTED`, registra el historial de aprobación y programa `RISK_ACCEPTANCE_EXPIRING` a 30 días del vencimiento. En la ficha del sistema, Rodrigo ve la cadena completa: sistema → riesgos → controles → evidencia esperada → requisito 6.1.2 / 6.1.4, cada eslabón con su "¿por qué es requerido?".
- **Resolución:** las tareas de Rodrigo quedan cerradas; el dashboard de Lucía refleja *Risk Exposure* recalculado con fórmula visible.
- **Caso límite:** cuatro meses después el proveedor publica una nueva versión del modelo. Rodrigo actualiza `AIModelVersion`; se emite `MODEL_VERSION_CHANGED`; el riesgo y la evaluación de impacto pasan a "reevaluación requerida" y se abre revisión de los controles mapeados (PR-DR-007). Nada se da por válido en silencio.

---

**UJ-3. Carmen aporta la evidencia de su control y descubre que subir no es aceptar.**

- **Persona + contexto:** Carmen Ibáñez, ingeniera de MLOps, *Control Owner* de A.6.2.6 (operación y seguimiento) y A.6.2.4. Pragmática; quiere saber exactamente qué evidencia sirve y cuándo vence.
- **Estado de entrada:** autenticada, rol `Control Owner` + `Evidence Owner`. En *Evidencia → Solicitudes* ve dos `EvidenceRequest` en estado `Requested` generadas por la activación de sus controles (UJ-1), con vencimiento a 30 días y la evidencia esperada tomada de la semilla.
- **Camino:**
  1. Abre la solicitud de A.6.2.6; la ficha del control explica el objetivo, qué riesgos lo seleccionaron (R-005) y qué criterio de auditoría lo probará.
  2. Sube el informe mensual de seguimiento de deriva (tipo `report`) y una captura del panel de alertas (tipo `screenshot`). La plataforma calcula el hash de cada archivo, asigna versión 1, clasificación, retención y expiración (90 días), y los enlaza al control, al riesgo R-005 y al sistema. Estado: `Collected`.
  3. Solicita revisión; la evidencia pasa a `Under Review` y se notifica a la revisora (Lucía, rol `Reviewer`). Carmen no puede aprobar su propia evidencia (PR-DR-011).
  4. Lucía acepta el informe (`Accepted`, con revisora y fecha) y rechaza la captura con motivo "no muestra la fecha ni el sistema" (`Rejected`). Carmen recibe la notificación con el motivo y vuelve a subir una captura completa como versión 2.
- **Clímax:** la puntuación *Evidence Coverage* del control pasa a 1/1 solo cuando la evidencia está `Accepted`; el estado del control avanza a `Operating`. Carmen ve el historial: quién subió, quién revisó, qué se rechazó y por qué.
- **Resolución:** a los 60 días la plataforma emite `EVIDENCE_EXPIRING`; a los 90, si no hay versión nueva, `EVIDENCE_EXPIRED` y el control vuelve a `Evidence Pending` en su requisito. Nunca se borra evidencia: se supersede (`Superseded`).
- **Caso límite:** Carmen intenta borrar una evidencia rechazada; la plataforma no ofrece borrado físico, solo archivado gobernado por retención y `legal hold` (PR-DR-015), y registra el intento.

---

**UJ-4. Javier audita el SGIA como manda ISO 19011 y la conclusión sale de los hallazgos, no al revés.**

- **Persona + contexto:** Javier Montes, auditor interno certificado *Lead Auditor* ISO/IEC 42001 (formado en un programa PECB), independiente de las áreas auditadas. Trabaja por ciclos; necesita muestrear evidencia con procedencia y dejar un informe defendible.
- **Estado de entrada:** autenticado, rol `Auditor`. En *Auditoría → Programa* existe el programa anual basado en riesgos creado por Lucía, con una auditoría `Scheduled` del alcance completo.
- **Camino:**
  1. Abre la auditoría, define el plan: alcance, objetivos, criterios (requisitos 4.1-10.2 y controles `Applicable` de la SoA v1, cargados automáticamente desde el catálogo y la SoA), método (remoto y en sitio), equipo auditor con declaración de independencia.
  2. Avanza el ciclo `Preparing → Opening → Fieldwork → Evidence Collection`. Por cada criterio, selecciona evidencia de la biblioteca (solo `Accepted`, con hash y revisor visibles) y la vincula como `AuditEvidence` al criterio; si falta, lo anota.
  3. Registra hallazgos con la clasificación configurada por Financiera Altiplano (`Conformity`, `Nonconformity` mayor/menor, `Observation`, `Opportunity for Improvement`): una no conformidad menor en 10.2 (una CAPA anterior cerrada sin verificación de eficacia registrada) y una observación en A.8.2 (información a usuarios incompleta). Cada hallazgo enlaza criterio, evidencia y requisito/control.
  4. Intenta emitir la conclusión con dos criterios sin hallazgo ni declaración de conformidad; la plataforma lo bloquea (PR-DR-023). Completa las declaraciones de conformidad.
- **Clímax:** la conclusión se deriva de los hallazgos y la evidencia reales; el informe de auditoría se genera con la cadena *criterio → evidencia → hallazgo → conclusión*; se emite `AUDIT_CONCLUDED` y `AUDIT_FINDING_CREATED` por hallazgo, que abre la no conformidad en CAPA (UJ-5 la verá agregada). Javier exporta el informe en EN para el comité de auditoría del grupo.
- **Resolución:** la auditoría queda en `Follow-up`; cuando la CAPA se cierre con verificación de eficacia, Javier verificará la corrección y cerrará la auditoría.
- **Caso límite:** un auditor externo del organismo de certificación recibe acceso `Read Only` al paquete de evidencia y a la SoA de Financiera Altiplano; no ve otros tenants ni puede modificar nada; cada acceso queda registrado. `[ASSUMPTION: el acceso de lectura para auditor externo se infiere de la persona "auditor externo" del brief; el permiso concreto se fija en la matriz rol × permiso pendiente de aprobación.]`

---

**UJ-5. Marta preside la revisión por la dirección con las entradas ya agregadas y firma decisiones que dejan huella.**

- **Persona + contexto:** Marta Roldán, directora general (alta dirección) de Financiera Altiplano. Dedica una hora al trimestre; quiere un tablero honesto con el vocabulario prescrito (preparación, cobertura, exposición) y un botón "aprobar" que deje rastro. Desconfía de los porcentajes sin explicación.
- **Estado de entrada:** autenticada (MFA), rol `Executive`. Lucía ha programado la revisión por la dirección con fecha de corte; Marta recibe la notificación y abre *Revisión por la dirección*.
- **Camino:**
  1. Ve las entradas de 9.3.2 agregadas automáticamente a la fecha de corte: estado de las decisiones anteriores, objetivos de la IA y sus métricas, KPI, riesgos (incluida la aceptación de R-005 y su expiración), evaluaciones de impacto, resultado de la auditoría de Javier con sus dos hallazgos, no conformidades y CAPA abiertas, cambios del portafolio (nueva versión del modelo de scoring), retroalimentación de partes interesadas, desempeño de proveedores y necesidades de recursos.
  2. Cada cifra del panel es una puntuación transparente: pulsa *Audit Readiness* y ve fórmula, numerador, denominador, exclusiones y los registros fuente. Ninguna pantalla dice "certificado".
  3. Registra las salidas de 9.3.3: decisión de reforzar la información a usuarios (A.8.2), asignación de recursos para el equipo de MLOps, acción "revisar la política de IA para incluir principios de equidad" con responsable y plazo, y confirmación del alcance sin cambios.
  4. Aprueba la revisión; la plataforma genera el acta como información documentada, con la lista de participantes y las decisiones.
- **Clímax:** al aprobar se emite `MANAGEMENT_REVIEW_APPROVED`: las acciones se convierten en tareas con propietario; la revisión de la política abre un `POLICY_REVIEW_DUE`; el acta se exporta a PDF en español. Marta ve que su firma quedó en el registro de auditoría con hash encadenado.
- **Resolución:** el espacio de preparación para certificación marca "Revisión por la dirección completada" en la lista de 13 puntos y muestra el estado `Ready for internal review`, con la explicación de qué falta para `Audit-ready`.
- **Caso límite:** Marta intenta aprobar con una entrada obligatoria sin datos (no hay resultados de auditoría porque la auditoría aún no concluyó); la plataforma lo permite solo si registra explícitamente "sin datos en el periodo" con motivo, y lo refleja en el acta.

---

**UJ-6. Sofía opera tres clientes con la misma metodología sin que un dato cruce de un tenant a otro.**

- **Persona + contexto:** Sofía Navarro, consultora líder ISO/IEC 42001 y 27001 en GovIA Consultores `[SUPUESTO heredado del brief: segmento consultora]`. Implementa el SGIA para tres clientes a la vez; quiere reutilizar su metodología, comparar avance y exportar entregables. Si el producto la sustituye, no lo adopta; debe ser "la herramienta de la consultora".
- **Estado de entrada:** autenticada con su identidad de GovIA, miembro (`Membership`) de tres tenants distintos con rol `Compliance Manager` en cada uno. Abre la plataforma y ve el selector de organización.
- **Camino:**
  1. Cambia al tenant de Financiera Altiplano; toda la UI, la API y las consultas quedan acotadas por `tenant_id` con Row Level Security: no existe vista agregada entre clientes en el MVP `[NON-GOAL for MVP]`.
  2. Revisa el espacio de preparación para certificación y exporta el *Audit Readiness Report* con la marca del cliente.
  3. Cambia al segundo cliente, una startup de salud; configura su escala de riesgo 3×3 y su clasificación de hallazgos, distinta de la del banco (ambas configurables por tenant).
  4. En el tercer cliente crea el contexto 4.1 a partir de la plantilla FODA vs. riesgo (`resumenes/11`) que la plataforma trae como estructura de campos, no como texto.
- **Clímax:** en la misma tarde Sofía deja tres SGIA con contexto, partes interesadas y alcance en borrador, cada uno con su registro de auditoría independiente y su cadena de hashes propia; ningún dato de un cliente aparece en otro (prueba de seguridad cross-tenant en cada cambio).
- **Resolución:** en Fase 2 dispondrá de plantillas propias de consultora (contexto, riesgos, preguntas de auditoría) reutilizables entre tenants y de un panel multi-cliente `[ASSUMPTION: capacidad de Fase 2/3 pendiente de validar con una consultora real; pregunta abierta Q2]`.
- **Caso límite:** un cliente termina el contrato; el administrador de ese tenant desactiva la membresía de Sofía; sus accesos previos permanecen en el registro de auditoría del tenant, sus sesiones se cierran y ninguna credencial de integración personal sobrevive (`USER_DEACTIVATED`).

---

**UJ-7. Diego crea la organización, invita al equipo y comprueba que el registro de auditoría es inviolable.**

- **Persona + contexto:** Diego Paredes, responsable de TI de Financiera Altiplano, `Organization Administrator`. Debe dejar la plataforma lista en un día laborable (contramétrica SM-C8) y responder al CISO "¿cómo sé que nadie alteró un registro?".
- **Estado de entrada:** recibe la invitación del administrador de plataforma tras la creación del tenant con `data_region = eu-west` `[ASSUMPTION]` e idioma por defecto español.
- **Camino:**
  1. Activa su cuenta (contraseña + MFA TOTP) y configura el proveedor de identidad corporativo (OIDC).
  2. Crea unidades de negocio (Crédito, Atención al cliente, TI), ubicaciones y equipos; invita a Lucía, Rodrigo, Carmen, Javier y Marta con sus roles; los usuarios reciben el correo en su idioma preferido.
  3. Ajusta la configuración del tenant: escala de riesgo por defecto 5×5, clasificación de hallazgos, política de retención de evidencia (7 años) `[ASSUMPTION]`, notificaciones por email e in-app.
  4. Abre *Administración → Registro de auditoría* y ejecuta la verificación de la cadena de hashes del tenant: cada entrada incluye el hash de la anterior; la verificación devuelve "íntegra" y el último hash.
- **Clímax:** exporta el registro de auditoría del primer día en JSON con hashes y se lo entrega al CISO junto con la explicación de la cadena. La organización queda operativa el mismo día.
- **Resolución:** cada acción posterior de cualquier usuario del tenant se encadena al registro; Diego revisa trimestralmente los accesos (revisiones de acceso, F2).
- **Caso límite:** Diego intenta asignarse `soa.approve` para desbloquear una aprobación urgente; la plataforma lo permite solo si su rol lo admite, emite `USER_PERMISSION_CHANGED` con motivo obligatorio y la revisora ve el cambio de permisos en las entradas de la próxima revisión por la dirección.

---

**Capacidades que las trayectorias exigen → FR** (resumen; el detalle está en §6): UJ-1 → FR-059 a FR-067, FR-100; UJ-2 → FR-034 a FR-036, FR-041 a FR-047, FR-050 a FR-053, FR-126; UJ-3 → FR-071 a FR-077, FR-084; UJ-4 → FR-087 a FR-093, FR-094; UJ-5 → FR-098, FR-099, FR-100, FR-102, FR-113; UJ-6 → FR-001, FR-002, FR-004, FR-006, FR-042, FR-090; UJ-7 → FR-001 a FR-007, FR-084, FR-132.

## 5. Glosario

El glosario canónico del producto es `docs/product/GLOSSARY.md` (26 términos de la cláusula 3, términos usados sin definición formal, ISO 19011:2026, ISO/IEC 23894, términos de producto y trampas de traducción). Este PRD lo usa **verbatim**; introducir un sinónimo es una violación de disciplina. Se reproducen aquí solo los términos cuyo uso preciso condiciona la lectura de los FR:

- **SGIA / AIMS** — sistema de gestión de la inteligencia artificial; en UI, con el término completo en la primera aparición.
- **Tenant** — unidad de aislamiento de datos; aloja una organización cliente; `tenant_id` + Row Level Security en toda tabla de negocio.
- **Catálogo de controles / catálogo de requisitos** — conocimiento normativo global, versionado (`StandardVersion`), de solo lectura para los tenants: `Requirement`, `Control`, `ControlGuidance`.
- **Estado del tenant** — `RequirementImplementation` (estado de un requisito en un tenant), `SoAItem` (decisión de aplicabilidad sobre un control), `ControlImplementation` (estado operativo de un control activo). Separación catálogo/estado pendiente de ratificación (§17 D-3).
- **Declaración de aplicabilidad (SoA)** — documentación de todos los controles necesarios y de la justificación de inclusión o exclusión (42001 cl. 3.26); en el producto, entidad `StatementOfApplicability` versionada.
- **Activación de control** — transición de un `SoAItem` a `Applicable` que dispara por evento flujos, tareas, solicitudes de evidencia y métricas.
- **Evidencia aceptada** — evidencia en estado `Accepted` tras revisión de un revisor autorizado distinto del que la subió; subir no es aceptar.
- **Procedencia de la evidencia** — origen, método de recogida, recogido por, fecha, hash, versión, revisor y fecha de revisión; para evidencia automatizada, además conector, endpoint, ID de objeto fuente, transformación y versión del colector.
- **Hallazgo (`Finding`)** — entidad genérica con `source_type` (audit, control_test, impact, incident, data_quality, ai_gap_analysis); `AuditFinding` e `ImpactAssessmentFinding` son especializaciones (§17 D-4).
- **Proveedor de IA (`AIProvider`)** — proveedor tecnológico del modelo o servicio. **`Supplier`** — tercero contractual (A.10.3). **`Integration`** — conector configurado del hub. Son tres entidades distintas (§17 D-4).
- **Puntuación transparente** — métrica que expone fórmula, numerador, denominador, exclusiones, registros fuente y marca temporal; vocabulario permitido: *readiness, coverage, implementation, effectiveness, exposure*.
- **Preparación para certificación** — estados `Ready for internal review`, `Audit-ready`, `Certification preparation status`; nunca "certificado".
- **Artefacto generado por IA** — contenido del motor de asistencia marcado con modelo, hora, prompt, fuentes, revisor y aprobación; nunca cambia un estado de cumplimiento por sí mismo.
- **`SOURCE_DETAIL_REQUIRED`** — marcador de la semilla para detalle normativo ausente del corpus; nunca se rellena inventando.
- **Tipo de registro normativo (`record_type`)** — `normative_requirement`, `implementation_guidance`, `recommended_practice`, `evidence_example`.
- **Cambio significativo** — cambio de modelo, versión, dataset, proveedor, caso de uso, despliegue, incidente, requisito regulatorio o degradación detectada que exige reevaluación (8.2, 8.4).
- **Trampas de traducción vinculantes:** *safety* = protección, *security* = seguridad, *accountability* = rendición de cuentas, *monitoring* = seguimiento, *deployment* = despliegue (del sistema de IA), *top management* = alta dirección, *statement of applicability* = declaración de aplicabilidad, *interested party* = parte interesada (entidad `InterestedParty`, nunca `Stakeholder`).

## 6. Requisitos funcionales

*Convenciones.* Cada FR indica: enunciado (actor puede capacidad bajo condiciones), `PR-*` cubiertos, fase, trayectorias que realiza y consecuencias verificables. Prioridad MoSCoW heredada de `REQUIREMENTS_ANALYSIS.md` salvo indicación. Las enumeraciones son canónicas en inglés y se traducen en la capa i18n. Todo FR de MVP recorre `UI → API → Domain Service → Database → Audit Log` (NFR-011); ningún FR se considera hecho con UI sin backend. Los eventos citados son los de `DOMAIN_MAP.md` §5.

### 6.1 Fundación: organización, tenencia, identidad, RBAC y administración

**Descripción.** Todo lo demás se apoya aquí. Un tenant aloja una organización cliente con su estructura, usuarios, roles, configuración y registro de auditoría propio. La regla de oro es el aislamiento: ningún dato cruza tenants ni por UI, ni por API, ni por informes, ni por búsqueda. Realiza UJ-6 y UJ-7. *Bounded contexts:* Identity & Organization, Administration, Workflow.

#### FR-001: Crear y estructurar la organización (tenant)
Un `Platform Administrator` puede crear un tenant con `Organization`, `data_region` e idioma por defecto; un `Organization Administrator` puede modelar `BusinessUnit`, `Department`, `Location`, `Team` y `Membership`. Cubre PR-XC-002, PR-NFR-011 (campo). Fase MVP. Realiza UJ-7, UJ-6.
- Un tenant se crea con `data_region` obligatorio y emite `TENANT_CREATED`.
- Un usuario puede pertenecer a varios tenants mediante `Membership`; el selector de organización solo lista los tenants con membresía activa.
- Las unidades y ubicaciones son referenciables desde el alcance del SGIA (FR-027).

#### FR-002: Aislar los datos por tenant
El sistema garantiza que toda consulta y escritura de datos de negocio queda acotada al tenant de la sesión mediante `tenant_id` en toda tabla de negocio, Row Level Security en PostgreSQL y prefijo de tenant en el almacenamiento de objetos. Cubre PR-XC-003, PR-EXT-009, PR-NFR-004. Fase MVP. Realiza UJ-6, UJ-7.
- Una prueba de seguridad automática que intenta leer, listar, buscar o exportar registros de otro tenant con credenciales válidas obtiene 0 filas o `404`, nunca `403` con filtración de existencia.
- Las URLs de descarga de evidencia son firmadas, de corta vida y verifican el `tenant_id` del objeto.
- La búsqueda (FR-123) y los informes (FR-105) heredan el aislamiento sin código específico.

#### FR-003: Autenticar usuarios y gestionar sesiones
Un usuario puede autenticarse con cuenta local (correo + contraseña + MFA TOTP) o con el proveedor de identidad de su organización vía OIDC; SAML en Fase 2. Cubre PR-XC-001, PR-EXT-007. Fase MVP* (SAML F2). Realiza UJ-7.
- MFA es configurable como obligatorio por tenant y por rol; los roles `Executive`, `Organization Administrator` y `Platform Administrator` lo exigen por defecto `[ASSUMPTION]`.
- Un administrador puede cerrar remotamente todas las sesiones de un usuario; la desactivación (FR-005) cierra sesiones en ≤ 60 s.
- Las contraseñas locales cumplen una política configurable con mínimo de 12 caracteres `[ASSUMPTION]`.

#### FR-004: Autorizar por roles y permisos granulares
Un `Organization Administrator` puede asignar a cada usuario uno o varios de los 12 roles por defecto (`Platform Administrator`, `Organization Administrator`, `AI Governance Manager`, `Risk Manager`, `Compliance Manager`, `Auditor`, `AI System Owner`, `Control Owner`, `Evidence Owner`, `Reviewer`, `Executive`, `Read Only`) y crear roles propios a partir del catálogo de permisos de P2 §4 (`organization.*`, `users.*`, `roles.manage`, `standards.*`, `requirements.*`, `controls.*` incl. `activate`/`deactivate`, `soa.*` incl. `approve`, `risks.*` incl. `approve`/`accept`, `assets.*`, `ai_systems.*`, `assessments.*` incl. `approve`, `evidence.*` incl. `upload`/`approve`/`delete`, `audits.*` incl. `execute`, `reports.*`, `integrations.*` incl. `sync`, `policies.*` incl. `generate`/`approve`, `management_review.*`). Cubre PR-XC-004. Fase MVP* (roles por unidad de negocio y por sistema en F2; ABAC opcional en F3). Realiza UJ-6, UJ-7.
- La matriz rol × permiso por defecto es la de `addendum.md` §B `[ASSUMPTION, conflicto C15, pendiente de aprobación humana §17 D-6]`.
- Toda operación de API verifica el permiso a nivel de objeto (no solo de recurso); una prueba de autorización rota a nivel de objeto falla el pipeline.
- El rol `Read Only` no puede ejecutar ninguna mutación; el rol `Auditor` no puede aprobar evidencia ni SoA del alcance que audita (independencia, ISO 19011).
- Cambiar roles o permisos exige motivo y emite `USER_PERMISSION_CHANGED`.

#### FR-005: Gestionar el ciclo de vida del usuario
Un `Organization Administrator` puede invitar, activar, desactivar y reactivar usuarios, y (F2) ejecutar revisiones de acceso periódicas con evidencia. Cubre PR-XC-005. Fase MVP* (revisiones de acceso F2). Realiza UJ-7, UJ-6.
- Una invitación expira a los 7 días `[ASSUMPTION]` y emite `USER_INVITED`; la activación emite `USER_ACTIVATED`.
- La desactivación emite `USER_DEACTIVATED`, cierra sesiones y revoca credenciales de integración personales; el historial del usuario permanece en el registro de auditoría.
- Ningún tenant puede quedar sin al menos un `Organization Administrator` activo.

#### FR-006: Configurar el tenant
Un `Organization Administrator` puede configurar región de datos (solo lectura tras creación), idioma por defecto, escala de riesgo por defecto, clasificación de hallazgos, políticas de retención por clase, reglas de notificación y feature flags de módulos. Cubre PR-NFR-011, PR-XC-012, PR-CAP-031, PR-DR-022. Fase MVP. Realiza UJ-6, UJ-7.
- Cada cambio de configuración queda en el registro de auditoría con valor anterior y nuevo.
- Los módulos deshabilitados por feature flag desaparecen de la navegación (FR-131).

#### FR-007: Registrar toda acción de gobernanza en un registro de auditoría inviolable
El sistema registra, en una tabla append-only con cadena de hashes por tenant, cada evento de gobernanza (al menos los 20 de P2 §41 normalizados en `DOMAIN_MAP.md` §5) con actor, marca temporal, acción, objeto, estado anterior, estado nuevo, motivo, metadatos de IP/sesión y `correlation_id`; un usuario con `organization.read` puede consultarlo, filtrarlo, verificar la cadena y exportarlo. Cubre PR-XC-006, PR-NFR-010, PR-DR-017, PR-XC-007 (outbox como fuente). Fase MVP. Realiza UJ-7, UJ-1, UJ-5.
- Cada entrada incluye el hash de la anterior; la operación "verificar cadena" recorre el tenant completo y devuelve `intacta` o la primera entrada rota.
- No existe API ni migración que actualice o borre entradas; una prueba que lo intenta falla.
- Toda decisión de cumplimiento del flujo E2E (§16) aparece en el registro con actor, motivo y valor anterior en ≤ 5 s desde su commit (publicación vía outbox).
- Exportación a JSON y CSV con hashes incluidos.

#### FR-008: Retener, archivar y borrar de forma gobernada
Un `Organization Administrator` puede definir políticas de retención, expiración y archivado por clase de evidencia y documento, aplicar `legal hold` a registros o conjuntos, y solicitar borrado mediante flujo aprobado. Cubre PR-XC-018, PR-NFR-012, PR-DR-015, PR-CAP-068. Fase MVP* (sin borrado físico en MVP; `legal hold` y borrado gobernado F2).
- En MVP ninguna operación borra físicamente registros de cumplimiento: "borrar" archiva (`deleted_at`) y registra actor y motivo.
- Un registro bajo `legal hold` rechaza expiración, archivado y borrado con mensaje explícito.
- Retención por defecto de evidencia y registros de cumplimiento: 7 años `[ASSUMPTION]`, configurable.

#### FR-009: Versionar y conservar el historial de los objetos importantes
El sistema mantiene, para toda entidad importante, los campos `id, tenant_id, created_at, created_by, updated_at, updated_by, status, version, deleted_at` y un historial navegable de versiones. Cubre PR-XC-016. Fase MVP.
- Cualquier objeto versionado permite ver quién cambió qué y cuándo, y comparar dos versiones.
- Las entidades con versionado formal (alcance, SoA, política, documento, evaluación de impacto, metodología de riesgo) crean una versión nueva en cada aprobación.

#### FR-010: Comentar y colaborar sobre cualquier objeto de cumplimiento
Un usuario con permiso de lectura sobre un objeto puede añadir comentarios contextuales; los comentarios son parte del historial y no se borran. Cubre PR-XC-017. Fase MVP (Could).
- Un comentario queda asociado a objeto, autor y fecha; mencionar a un usuario genera notificación in-app.

#### FR-011: Exponer una API documentada por dominio
Un integrador autenticado puede usar una API tipada organizada por dominio (27 recursos de P2 §43, de `/api/organizations` a `/api/management-reviews`) con esquemas, errores consistentes, paginación, filtrado, ordenación, autorización por permiso y registro de auditoría. Cubre PR-XC-013. Fase MVP.
- La especificación OpenAPI se genera desde los esquemas Zod y se publica con la aplicación.
- Toda mutación por API deja la misma huella en el registro de auditoría que la misma acción por UI.

#### FR-012: Procesar trabajo en segundo plano de forma fiable
El sistema ejecuta en segundo plano (jobs reintentables, idempotentes, observables y auditables) la generación de informes y documentos, notificaciones, cálculo de puntuaciones y KPI, detección de evidencia y aceptaciones por expirar, reevaluaciones programadas y, en F2, sincronizaciones y descubrimiento. Cubre PR-XC-014, PR-XC-019. Fase MVP.
- Cada job registra `correlation_id`, intentos, duración y resultado; un job fallido tres veces genera alerta operativa.
- El estado de un informe o exportación pedido por el usuario es consultable (`Queued`, `Running`, `Ready`, `Failed`).

#### FR-013: Importar y exportar registros
Un usuario con permiso de gestión puede exportar registros de su tenant en CSV y JSON (MVP) y, en F2, importar con el flujo `Upload → Validate → Preview → Map Fields → Detect Duplicates → Confirm → Import → Report` y exportar en XLSX, PDF, DOCX y Markdown. Cubre PR-CAP-150, PR-DR-016. Fase MVP* (exportación CSV/JSON en MVP; importación F2).
- Ninguna importación sobreescribe datos sin previsualización, detección de duplicados y confirmación explícita.
- Toda exportación respeta permisos y clasificación; queda registrada.

#### FR-014: Detectar problemas de calidad de datos de gobernanza
El sistema ejecuta comprobaciones programadas (propietarios ausentes, controles huérfanos, riesgos sin tratamiento, controles sin evidencia, sistemas sin evaluación, evidencia expirada, relaciones de alcance ausentes, sistemas y proveedores duplicados, evaluaciones obsoletas) y muestra un panel de calidad de datos. Cubre PR-CAP-155. Fase F2.
- Cada problema detectado emite `DATA_QUALITY_ISSUE_DETECTED` y puede convertirse en tarea o hallazgo (`source_type = data_quality`).

### 6.2 Motor de normas y requisitos

**Descripción.** El conocimiento normativo es global, versionado y de solo lectura para los tenants; entra en la base de datos únicamente a través de la semilla estructurada (`packages/standards-seed/`) construida desde `resumenes/` y revisada por humanos. Nunca contiene texto literal de ISO/IEC/UNE. Cada tenant mantiene su propio estado de implementación por requisito. *Bounded contexts:* Standards & Compliance, AIMS.

#### FR-015: Modelar las normas de forma versionada
El sistema representa `Standard`, `StandardVersion`, `Clause`, `SubClause`, `Requirement`, `Annex`, `ControlDomain`, `Control`, `ControlGuidance`, `RequirementControlMapping` y `StandardRelationship`, con soporte desde el inicio para ISO/IEC 42001:2023 + Amd 1:2024, UNE-ISO/IEC 42001:2025 (adopción idéntica), ISO 19011:2026 e ISO/IEC 23894:2023 (parcial), y registra ISO/IEC 42006 como dependencia pendiente. Cubre PR-CAP-001, PR-CAP-006. Fase MVP.
- Publicar una nueva `StandardVersion` no altera el estado de cumplimiento de ningún tenant; emite `STANDARD_VERSION_PUBLISHED`.
- Un tenant puede ver qué versión de la norma está usando su SoA y su lista de requisitos.

#### FR-016: Representar cada requisito de las cláusulas 4 a 10 con ficha completa
Un usuario con `requirements.read` puede consultar, por requisito, identificador, norma, versión, cláusula, subcláusula, título, resumen (paráfrasis), propósito, aplicabilidad, guía de implementación, evidencia esperada, rol responsable sugerido, relaciones y notas. Cubre PR-CAP-002. Fase MVP.
- La semilla contiene cada subcláusula de 4.1 a 10.2 presente en `resumenes/01 §4`, no solo los títulos de cláusula.
- Todo requisito muestra su documento fuente (`resumenes/NN §sección`) (nodo [0] de la trazabilidad, A10).

#### FR-017: Distinguir lo normativo de lo orientativo
El sistema etiqueta cada registro normativo con `record_type` ∈ {`normative_requirement`, `implementation_guidance`, `recommended_practice`, `evidence_example`} y lo muestra diferenciado en toda la UI. Cubre PR-CAP-007, PR-DR-018. Fase MVP.
- La orientación del Anexo B se carga como `implementation_guidance` asociada a su control y nunca aparece como requisito ni exige justificación en la SoA.
- Un filtro "solo requisitos normativos" está disponible en todas las listas de requisitos y controles.

#### FR-018: Sembrar el conocimiento normativo de forma determinista y auditable
El sistema carga la semilla versionada con metadatos de las normas, cláusulas 4-10, los 38 controles del Anexo A con sus 9 dominios, la orientación del Anexo B, los 11 objetivos y 7 fuentes de riesgo del Anexo C, la estructura disponible de ISO/IEC 23894 e ISO 19011:2026, roles, escalas de riesgo, plantilla de impacto, ciclo de vida por defecto, informes y dashboards de ejemplo; usa `SOURCE_DETAIL_REQUIRED` donde falte detalle y cita `resumenes/NN §sección` en cada registro. Cubre PR-CAP-005, PR-NFR-020, PR-DR-019, PR-NFR-015. Fase MVP* (informes y dashboards de ejemplo completos en F2).
- La semilla es idempotente: ejecutarla dos veces no duplica registros.
- Pruebas de dominio en verde: exactamente 38 controles con distribución A.2:3, A.3:2, A.4:5, A.5:4, A.6:9, A.7:5, A.8:4, A.9:3, A.10:3; 26 términos de la cláusula 3; 11 objetivos y 7 fuentes en el Anexo C.
- Ningún registro contiene texto literal de la norma (revisión humana registrada antes de publicar la versión de semilla).
- Un registro `SOURCE_DETAIL_REQUIRED` se muestra como tal en la UI y nunca se rellena automáticamente.

#### FR-019: Mantener relaciones explícitas entre requisitos, controles y objetos de cumplimiento
El sistema mantiene como mapeos explícitos (no texto libre) las relaciones requisito ↔ control ↔ riesgo ↔ política/procedimiento ↔ tarea ↔ KPI ↔ pregunta de auditoría, sembradas para requisito ↔ control y editables por el tenant para el resto. Cubre PR-CAP-008. Fase MVP*.
- Desde cualquier requisito se llega a sus controles mapeados y viceversa en un clic (FR-125).

#### FR-020: Mantener el estado de implementación de cada requisito por tenant
Un usuario con `requirements.manage` puede asignar propietario y estado a cada requisito de su tenant (`RequirementImplementation`) con los valores `Not Started`, `Planned`, `In Progress`, `Implemented`, `Partially Implemented`, `Not Applicable`, `Needs Review`, `Evidence Pending`, `Ready for Audit`, `Nonconforming`. Cubre PR-CAP-003. Fase MVP.
- `Not Applicable` solo se ofrece para registros `implementation_guidance` y para normas de apoyo; para un `normative_requirement` de ISO/IEC 42001 exige justificación y muestra advertencia de interpretación normativa `[ASSUMPTION, conflicto C5, pendiente de aprobación humana §17 D-2]`.
- El estado se recalcula con sugerencia (no cambio automático) al recibir `CONTROL_ACTIVATED`, `EVIDENCE_APPROVED`, `EVIDENCE_EXPIRED`, `AUDIT_FINDING_CREATED`, `CAPA_CLOSED`; el cambio efectivo lo confirma un humano y emite `REQUIREMENT_STATUS_CHANGED`.
- Un requisito no puede pasar a `Implemented` sin al menos una evidencia `Accepted` enlazada `[ASSUMPTION]`.

#### FR-021: Ofrecer un espacio de trabajo por requisito
Un usuario puede abrir la página de un requisito y responder sin cambiar de módulo las 16 preguntas de P2 §6: qué exige, por qué, quién es responsable, qué evidencia se espera, controles, riesgos, políticas, sistemas afectados, tareas, estado, evidencia recogida, preguntas de auditoría, hallazgos, CAPA y preparación. Cubre PR-CAP-004, PR-CAP-166 (texto). Fase MVP* (con las relaciones disponibles en cada fase).
- Cada bloque enlaza al objeto origen y muestra estado vacío explícito cuando no hay datos ("sin evidencia aceptada"), nunca un bloque en blanco.

#### FR-022: Mapear requisitos regulatorios externos
Un `Compliance Manager` puede cargar requisitos regulatorios externos (sin jurisdicción fijada) y mapearlos a requisitos ISO, controles, sistemas de IA y evidencia. Cubre PR-CAP-009, PR-EXT-015. Fase F2.
- Un cambio en un requisito regulatorio emite `REGULATORY_REQUIREMENT_CHANGED` y marca para revisión los riesgos y controles mapeados.
- La plataforma no interpreta leyes: el texto y la obligación los carga la organización.

#### FR-023: Instalar paquetes de dominio sectorial
Un `Organization Administrator` puede activar paquetes sectoriales (riesgos, preguntas, plantillas de evidencia, controles recomendados, informes) etiquetados como guía organizacional. Cubre PR-CAP-010, PR-DR-025. Fase F3.
- Ningún elemento de un paquete se muestra como requisito ISO ni entra en la SoA como control del Anexo A.

#### FR-024: Mantener un banco de preguntas de auditoría por requisito y control
Un `Auditor` puede consultar y ampliar el banco de preguntas de auditoría sembrado por requisito y control; en F2 la IA propone preguntas marcadas como sugerencia. Cubre PR-CAP-090. Fase MVP* (Could).
- Toda pregunta enlaza al requisito o control que verifica.

### 6.3 SGIA: contexto, partes interesadas, alcance, política, roles y objetivos

**Descripción.** Las cláusulas 4, 5 y 6.2 son el "Govern" del ciclo. Aquí la organización declara quién es respecto a la IA, a quién afecta, qué abarca su sistema de gestión, qué intención expresa la alta dirección y qué se propone medir. Realiza UJ-6 (creación por consultora) y UJ-7. *Bounded contexts:* AIMS, Documents & Policies, Objectives & KPIs.

#### FR-025: Registrar el contexto de la organización y sus roles respecto a la IA
Un `AI Governance Manager` puede registrar cuestiones internas y externas, contexto estratégico, tecnológico y regulatorio, panorama de IA, dependencias, fortalezas, debilidades, oportunidades y amenazas (estructura de `resumenes/11`), historial de revisión y los roles de la organización respecto a la IA (proveedor, productor, cliente, socio, sujeto, autoridad pertinente; 4.1 nota 1). Cubre PR-CAP-011, PR-DR-029. Fase MVP.
- Cada revisión del contexto emite `CONTEXT_REVIEWED` con fecha y revisor.
- Los roles declarados condicionan la aplicabilidad *sugerida* de requisitos y controles (nunca automática) `[ASSUMPTION heredada de PR-DR-029]`.
- Las frases sobre cambio climático de Amd 1:2024 (4.1, 4.2) están presentes como cuestiones a considerar.

#### FR-026: Gestionar las partes interesadas
Un `AI Governance Manager` puede registrar partes interesadas por tipo (empleados, clientes, reguladores, proveedores, socios tecnológicos, inversores, grupos afectados, otros) con necesidades, expectativas, influencia, requisitos pertinentes, necesidades de comunicación y fecha de revisión (estructura de `resumenes/12`). Cubre PR-CAP-012. Fase MVP.
- La entidad es `InterestedParty`; la UI nunca dice "stakeholder".
- Una parte interesada puede enlazarse como parte afectada de un sistema de IA y de una evaluación de impacto.

#### FR-027: Diseñar, aprobar y versionar el alcance del SGIA
Un `AI Governance Manager` puede componer el alcance desde unidades de negocio, ubicaciones, sistemas de IA, procesos, productos, servicios, actividades del ciclo de vida y proveedores, declarar exclusiones justificadas y enviarlo a aprobación; un `Executive` u `Organization Administrator` con permiso lo aprueba, creando una versión. Cubre PR-CAP-013, PR-DR-017. Fase MVP. Realiza UJ-7 (previo a UJ-1).
- El alcance no se aprueba sin al menos una unidad de negocio y un sistema de IA `[ASSUMPTION]`; toda exclusión exige justificación.
- La aprobación emite `SCOPE_CHANGED`; los sistemas de IA fuera del alcance se marcan como tales y no entran en denominadores de puntuación.
- Comparación entre versiones de alcance disponible.

#### FR-028: Mostrar el centro de control del SGIA
Un usuario con permiso de lectura puede ver en una sola vista el estado del SGIA: alcance vigente, política, objetivos, riesgos, controles, evidencia, auditorías y mejoras, con navegación de detalle. Cubre PR-CAP-014. Fase MVP (Should).
- Cada indicador del centro de control es una puntuación transparente (FR-100) o un contador con enlace a la lista fuente.

#### FR-029: Gestionar la política de IA como información documentada con ciclo de vida
Un `Compliance Manager` puede crear la política de IA como `Policy`/`PolicyVersion` con el ciclo canónico `Draft`, `AI Generated` (solo F2), `Under Review`, `Pending Approval`, `Approved`, `Published`, `Superseded`, `Archived`, enlazarla a otras políticas (A.2.3) y programar su revisión (A.2.4); un `Executive` con `policies.approve` la aprueba. Cubre PR-CAP-015, PR-CAP-070. Fase MVP `[decisión provisional C2, §17 D-1]`.
- La aprobación emite `POLICY_APPROVED` y crea la versión; la política aprobada aparece en la lista de preparación (5.2).
- Una política `Approved` con fecha de revisión vencida emite `POLICY_REVIEW_DUE`.
- En MVP no existe generación por LLM; el estado `AI Generated` no es alcanzable.

#### FR-030: Definir roles, responsabilidades y comité de gobernanza
Un `AI Governance Manager` puede mantener la matriz RACI del SGIA, el comité de gobernanza de IA y las asignaciones nominales (AI System Owner, Model Owner, Data Owner, Human Supervisor, Risk Owner, Control Owner, Evidence Owner, Auditor) sobre usuarios reales. Cubre PR-CAP-017. Fase MVP (Should).
- Cada control `Applicable` y cada sistema de IA muestra su propietario tomado de estas asignaciones; un cambio de propietario emite evento y reasigna tareas abiertas.

#### FR-031: Establecer objetivos de la IA medibles
Un `AI Governance Manager` puede registrar `AIObjective` coherentes con la política, con qué se hará, recursos, responsable, plazo y método de evaluación, y `AIObjectiveMetric` con valor objetivo y valores medidos (manuales en MVP). Cubre PR-CAP-018, PR-DR-030. Fase MVP (Should) `[decisión provisional C3, §17 D-1]`.
- Un objetivo sin responsable o sin método de evaluación se guarda como borrador y no cuenta en la lista de preparación (6.2).
- Los objetivos y sus métricas son entrada automática de la revisión por la dirección (FR-099).

#### FR-032: Gestionar cambios del SGIA de forma planificada
Un `AI Governance Manager` puede abrir una `ChangeRequest` y recorrer `Change Request → Impact Analysis → Risk Reassessment → Impact Reassessment → Control Review → Approval → Implementation → Evidence`. Cubre PR-CAP-019. Fase F2.
- Un `MODEL_VERSION_CHANGED` o `AI_SYSTEM_CHANGED` significativo propone abrir una solicitud de cambio.

#### FR-033: Gestionar competencia y formación
Un `Organization Administrator` puede definir requisitos de competencia por rol de IA, cursos, registros de formación con evidencia y expiración, y ver brechas de competencia (7.2, 7.3, A.4.6). Cubre PR-CAP-140. Fase F2.
- Un registro de formación expirado emite notificación y aparece como brecha.

### 6.4 Inventario de IA y activos

**Descripción.** El inventario es el "Discover" del ciclo y el eje de toda evaluación: no hay riesgo ni impacto sin sistema de IA. Modela el sistema, sus modelos y versiones, datos, despliegues, proveedores tecnológicos y terceros contractuales, y (F2/F3) descubrimiento automático y agentes. Realiza UJ-2. *Bounded context:* AI Portfolio.

#### FR-034: Registrar un sistema de IA con ficha completa
Un `AI System Owner` puede crear y mantener un `AISystem` con los atributos de P2 §11: nombre, descripción, propósito de negocio, propietarios (negocio y técnico), proveedor, modelo y versión, despliegue, entorno, geografía, datos y clasificación, datos personales/sensibles, usuarios, partes afectadas, nivel de automatización, tipo de decisión, supervisión humana, uso previsto, uso prohibido, mal uso razonablemente previsible, clasificación de riesgo e impacto, estado de ciclo de vida, dependencias, contratos, controles, incidentes, seguimiento y evidencia. Cubre PR-CAP-020. Fase MVP. Realiza UJ-2.
- Crear un sistema emite `AI_SYSTEM_CREATED`; modificar campos significativos emite `AI_SYSTEM_CHANGED`.
- Los campos obligatorios mínimos para entrar en el alcance son: propósito, propietario de negocio, uso previsto, mal uso previsible, supervisión humana, partes afectadas `[ASSUMPTION]`.
- La ficha muestra la cadena riesgos → controles → evidencia del sistema (FR-125).

#### FR-035: Modelar los activos relacionados como entidades propias
El sistema representa `AIAsset`, `AIModel`, `AIModelVersion`, `AIProvider`, `AIDeployment`, `AIDataset`, `AIUseCase`, `AIProject`, `AIDataSource`, `AIIntegration` y sus relaciones con el sistema de IA. Cubre PR-CAP-021. Fase MVP* (UI dedicada para modelo, versión, proveedor, dataset y despliegue; `AIUseCase`, `AIProject`, `AIDataSource`, `AIIntegration` solo en esquema en MVP).
- Cambiar la versión de un modelo emite `MODEL_VERSION_CHANGED`; cambiar un dataset emite `DATASET_CHANGED`.
- Un activo puede compartirse entre varios sistemas de IA del mismo tenant.

#### FR-036: Distinguir proveedor tecnológico, tercero contractual y cliente
El sistema modela `AIProvider` (proveedor tecnológico del modelo o servicio), `Supplier` (tercero contractual, A.10.3), `Customer` (A.10.4) y `Contract` como entidades distintas que pueden apuntar a la misma organización externa; `Integration` (FR-114) es otra cosa. Cubre PR-CAP-021 `[decisión provisional C14, §17 D-4]`. Fase MVP* (`Supplier`, `Customer`, `Contract` en esquema; UI mínima).
- Un `Supplier` puede tener requisitos, seguimiento y acciones correctivas asociadas (A.10.3); un cambio relevante emite `SUPPLIER_CHANGED`.

#### FR-037: Visualizar el grafo de relaciones de activos
Un usuario puede ver gráficamente proceso de negocio → sistema de IA → modelo, versión, dataset, proveedor, API, infraestructura, usuarios, propietario, riesgos, controles, evidencia, incidentes y contratos. Cubre PR-CAP-022. Fase F2.
- El grafo se genera desde las relaciones explícitas; ningún nodo se dibuja sin registro fuente.

#### FR-038: Gobernar agentes de IA como sistemas de IA de primera clase
Un `AI System Owner` puede registrar un `Agent` con sub-agentes, herramientas y permisos, memoria, nivel de autonomía, acciones permitidas y prohibidas, sistemas externos, aprobación humana, límites de gasto y ejecución, *kill switch*, logs, modelo, prompts y versiones. Cubre PR-CAP-023. Fase F3.
- Un agente hereda todos los flujos de un sistema de IA (riesgo, impacto, controles, evidencia) más los atributos propios.

#### FR-039: Descubrir sistemas de IA candidatos desde plataformas conectadas
El sistema ejecuta `CONNECT → DISCOVER → NORMALIZE → CLASSIFY → MATCH → CREATE/UPDATE ASSET → MAP REQUIREMENTS → MAP CONTROLS → IDENTIFY RISKS → REQUEST HUMAN VALIDATION`, crea `CandidateAISystem` y solicita validación humana. Cubre PR-CAP-024, PR-DR-012. Fase F2.
- Un candidato nunca es un sistema del inventario hasta que una persona con `ai_systems.manage` lo confirma (alcance, propietario, uso previsto, datos, partes afectadas, riesgo, supervisión); emite `CANDIDATE_AI_SYSTEM_DISCOVERED` y crea tarea.
- El descubrimiento nunca marca cumplimiento ni cambia estados de requisitos o controles.

#### FR-040: Disponer de una organización de demostración realista
El sistema incluye un tenant demo etiquetado ("Demo Financial Services") con varios sistemas de IA (soporte al cliente, detección de fraude, decisión de crédito, propensión, asistente de código, clasificador documental, asistente generativo, agente), proveedores, riesgos alto/medio/bajo, unidades, controles, evidencia, incidentes (F2), hallazgos, políticas, informes y una revisión por la dirección, derivados de `resumenes/`. Cubre PR-CAP-026, PR-DR-020. Fase MVP (Should; último cambio del MVP).
- Toda pantalla del tenant demo muestra la etiqueta "Demo"; sus datos nunca se agregan con los de tenants reales ni cuentan para los criterios de salida del MVP.

### 6.5 Riesgo de la IA

**Descripción.** El "Assess" desde la perspectiva de la organización (6.1.2, 6.1.3, 8.2, 8.3; ISO/IEC 23894 como apoyo). La metodología es del tenant; la plataforma no impone un método universal. Las reglas duras: riesgo tratado ≠ plan existente; aceptación con expiración; reevaluación ante cambio significativo. Realiza UJ-2. *Bounded context:* Risk.

#### FR-041: Documentar y aprobar la metodología y los criterios de riesgo de la IA
Un `Risk Manager` puede registrar la `RiskMethodology` del tenant (criterios de aceptabilidad, escalas, apetito, frecuencia de revisión) y enviarla a aprobación como información documentada (6.1.1, 6.1.2). Cubre PR-CAP-037. Fase MVP.
- Sin metodología `Approved` no se puede aprobar ningún plan de tratamiento ni aceptar riesgos; la lista de preparación marca "metodología de riesgo aprobada" solo entonces.

#### FR-042: Configurar escalas de puntuación por tenant
Un `Risk Manager` puede elegir escalas 3×3, 4×4, 5×5 o personalizadas para probabilidad e impacto y, en F2, dimensiones adicionales (severidad, velocidad, detectabilidad, confianza, incertidumbre). Cubre PR-CAP-031. Fase MVP* . Realiza UJ-6.
- Cambiar la escala con riesgos ya puntuados exige confirmación, conserva los valores históricos y marca los riesgos como "revisión requerida".

#### FR-043: Mantener el registro de riesgos de IA
Un `Risk Manager` o `AI System Owner` puede crear riesgos con fuente de riesgo (catálogo semilla del Anexo C: C.3.1-C.3.7 y objetivos C.2.1-C.2.11, ampliable), evento, causa, consecuencia, partes afectadas, sistema y activo afectados, controles existentes, probabilidad, impacto, riesgo inherente, tratamiento, riesgo residual, propietario, fecha objetivo, aceptación, seguimiento y revisión (estructura compatible con `resumenes/10`). Cubre PR-CAP-030, PR-CAP-038. Fase MVP. Realiza UJ-2.
- Crear un riesgo emite `RISK_CREATED`; puntuarlo, `RISK_SCORED`; superar el umbral configurado, `RISK_ESCALATED`.
- Todo riesgo identificado puede reflejarse en la SoA con sus controles (3.26 nota 2): la SoA muestra riesgos sin control mapeado.

#### FR-044: Calcular riesgo inherente y residual con la escala del tenant
El sistema calcula el riesgo inherente a partir de probabilidad × impacto según la escala configurada y el residual a partir de la eficacia declarada o probada de los controles mapeados; ambos exponen su fórmula. Cubre PR-CAP-032. Fase MVP.
- La eficacia de un control nunca se presume por existir: un control `Planned` o sin evidencia aceptada no reduce el residual `[ASSUMPTION sobre el algoritmo, a detallar en arquitectura]`.
- `CONTROL_DEACTIVATED` o `CONTROL_TEST_FAILED` recalculan el residual de los riesgos afectados.

#### FR-045: Tratar el riesgo con un plan aprobado
Un `Risk Manager` puede elegir `Avoid`, `Mitigate`, `Transfer/Share` o `Accept/Retain`, redactar el `RiskTreatmentPlan` con controles, responsable y plazos, y obtener la aprobación de la gerencia designada (`risks.approve`). Cubre PR-CAP-034, PR-DR-005. Fase MVP. Realiza UJ-2.
- La aprobación emite `RISK_TREATMENT_APPROVED`.
- Un riesgo solo se muestra como "tratado" cuando tiene plan aprobado, controles en estado `Implemented` u `Operating` y residual evaluado y aceptado o dentro de criterios; en cualquier otro caso su estado es explícito ("plan aprobado, controles pendientes").

#### FR-046: Aceptar el riesgo residual con rol, justificación y expiración
Un usuario con `risks.accept` puede aceptar un riesgo residual con justificación, fecha de expiración/revisión y referencia al residual vigente; el historial de aceptaciones se conserva. Cubre PR-CAP-035, PR-DR-006. Fase MVP. Realiza UJ-2.
- La aceptación emite `RISK_ACCEPTED`; 30 días antes del vencimiento `[ASSUMPTION]` se emite `RISK_ACCEPTANCE_EXPIRING` y se crea tarea; vencida, `RISK_ACCEPTANCE_EXPIRED` y el riesgo vuelve a "revisión requerida".
- No existe aceptación sin fecha de expiración.

#### FR-047: Reevaluar el riesgo ante cambios significativos
El sistema marca un riesgo como "reevaluación requerida" y abre revisión de sus controles cuando recibe `AI_SYSTEM_CHANGED`, `MODEL_VERSION_CHANGED`, `DATASET_CHANGED`, `INCIDENT_CREATED`, `IMPACT_ASSESSMENT_COMPLETED`, `REGULATORY_REQUIREMENT_CHANGED` o una degradación detectada, además de a intervalos planificados. Cubre PR-CAP-036, PR-DR-007. Fase MVP* (cambio de modelo/versión e intervalo manual en MVP; el resto de disparadores según llegan sus módulos). Realiza UJ-2 (caso límite).
- El disparo emite `RISK_REASSESSMENT_REQUIRED` con el evento causante; la ficha del riesgo muestra "pendiente de reevaluación desde <evento>".

#### FR-048: Calcular tendencia y estado frente al apetito de riesgo
El sistema calcula la tendencia de cada riesgo y del agregado, y el `Risk Appetite Status` frente al apetito declarado en la metodología. Cubre PR-CAP-033. Fase F2.

#### FR-049: Sugerir riesgos a partir de las características del sistema
El motor de asistencia puede proponer riesgos candidatos a partir de la ficha del sistema; se crean como sugerencias marcadas, nunca como riesgos aprobados. Cubre PR-CAP-039. Fase F2.

### 6.6 Evaluación del impacto del sistema de IA

**Descripción.** El "Assess" desde la perspectiva de las personas y la sociedad (6.1.4, 8.4, A.5), proceso distinto del riesgo y enlazado con él: lo más distintivo de 42001 frente a 27001. Realiza UJ-2. *Bounded context:* Impact.

#### FR-050: Ejecutar evaluaciones de impacto con plantillas
Un `AI System Owner` puede crear una `ImpactAssessment` desde una `AssessmentTemplate`, responder preguntas, adjuntar evidencia, registrar hallazgos de impacto con severidad, mitigación y propietario, y enviarla a aprobación (`assessments.approve`). Cubre PR-CAP-040. Fase MVP. Realiza UJ-2.
- Completar emite `IMPACT_ASSESSMENT_COMPLETED`; aprobar, `IMPACT_ASSESSMENT_APPROVED`; ambos quedan en el registro de auditoría.
- Cada hallazgo de impacto es una especialización de `Finding` (`source_type = impact`) y puede derivar en acción correctiva.

#### FR-051: Cubrir todas las dimensiones de impacto con la plantilla semilla
La plantilla por defecto cubre individuos, grupos, sociedad, privacidad, equidad, transparencia, protección (*safety*), seguridad (*security*), derechos humanos, accesibilidad, medio ambiente, impacto económico y social, decisiones automatizadas, supervisión humana y mal uso razonablemente previsible (A.5.4, A.5.5). Cubre PR-CAP-041. Fase MVP.
- Las enumeraciones `safety` y `security` son distintas y se etiquetan en ES como "protección (safety)" y "seguridad (security)" (conflicto C12).

#### FR-052: Versionar las evaluaciones y enlazarlas a la etapa del ciclo de vida
Cada evaluación aprobada crea una versión y registra la etapa del ciclo de vida del sistema en la que se realizó; una nueva evaluación tras un cambio significativo referencia la anterior. Cubre PR-CAP-042. Fase MVP* (etapa como campo en MVP; ciclo de vida completo F2).
- `MODEL_VERSION_CHANGED` emite `IMPACT_REASSESSMENT_REQUIRED` para las evaluaciones vigentes del sistema.

#### FR-053: Considerar el impacto en la evaluación del riesgo
Los resultados aprobados de una evaluación de impacto quedan disponibles y enlazados en la evaluación de riesgo del mismo sistema. Cubre PR-CAP-044, PR-DR-028. Fase MVP (Should). Realiza UJ-2.
- Una evaluación de riesgo no puede marcarse completa si existe una evaluación de impacto posterior no revisada; la UI explica el bloqueo.

#### FR-054: Asistir la evaluación de impacto con IA
El motor de asistencia puede sugerir preguntas y áreas de impacto adicionales, marcadas como sugerencia. Cubre PR-CAP-043. Fase F2.

### 6.7 Ciclo de vida del sistema de IA

**Descripción.** Etapas y puertas configurables que impiden avanzar un sistema sin sus evaluaciones, controles, evidencia y aprobaciones (A.6.1.3, A.6.2.2 a A.6.2.8). En MVP existe el modelo y el campo de etapa; las puertas llegan en Fase 2. *Bounded context:* Lifecycle.

#### FR-055: Configurar las etapas del ciclo de vida
Un `AI Governance Manager` puede configurar etapas con la plantilla por defecto `Idea, Assessment, Design, Development, Training, Validation, Approval, Deployment, Production, Operation & Monitoring, Change, Retirement`. Cubre PR-CAP-045. Fase F2.
- La etapa `Monitoring` de P2 §12 se nombra `Operation & Monitoring` (etiqueta ES "operación y seguimiento") para alinearse con A.6.2.6 `[ASSUMPTION, conflicto C13]`.

#### FR-056: Definir puertas de ciclo de vida
Un `AI Governance Manager` puede definir por etapa una `LifecycleGate` con criterios de entrada, evidencia requerida, controles requeridos, revisión de riesgo, revisión de impacto, aprobación, criterios de salida y traza de auditoría (ejemplo: puerta de despliegue A.6.2.5). Cubre PR-CAP-046. Fase F2.

#### FR-057: Bloquear la progresión con puertas incompletas
El sistema impide avanzar de etapa si una puerta obligatoria está incompleta, salvo excepción autorizada y registrada (FR-070). Cubre PR-CAP-047, PR-DR-013. Fase F2.
- El bloqueo emite `LIFECYCLE_GATE_FAILED` con la lista de condiciones incumplidas; el paso emite `LIFECYCLE_GATE_PASSED` y `LIFECYCLE_STAGE_CHANGED`.

#### FR-058: Registrar revisiones de ciclo de vida
Un `AI System Owner` puede registrar `LifecycleReview` por etapa con participantes, resultado y acciones (A.6.2.6). Cubre PR-CAP-048. Fase F2 (Could).

### 6.8 Declaración de aplicabilidad y modelo operativo de controles

**Descripción.** El corazón del producto: la SoA compara la situación del tenant con los 38 controles del Anexo A, exige justificación de exclusiones y **activa por evento solo los flujos de los controles aplicables**. Un control no aplicable no genera nada, pero permanece visible y auditable. Realiza UJ-1. *Bounded context:* Controls & SoA.

#### FR-059: Disponer del catálogo completo del Anexo A
El sistema carga desde la semilla los 9 dominios (A.2 a A.10) y los 38 controles con la distribución A.2:3, A.3:2, A.4:5, A.5:4, A.6:9, A.7:5, A.8:4, A.9:3, A.10:3, con objetivo de dominio, título, propósito parafraseado, evidencia esperada y la orientación del Anexo B como `implementation_guidance`. Cubre PR-CAP-050. Fase MVP. Realiza UJ-1.
- La prueba de dominio de la distribución es bloqueante del pipeline.

#### FR-060: Decidir la aplicabilidad de cada control en una SoA de primera clase
Un `Compliance Manager` o `AI Governance Manager` puede, por control (`SoAItem`), registrar aplicabilidad (`Applicable`, `Not Applicable`, `Applicable with Alternative Control`, `Pending Assessment`), seleccionado, justificación, riesgos, sistemas de IA, requisitos, estado de implementación, propietario, requisitos de evidencia, estado de evidencia, excepciones, riesgo residual, aprobación, fecha de vigencia y fecha de revisión. Cubre PR-CAP-051, PR-CAP-052. Fase MVP. Realiza UJ-1.
- Los 38 controles aparecen siempre en la SoA, con la decisión vigente; ninguno puede eliminarse de la vista.
- Todo cambio de aplicabilidad emite `CONTROL_APPLICABILITY_CHANGED` con valor anterior, nuevo, actor y motivo.

#### FR-061: Exigir justificación para excluir y no generar flujos para lo excluido
El sistema rechaza la transición a `Not Applicable` sin justificación documentada; un control `Not Applicable` no genera tareas, no aparece como requisito activo en dashboards, no solicita evidencia recurrente y no entra en denominadores de métricas de controles activos, pero permanece visible con su justificación y se incluye en los informes de completitud. Cubre PR-DR-001, PR-DR-003. Fase MVP. Realiza UJ-1.
- Prueba unitaria: marcar `Not Applicable` un control con tareas abiertas cancela las tareas (con motivo) y no crea nuevas.
- La orientación del Anexo B nunca aparece como elemento a justificar.

#### FR-062: Exigir completitud antes de aprobar la SoA
El sistema bloquea la aprobación de la SoA mientras algún control `Applicable` o `Applicable with Alternative Control` carezca de propietario, estado de implementación, expectativas de evidencia, al menos un riesgo mapeado o al menos un requisito mapeado, y mientras exista algún control en `Pending Assessment`. Cubre PR-DR-004. Fase MVP. Realiza UJ-1.
- El mensaje de bloqueo enumera cada control y cada campo faltante.

#### FR-063: Aprobar y versionar la SoA
Un usuario con `soa.approve` puede aprobar la SoA creando una versión inmutable y comparar cualquier par de versiones (controles cambiados, justificaciones, propietarios). Cubre PR-CAP-054. Fase MVP. Realiza UJ-1.
- La aprobación emite `SOA_APPROVED`; la versión aprobada es la que usan la auditoría (criterios), los informes y la lista de preparación.

#### FR-064: Activar y desactivar controles por evento
Cuando un control pasa a `Applicable` (o la SoA se aprueba con controles aplicables), el sistema emite `CONTROL_ACTIVATED`, que crea las tareas de implementación, las solicitudes de evidencia con vencimiento, el seguimiento y las métricas del control; el paso a `Not Applicable` o `Retired` emite `CONTROL_DEACTIVATED`, que cancela tareas abiertas y solicitudes pendientes y retira el control de los denominadores. Cubre PR-CAP-053, PR-DR-002, PR-DR-001. Fase MVP. Realiza UJ-1.
- Las tareas y solicitudes creadas referencian el evento origen; su cancelación conserva el registro con motivo "control desactivado".
- Consumidores mínimos del evento en MVP: Evidence, Workflow, AIMS, Objectives & KPIs, Audit.

#### FR-065: Operar cada control activo desde su espacio de trabajo
Un `Control Owner` puede ver y operar su `ControlImplementation`: objetivo y razón de ser, requisitos y riesgos mapeados, sistemas de IA, propietario, plan de implementación, tareas, evidencia, pruebas (F2), métricas, excepciones (F2), hallazgos, acciones correctivas, historial de auditoría, eficacia y próxima revisión; y hacer avanzar el ciclo `Not Applicable`, `Applicable`, `Planned`, `Implementing`, `Implemented`, `Operating`, `Needs Improvement`, `Ineffective`, `Retired`. Cubre PR-CAP-055, PR-CAP-056. Fase MVP* (pruebas y excepciones F2). Realiza UJ-3.
- `Implemented` exige al menos una evidencia `Accepted`; `Operating` exige evidencia vigente (no expirada) `[ASSUMPTION]`.
- `EVIDENCE_EXPIRED` sobre la única evidencia vigente devuelve el control a `Needs Improvement` y su requisito a `Evidence Pending`.

#### FR-066: Incorporar controles adicionales o alternativos de la organización
Un `Compliance Manager` puede definir `CustomControl` fuera del Anexo A (o como alternativa a uno del Anexo A) e incorporarlos a la SoA con la misma trazabilidad, aplicabilidad y activación. Cubre PR-CAP-058. Fase MVP (Should).
- Un `CustomControl` se identifica visiblemente como "control de la organización", nunca con identificador A.n.n.

#### FR-067: Emitir el informe de completitud de la SoA
Un usuario con `soa.read` puede generar el *Statement of Applicability Report* con los 38 controles (y los adicionales), aplicabilidad, justificación, propietario, estado de implementación y evidencia, incluidos los `Not Applicable` con su justificación. Cubre PR-CAP-060, PR-CAP-111 (#3). Fase MVP. Realiza UJ-1.
- El informe indica la versión de la SoA y la versión de la norma; exportable a PDF, CSV y JSON en ES y EN.

#### FR-068: Probar la eficacia de los controles
Un `Control Owner` o `Auditor` puede registrar `ControlTest` (eficacia de diseño, implementación, eficacia operativa) con procedimiento, probador, fecha, muestra, evidencia, resultado, hallazgos y acciones correctivas. Cubre PR-CAP-057. Fase F2.
- Un resultado fallido emite `CONTROL_TEST_FAILED`, recalcula el residual (FR-044) y crea un `Finding` (`source_type = control_test`).

#### FR-069: Sugerir el mapeo de controles con IA
El motor de asistencia puede proponer controles del Anexo A para un riesgo o sistema, como sugerencia marcada. Cubre PR-CAP-059. Fase F2.

#### FR-070: Gestionar excepciones formales con expiración obligatoria
Un `Compliance Manager` puede registrar una `Exception` sobre requisito o control con motivo, justificación de negocio, riesgo, control compensatorio, propietario, aprobador, expiración y fecha de revisión; ninguna excepción es permanente. Cubre PR-CAP-135, PR-DR-014. Fase F2.
- Aprobar emite `EXCEPTION_APPROVED`; 30 días antes del vencimiento, `EXCEPTION_EXPIRING`; vencida, la excepción deja de justificar puertas ni estados.

### 6.9 Evidencia

**Descripción.** La evidencia es el centro de la preparación para la auditoría. Tiene ciclo de vida completo, hash, versión, procedencia y revisión formal. La regla dura: evidencia subida no es evidencia aceptada; solo cuenta la `Accepted`. Nunca se borra: se supersede o archiva. Realiza UJ-3. *Bounded context:* Evidence.

#### FR-071: Mantener una biblioteca de evidencia con tipos y campos completos
Un `Evidence Owner` puede registrar evidencia de 15 tipos (`document`, `screenshot`, `api_result`, `system_record`, `configuration`, `log`, `report`, `meeting_record`, `approval`, `training_record`, `assessment`, `audit_result`, `monitoring_metric`, `source_code_result`, `automated_scan`) con los 22 campos de P2 §15: identificador, título, descripción, origen, método de recogida, fecha y autor de recogida, hash, versión, propietario, clasificación, retención, expiración, requisitos, controles, riesgos, sistemas de IA y auditoría relacionados, estado de revisión, revisor y fecha de revisión. Cubre PR-CAP-061. Fase MVP. Realiza UJ-3.
- Archivos almacenados en el almacenamiento de objetos con prefijo de tenant (FR-002); metadatos en base de datos.
- Tipos y tamaño máximo de archivo validados en MVP; escaneo antimalware en F2 (NFR-004).

#### FR-072: Recorrer el ciclo de vida de la evidencia
El sistema gestiona los estados `Requested`, `Missing`, `Collected`, `Under Review`, `Accepted`, `Rejected`, `Expired`, `Superseded` con transiciones controladas por permiso. Cubre PR-CAP-062. Fase MVP. Realiza UJ-3.
- Solo `Accepted` cuenta en *Evidence Coverage*, en el estado de controles y requisitos y en los criterios de auditoría.
- Cada transición queda en el registro de auditoría con actor y motivo; `Rejected` exige motivo.

#### FR-073: Garantizar la integridad con hash y versión
El sistema calcula y almacena un hash criptográfico (SHA-256 `[ASSUMPTION]`) y una versión para toda evidencia, manual o automatizada, y permite verificar la integridad de un archivo frente a su hash. Cubre PR-CAP-063. Fase MVP. Realiza UJ-3.
- Subir un archivo nuevo sobre una evidencia crea versión n+1 y marca la anterior `Superseded`; el hash de cada versión es visible en la UI y en el informe.

#### FR-074: Solicitar evidencia y revisarla formalmente con separación de funciones
El sistema crea `EvidenceRequest` con vencimiento (por activación de control, por tarea o manualmente); un `Reviewer` con `evidence.approve`, distinto de quien subió la evidencia, la acepta o rechaza con motivo mediante `EvidenceReview`. Cubre PR-CAP-065, PR-DR-011. Fase MVP. Realiza UJ-3.
- La aceptación emite `EVIDENCE_APPROVED`; el rechazo, `EVIDENCE_REJECTED` con motivo obligatorio; ambos notifican al propietario.
- Si el único revisor disponible es quien subió la evidencia, la plataforma lo señala y exige un segundo usuario con permiso `[ASSUMPTION heredada de PR-DR-011]`.
- Una solicitud vencida sin evidencia pasa a `Missing` y emite `EVIDENCE_EXPIRING`/tarea vencida.

#### FR-075: Enlazar la evidencia a los objetos que soporta
Un `Evidence Owner` puede crear `EvidenceLink` a requisitos, controles, riesgos, sistemas de IA, auditorías y evaluaciones de impacto; una evidencia puede soportar varios objetos. Cubre PR-CAP-066. Fase MVP. Realiza UJ-3.
- El panel de trazabilidad de cada objeto lista su evidencia con estado y hash (FR-125).

#### FR-076: Registrar la procedencia de la evidencia automatizada
Para evidencia recogida por conector, el sistema registra `EvidenceProvenance`: conector, endpoint, hora de recogida, metadatos de la petición, identificador del objeto fuente, transformación aplicada, versión del colector y hash. Cubre PR-CAP-064. Fase F2 (el modelo existe en MVP para el adaptador mock).
- Una evidencia automatizada sin procedencia completa no puede pasar a `Accepted`.

#### FR-077: Detectar evidencia obsoleta o por expirar
Un job programado detecta evidencia próxima a expirar (30 días `[ASSUMPTION]`) y expirada, emite `EVIDENCE_EXPIRING` y `EVIDENCE_EXPIRED`, notifica y actualiza el estado de controles y requisitos. Cubre PR-CAP-069. Fase MVP. Realiza UJ-3.
- El informe *Evidence Freshness* (F2) y el dashboard consumen estos eventos.

#### FR-078: Analizar evidencia con IA
El motor de asistencia puede indicar qué requisitos y controles soporta una evidencia, brechas, inconsistencias e información obsoleta, solo como sugerencia. Cubre PR-CAP-067. Fase F2.

### 6.10 Documentos y políticas

**Descripción.** Información documentada viva (7.5): políticas, procedimientos y documentos con versiones, aprobación, distribución y exportación; generación asistida en Fase 2. *Bounded context:* Documents & Policies.

#### FR-079: Gestionar la información documentada con un ciclo canónico
Un `Compliance Manager` puede crear `Document`/`DocumentVersion`, `Policy`/`PolicyVersion` y `Procedure` con el ciclo canónico único de FR-029 (los 6 estados de P2 §20 son subconjunto de los 8 de §8 `[decisión C4]`), control de versiones, aprobación y distribución. Cubre PR-CAP-070. Fase MVP.
- Publicar emite `DOCUMENT_PUBLISHED`; toda versión aprobada es inmutable y descargable con su hash.
- Los 21 elementos de información documentada obligatoria (PR-DR-027) pueden asociarse a un documento o registro identificable.

#### FR-080: Exportar documentos e informes
Un usuario puede exportar documentos, actas e informes a PDF y HTML/Markdown (MVP) y DOCX/XLSX (F2) mediante el servicio compartido de renderizado, en el idioma de su preferencia. Cubre PR-CAP-072, PR-XC-011. Fase MVP* .
- Toda exportación incluye pie con tenant, fecha, versión y, cuando aplique, hash del documento.

#### FR-081: Generar borradores de documentos con plantillas y datos reales
Un `Compliance Manager` con `policies.generate` puede generar borradores desde 15 plantillas (política de IA, gestión de riesgos, desarrollo, IA responsable, gobernanza de datos, seguridad, incidentes, ciclo de vida, supervisión humana, proveedores, evaluación de impacto, seguimiento, auditoría, evidencia, excepciones) alimentadas con contexto, alcance, objetivos, controles aplicables, postura de riesgo y principios de la organización; el borrador nace en `AI Generated` y sigue el ciclo de aprobación humana. Cubre PR-CAP-071, PR-CAP-016. Fase F2.
- Cada borrador lleva los metadatos de FR-128; nunca se publica sin aprobación.

### 6.11 Tareas, aprobaciones y notificaciones

**Descripción.** El motor que convierte requisitos en trabajo y decisiones en huella. Realiza UJ-3, UJ-7. *Bounded contexts:* Workflow, Notifications.

#### FR-082: Convertir el cumplimiento en tareas
El sistema crea y gestiona `Task` originadas desde requisito, control, riesgo, evaluación de impacto, hallazgo, incidente, acción correctiva, revisión de política, solicitud de evidencia, puerta de ciclo de vida, revisión por la dirección o descubrimiento, con título, descripción, origen, propietario, revisor, prioridad, vencimiento, estado, dependencias, evidencia requerida, aprobación requerida, escalado, SLA y recurrencia. Cubre PR-CAP-075. Fase MVP.
- Toda tarea referencia su objeto origen y el evento que la creó; una tarea vencida emite `TASK_OVERDUE`.
- Las tareas recurrentes (p. ej., evidencia trimestral) se generan por job (FR-012).

#### FR-083: Aprobar con flujos genéricos y registro de decisión
El sistema ofrece `Workflow`, `Approval` y `Comment` reutilizables por todos los contextos, con aprobaciones multinivel, delegación y registro de la decisión (aprobador, fecha, motivo). Cubre PR-XC-008. Fase MVP* (multinivel y delegación completas en F2).
- Toda aprobación de cumplimiento (alcance, política, plan de tratamiento, aceptación, SoA, evidencia, cierre de CAPA, revisión por la dirección) usa este mecanismo y emite `APPROVAL_REQUESTED`/`APPROVAL_GRANTED`/`APPROVAL_REJECTED`.

#### FR-084: Notificar con reglas configurables y plantillas bilingües
El sistema envía notificaciones in-app y por correo (webhooks preparados) según reglas configurables para los 13 disparadores de P2 §30 (tareas vencidas, evidencia por expirar, riesgos altos, expiración de aceptaciones, puertas fallidas, incidentes, sincronizaciones fallidas, hallazgos, auditorías próximas, revisión por la dirección, expiración de políticas, revisión de controles, cambios regulatorios), en el idioma del destinatario. Cubre PR-XC-009, PR-XC-012, PR-EXT-006. Fase MVP* (in-app y correo básicos en MVP). Realiza UJ-3, UJ-7.
- Cada envío queda en `NotificationDelivery` con estado; los fallos emiten `NOTIFICATION_FAILED` y se reintentan.

### 6.12 Incidentes de IA

**Descripción.** Incidentes como disparadores del sistema (A.8.4, A.6.2.6, 10.2). *Bounded context:* Incidents. Fase 2.

#### FR-085: Gestionar incidentes de IA
Un usuario con permiso puede registrar incidentes con 13 categorías (comportamiento inesperado, salida incorrecta, sesgo, privacidad, seguridad, alucinación, calidad de datos, deriva, uso no autorizado, contenido inseguro, regulatorio, proveedor, fallo de supervisión humana), cronología, comunicación y ciclo `Reported, Triaged, Investigating, Contained, Remediating, Resolved, Closed, Lessons Learned`. Cubre PR-CAP-080. Fase F2.
- Crear emite `INCIDENT_CREATED`; cerrar exige lecciones aprendidas registradas `[ASSUMPTION]`.

#### FR-086: Propagar los incidentes significativos
Un incidente marcado significativo dispara reevaluación de riesgo e impacto, revisión de controles, acción correctiva y notificación a la dirección. Cubre PR-CAP-081. Fase F2.

### 6.13 Auditoría interna alineada con ISO 19011:2026

**Descripción.** El "Audit" del ciclo (9.2). Programa basado en riesgos, auditorías con ciclo completo, evidencia por criterio, hallazgos configurables, conclusión derivada e informe; independencia del auditor. Realiza UJ-4. *Bounded context:* Audit.

#### FR-087: Planificar el programa de auditoría
Un `Auditor` o `Compliance Manager` puede crear el `AuditProgramme` con planificación anual basada en riesgos, alcance, objetivos, criterios, recursos, competencia e independencia de auditores, métodos (incluidas auditorías remotas y combinadas). Cubre PR-CAP-085. Fase MVP* (programa anual simple en MVP; planificación basada en riesgos automática con `RISK_ESCALATED` en F2). Realiza UJ-4.
- Programar una auditoría emite `AUDIT_SCHEDULED` y notifica a los auditados 15 días antes `[ASSUMPTION]`.

#### FR-088: Ejecutar auditorías con plan y ciclo completo
Un `Auditor` con `audits.execute` puede crear el `AuditPlan` y recorrer `Planned, Scheduled, Preparing, Opening, Fieldwork, Evidence Collection, Findings, Closing, Report, Follow-up, Closed`, con criterios (`AuditCriterion`) cargados desde los requisitos del catálogo y la versión aprobada de la SoA. Cubre PR-CAP-086. Fase MVP. Realiza UJ-4.
- Iniciar emite `AUDIT_STARTED`; concluir, `AUDIT_CONCLUDED`.
- Un auditor no puede auditar controles de los que es propietario (independencia) `[ASSUMPTION heredada de ISO 19011 y del rol Auditor]`.

#### FR-089: Vincular evidencia de auditoría por criterio
Un `Auditor` puede vincular evidencia de la biblioteca (solo `Accepted`) y evidencia recogida durante la auditoría a cada criterio, requisito, control, sistema de IA y hallazgo mediante `AuditEvidence`. Cubre PR-CAP-087. Fase MVP. Realiza UJ-4.
- `AuditEvidence` es un vínculo con el criterio evaluado; no duplica el archivo (conflicto C6).

#### FR-090: Registrar hallazgos con clasificación configurable
Un `Auditor` puede registrar `AuditFinding` con clasificación configurable por organización, por defecto `Conformity`, `Nonconformity`, `Observation`, `Opportunity for Improvement`, y graduación opcional (mayor/menor o numérica 1-5), enlazando criterio, evidencia, requisito y control. Cubre PR-CAP-088, PR-DR-022. Fase MVP. Realiza UJ-4, UJ-6.
- Cada hallazgo emite `AUDIT_FINDING_CREATED`; una `Nonconformity` crea automáticamente la no conformidad en CAPA (FR-094).

#### FR-091: Derivar la conclusión y emitir el informe de auditoría
Un `Auditor` puede emitir la `AuditConclusion` solo cuando cada criterio evaluado tiene al menos un hallazgo o una declaración de conformidad, y generar el `AuditReport` con la cadena `criterio → evidencia → hallazgo → conclusión → acción correctiva → seguimiento`. Cubre PR-CAP-089, PR-DR-023. Fase MVP. Realiza UJ-4.
- El informe es exportable (FR-080) en ES y EN e inmutable una vez emitido.

#### FR-092: Hacer seguimiento de la auditoría
Un `Auditor` puede verificar en `Follow-up` las correcciones y acciones correctivas del auditado antes de cerrar la auditoría. Cubre PR-CAP-091. Fase MVP (Should).
- `CAPA_CLOSED` de una no conformidad de la auditoría notifica al auditor (`AUDIT_FOLLOW_UP_DUE`).

#### FR-093: Dar acceso de solo lectura a auditores externos
Un `Organization Administrator` puede invitar a un auditor externo con rol `Read Only` limitado al paquete de evidencia, la SoA y los informes del tenant, con caducidad. Cubre PR-XC-004, PR-XC-005 `[ASSUMPTION S1]`. Fase MVP (Should).
- El auditor externo no ve otros tenants ni puede mutar; cada acceso queda en el registro de auditoría.

### 6.14 No conformidades, CAPA y mejora continua

**Descripción.** 10.1 y 10.2: hallazgo genérico con origen, no conformidad, corrección, causa raíz, acción correctiva y verificación de eficacia obligatoria antes del cierre. Realiza UJ-4 (origen) y UJ-5 (agregación). *Bounded context:* CAPA.

#### FR-094: Registrar hallazgos genéricos y no conformidades
El sistema mantiene `Finding` como entidad genérica con `source_type` ∈ {`audit`, `control_test`, `impact`, `incident`, `data_quality`, `ai_gap_analysis`} y `Nonconformity` con requisito, control, evidencia, hallazgo origen, corrección inmediata, causa raíz, acción correctiva, propietario, vencimiento, prueba de eficacia, verificación y cierre; `AuditFinding` e `ImpactAssessmentFinding` son especializaciones. Cubre PR-CAP-095 `[decisión provisional C7, §17 D-4]`. Fase MVP. Realiza UJ-4.
- Crear emite `FINDING_CREATED` y, si procede, `CAPA_CREATED`; una CAPA vencida emite `CAPA_OVERDUE`.

#### FR-095: Analizar la causa raíz
Un propietario de CAPA puede documentar la causa raíz con 5 porqués, Ishikawa o método personalizado (`RootCauseAnalysis`). Cubre PR-CAP-096. Fase MVP (Should).
- No se puede definir acción correctiva sin causa raíz registrada `[ASSUMPTION]`.

#### FR-096: Verificar la eficacia antes de cerrar
El sistema exige `EffectivenessVerification` (verificador, método, evidencia, resultado) antes de permitir el cierre de una CAPA; un cierre sin verificación es rechazado. Cubre PR-CAP-097, PR-DR-024. Fase MVP. Realiza UJ-4.
- Cerrar emite `CAPA_EFFECTIVENESS_VERIFIED` y `CAPA_CLOSED`; el verificador es distinto del propietario de la acción `[ASSUMPTION]`.

#### FR-097: Registrar oportunidades de mejora continua
Un usuario puede registrar `ImprovementOpportunity` con el flujo `Improvement Opportunity → Analysis → Action → Owner → Implementation → Effectiveness → Closure`, alimentado por hallazgos, incidentes, cambios de riesgo, retroalimentación, regulación, seguimiento, revisión por la dirección y lecciones aprendidas. Cubre PR-CAP-098. Fase F2.

### 6.15 Revisión por la dirección

**Descripción.** El "Improve" desde la alta dirección (9.3): entradas agregadas, decisiones y acciones trazadas, acta formal. Realiza UJ-5. *Bounded context:* Management Review.

#### FR-098: Ejecutar la revisión por la dirección con entradas y salidas obligatorias
Un `Executive` o `AI Governance Manager` puede programar una `ManagementReview` con fecha de corte, revisar las 14 entradas (estado de decisiones previas, objetivos, KPI, riesgos, impactos, auditorías, incidentes, no conformidades, CAPA, cambios regulatorios, cambios del portafolio, retroalimentación de partes interesadas, desempeño de proveedores, necesidades de recursos; cubre 9.3.2), registrar las 8 salidas (decisiones, acciones, recursos, objetivos, política, alcance, riesgos, mejora; 9.3.3) y aprobar el acta como información documentada. Cubre PR-CAP-100. Fase MVP. Realiza UJ-5.
- Programar emite `MANAGEMENT_REVIEW_SCHEDULED`; aprobar, `MANAGEMENT_REVIEW_APPROVED`, que convierte acciones en tareas y decisiones sobre política/alcance en solicitudes de revisión.
- Una entrada sin datos debe marcarse explícitamente "sin datos en el periodo" con motivo; el acta lo refleja.
- El acta es exportable a PDF (FR-080) e inmutable.

#### FR-099: Agregar automáticamente las entradas de la revisión
El sistema agrega a la fecha de corte las entradas desde Risk, Audit, CAPA, Incidents (F2), Objectives & KPIs y AI Portfolio (`ManagementReviewInput` con referencias a los registros fuente). Cubre PR-CAP-101. Fase MVP (Should). Realiza UJ-5.
- Cada cifra agregada enlaza a la lista de registros que la componen; no hay cifras sin fuente.

### 6.16 Objetivos, KPI, puntuación y dashboards

**Descripción.** El "Monitor" honesto (9.1): toda puntuación es transparente, el vocabulario es *readiness / coverage / implementation / effectiveness / exposure*, y nunca aparece "certificado" ni un porcentaje único de cumplimiento. Realiza UJ-1, UJ-5. *Bounded contexts:* Objectives & KPIs, Reporting.

#### FR-100: Calcular puntuaciones transparentes
El sistema calcula `Requirement Readiness`, `Evidence Coverage`, `Control Implementation`, `Control Effectiveness`, `Risk Exposure` y `Audit Readiness` como `ScoreSnapshot` que expone fórmula, numerador, denominador, exclusiones (p. ej., controles `Not Applicable`, sistemas fuera de alcance), registros fuente y marca temporal, en UI y API. Cubre PR-CAP-106, PR-DR-010, PR-DR-009. Fase MVP. Realiza UJ-1, UJ-5.
- Ninguna pantalla muestra un porcentaje de cumplimiento sin el desplegable de explicación; una prueba de UI lo verifica en el dashboard y los informes.
- Los términos "compliance score", "certificado" y "certified" no existen en los recursos de i18n ni en la API; una prueba de cadenas lo comprueba.
- Recalcular emite `SCORE_RECALCULATED`.

#### FR-101: Configurar KPI y métricas
Un `AI Governance Manager` puede definir `KPI`/`Metric` en las familias cumplimiento, riesgo, portafolio de IA, evidencia, auditoría y formación, con umbrales. Cubre PR-CAP-105. Fase MVP* (cumplimiento y riesgo en MVP).
- Superar un umbral emite `KPI_THRESHOLD_BREACHED`, que es entrada de la revisión por la dirección y (F2) posible incidente.

#### FR-102: Mostrar el dashboard ejecutivo
Un usuario con permiso de lectura puede ver un dashboard que responde de inmediato: preparación del SGIA, exposición al riesgo, eficacia de controles, cobertura de evidencia, portafolio, hallazgos abiertos, CAPA abiertas, incidentes (F2), preparación para auditoría y tareas vencidas, con navegación de detalle. Cubre PR-CAP-107. Fase MVP (Should). Realiza UJ-5.
- Cada widget es una puntuación transparente o un contador enlazado; los `Not Applicable` no aparecen como pendientes.

#### FR-103: Ofrecer dashboards por dominio
El sistema ofrece dashboards por dominio (ejecutivo, ISO 42001, riesgo, inventario, ciclo de vida, gobernanza de datos, IA responsable, seguridad, terceros, auditoría, evidencia, incidentes, CAPA, formación, integraciones) que solo muestran controles y flujos activos según la SoA. Cubre PR-CAP-108. Fase F2.

#### FR-104: Evaluar la madurez de la gobernanza de IA
Un `AI Governance Manager` puede ejecutar una `MaturityAssessment` con 12 dimensiones y niveles `Initial, Developing, Defined, Managed, Optimized`, presentada como métrica de gestión y nunca como puntuación de certificación. Cubre PR-CAP-145. Fase F3.

### 6.17 Informes

**Descripción.** Un motor declarativo (sin un *code path* por informe), filtrable y exportable; cuatro informes esenciales en MVP y el catálogo de 25 de P2 §27 por fases. Realiza UJ-1, UJ-4, UJ-5, UJ-6. *Bounded context:* Reporting.

#### FR-105: Generar informes desde definiciones declarativas
Un usuario con `reports.read` puede ejecutar `ReportDefinition` con filtros por rango de fechas, unidad de negocio, departamento, sistema de IA, proveedor, riesgo, dominio de control, propietario, ciclo de vida, estado y filtros personalizados, y exportar a PDF, CSV y JSON (MVP) y XLSX y HTML (F2). Cubre PR-CAP-110. Fase MVP* .
- Añadir un informe nuevo es añadir una definición, no código; una prueba de arquitectura lo verifica.
- La ejecución es asíncrona (FR-012) y queda en `ReportRun` con parámetros, autor, fecha y hash del resultado.

#### FR-106: Entregar los cuatro informes esenciales del MVP
El sistema entrega: *Statement of Applicability Report* (#3), *ISO/IEC 42001 Clause Compliance Report* (#2: cláusulas 4-10, estado, propietarios, evidencia, brechas), *AI Risk Register Report* (#5: inherente/residual, tratamiento, propietario, vencimientos) y *Audit Readiness Report* (#13: preparación por requisito, control y evidencia, con la lista de comprobación de FR-113). Cubre PR-CAP-111. Fase MVP. Realiza UJ-1, UJ-5, UJ-6.
- Los cuatro son bilingües, exportables y muestran versión de SoA, versión de norma y fecha de corte; ninguno usa "certificado".

#### FR-107: Entregar el resto del catálogo de informes
El sistema entrega con el mismo motor los informes #1, #4, #6-#12, #14-#18 y #23 de P2 §27 (dashboard ejecutivo, eficacia de controles, sistemas de alto riesgo, inventario, cumplimiento de ciclo de vida, evaluación de impacto, gobernanza de datos, terceros, incidentes, cobertura y frescura de evidencia, no conformidades y CAPA, revisión por la dirección, KPI, eficacia del tratamiento). Cubre PR-CAP-112. Fase F2.

#### FR-108: Emitir los paquetes de preparación para certificación y de evidencia de auditoría
Un `Compliance Manager` puede generar el *Certification Readiness Pack* (#24: resumen ejecutivo más índice detallado de requisitos, controles y evidencia) y el *Audit Evidence Pack* (#25: exportación estructurada con referencias de evidencia y trazabilidad, hashes incluidos). Cubre PR-CAP-115. Fase F2.

#### FR-109: Programar informes
Un usuario puede programar informes (diaria, semanal, mensual, trimestral, personalizada) con destinatarios (usuarios, equipos, roles) y entrega in-app, correo o paquete descargable. Cubre PR-CAP-114. Fase F2.

#### FR-110: Añadir resúmenes narrativos generados por IA
Los informes pueden incluir un resumen ejecutivo generado por el motor de asistencia, marcado como generado y con los metadatos de FR-128. Cubre PR-CAP-116. Fase F2.

#### FR-111: Construir informes dinámicos
Un usuario puede componer informes (fuente, dimensiones, métricas, filtros, visualización, guardar, programar, compartir, exportar) sobre una abstracción de consulta, sin SQL arbitrario. Cubre PR-CAP-113. Fase F3.

#### FR-112: Entregar los informes dependientes de conectores y regulación
El sistema entrega los informes #19 (conectores y descubrimiento), #20 (impacto regulatorio), #21 (supervisión humana) y #22 (cambios de modelo). Cubre PR-CAP-117. Fase F3.

### 6.18 Preparación para certificación

**Descripción.** Un espacio que responde "¿qué falta para presentarnos?" sin declarar jamás "certificado". Realiza UJ-5, UJ-6. *Bounded context:* Reporting (vista agregada sobre AIMS, Controls & SoA, Evidence, Risk, Audit, CAPA, Management Review).

#### FR-113: Mostrar el espacio de preparación para certificación
Un usuario con permiso de lectura puede ver el cálculo de requisitos, controles, evidencia, riesgos, auditorías, hallazgos, CAPA y revisión por la dirección, la lista de comprobación de 13 preguntas de P2 §53 (contexto listo, alcance aprobado, política aprobada, metodología de riesgo aprobada, evaluaciones de riesgo completas, evaluaciones de impacto completas, SoA aprobada, controles implementados, evidencia suficiente, seguimiento activo, auditoría interna completada, revisión por la dirección completada, CAPA cerradas), los 21 elementos de información documentada obligatoria con su registro, y el estado `Ready for internal review`, `Audit-ready` o `Certification preparation status`. Cubre PR-CAP-120, PR-DR-009, PR-DR-027. Fase MVP (Should). Realiza UJ-5.
- Cada casilla de la lista enlaza al registro que la satisface o a la acción que falta; el estado global se explica con sus condiciones.
- Los criterios exactos de cada estado se fijan en arquitectura `[ASSUMPTION]`; en ningún caso incluyen la palabra "certificado".

### 6.19 Integraciones y conectores

**Descripción.** Un SDK de conectores desacoplado del dominio, con credenciales en gestor de secretos y estados honestos (`mock`, `configured`, `validated`). El MVP entrega SDK + adaptador mock etiquetado; los conectores reales llegan en Fase 2 y solo se declaran validados contra la API real con credenciales. *Bounded context:* Integrations.

#### FR-114: Ofrecer un SDK de conectores extensible
El sistema ofrece un marco con `ConnectorDefinition`, `Integration`, `IntegrationCredential` (referencia a secreto), prueba de conexión, `IntegrationSync` con programación, `DataAvailability`, mapeos de datos, evidencia, riesgo y hallazgos, `SyncError` y logs, sin acoplar la lógica de negocio a ningún proveedor. Cubre PR-CAP-125. Fase MVP.
- Añadir un conector no modifica el modelo de dominio; una prueba de arquitectura lo verifica.
- Una sincronización emite `CONNECTOR_SYNC_COMPLETED` o `CONNECTOR_SYNC_FAILED`.

#### FR-115: Entregar un adaptador mock etiquetado con pruebas de contrato
El sistema incluye un conector `mock` que ejercita el SDK completo (descubrimiento de activos ficticios, evidencia automatizada con procedencia), visible con etiqueta "mock" en toda la UI, y pruebas de contrato del SDK; el estado de cada conector es `mock`, `configured` o `validated`. Cubre PR-CAP-131, PR-DR-021, PR-NFR-016. Fase MVP.
- Ningún conector aparece como `validated` sin una prueba ejecutada contra la API real del proveedor con credenciales reales (aprobación humana previa).
- Los activos y la evidencia del mock llevan la etiqueta de demo (PR-DR-020) y nunca cuentan como cumplimiento real.

#### FR-116: Gestionar credenciales de integración con un gestor de secretos
Un usuario con `integrations.manage` puede crear, rotar (F2) y revocar credenciales que se almacenan solo en el gestor de secretos, con cifrado en reposo, scopes mínimos, prueba de conexión y aislamiento por tenant; el sistema emite `CREDENTIAL_CREATED`, `CREDENTIAL_UPDATED`, `CREDENTIAL_ROTATED`, `CREDENTIAL_ACCESSED`, `CREDENTIAL_REVOKED`. Cubre PR-XC-015, PR-EXT-008. Fase MVP* (rotación F2).
- Ninguna columna de base de datos ni respuesta de API contiene un secreto; una prueba de exposición de secretos lo verifica.

#### FR-117: Conectar Microsoft Azure / Azure AI Foundry
El conector recupera, donde la API y los permisos lo permitan, proyectos, modelos, despliegues, versiones, endpoints, recursos, configuraciones, metadatos de seguimiento, evaluaciones, configuración de *safety*, artefactos de IA responsable, uso, logs, propietarios/etiquetas y relaciones. Cubre PR-CAP-126, PR-EXT-001. Fase F2.
- No se asume la disponibilidad de ningún dato hasta verificarlo; los no disponibles se muestran con FR-120.

#### FR-118: Conectar OpenAI
El conector recupera información de proveedor, cuenta, modelos, despliegues, uso y configuración expuesta por la API autorizada. Cubre PR-CAP-127, PR-EXT-002. Fase F2.

#### FR-119: Conectar Anthropic
El conector recupera información de organización, API y modelos donde el proveedor la exponga. Cubre PR-CAP-128, PR-EXT-003. Fase F2.

#### FR-120: Mostrar la disponibilidad de cada dato por conector
La UI muestra por dato y conector `Available`, `Not Available`, `Permission Required`, `Not Supported by Provider API`, `Not Configured`. Cubre PR-CAP-129. Fase F2.

#### FR-121: Clasificar los errores de conector con guía de remediación
El sistema clasifica los errores en `Authentication Failed`, `Authorization Failed`, `Rate Limited`, `Provider Unavailable`, `Invalid Configuration`, `Unsupported Endpoint`, `Partial Sync`, `Data Mapping Error`, `Timeout`, `Unknown Error`, cada uno con guía de remediación. Cubre PR-CAP-130. Fase F2.

#### FR-122: Añadir conectores futuros sin cambiar el dominio
El SDK admite conectores para AWS AI, Vertex AI, GitHub, Azure DevOps, Jira, ServiceNow, Datadog, Splunk, Purview, Entra ID, SharePoint, almacenamiento en nube, SIEM, ticketing, registros de modelos y plataformas ML. Cubre PR-CAP-132, PR-EXT-014. Fase F3.

### 6.20 Búsqueda, trazabilidad y explicación contextual

**Descripción.** Encontrar cualquier cosa, recorrer la cadena en ambos sentidos y entender por qué se exige. Realiza UJ-2. *Bounded contexts:* Administration (índice), transversal.

#### FR-123: Buscar en todo el tenant con explicación de la coincidencia
Un usuario puede buscar léxicamente sistemas de IA, activos, requisitos, controles, riesgos, evidencia, políticas, auditorías, hallazgos, incidentes e informes, respetando permisos, y ver por qué coincidió cada resultado. Cubre PR-CAP-160, PR-XC-010. Fase MVP (Should).
- El índice se alimenta por proyección de eventos; un objeto sin permiso de lectura no aparece ni siquiera como conteo.

#### FR-124: Buscar semánticamente
El sistema ofrece búsqueda por embeddings sobre los mismos tipos. Cubre PR-CAP-161. Fase F3.

#### FR-125: Navegar la trazabilidad en ambos sentidos
Desde cualquier nodo (`Requirement → Control → Risk → AI System → Asset/Data/Model → Evidence → Audit → Finding → Corrective Action → Management Review`) un usuario puede recorrer la cadena hacia adelante y hacia atrás; en MVP mediante enlaces y paneles de relación, en F2 con visualización gráfica. Cubre PR-CAP-165. Fase MVP* . Realiza UJ-2.
- Cada requisito y control sembrado muestra su documento fuente (`resumenes/NN §sección`) como primer nodo (A10).
- Métrica SM-5: un auditor llega de un requisito a su evidencia aceptada en menos de 2 minutos en la demo.

#### FR-126: Explicar "¿por qué es requerido?" en cada objeto de cumplimiento
Cada requisito, control, tarea y solicitud de evidencia muestra por qué existe el requisito, por qué se activó el control, qué riesgo lo seleccionó, qué sistemas afecta, qué evidencia lo demuestra, qué pasa si expira y qué criterio de auditoría lo probará; en MVP con textos de la semilla y datos del tenant, en F2 con generación asistida marcada. Cubre PR-CAP-166. Fase MVP* . Realiza UJ-2, UJ-3.
- El texto de la semilla cita su fuente; nunca contiene texto literal de la norma.

### 6.21 Gobernanza asistida por IA y autogobernanza de la plataforma

**Descripción.** La IA del producto es asistente, no decisora: interpreta, sugiere, redacta borradores, analiza y resume; una persona autorizada aprueba; cada artefacto lleva su ficha. Y la plataforma es a su vez un sistema de IA: sus funciones LLM se inventarían en el tenant plataforma y cumplen A.6, A.8 y A.9 (ajuste A9). Nada de esta sección existe en MVP, salvo el modelo de datos. *Bounded context:* AI Assistance.

#### FR-127: Ofrecer un motor de asistencia por IA
Un usuario con permiso puede invocar explícitamente (nunca por evento automático) interpretación de requisitos, análisis de brechas, sugerencia de riesgos, asistencia en evaluación de impacto, mapeo de controles, generación de políticas, análisis de evidencia, preparación de auditoría, sugerencia de acciones correctivas y resúmenes ejecutivos. Cubre PR-CAP-170. Fase F2.
- Cada invocación crea una `AIAssistanceTask` con usuario, contexto y coste; ninguna invocación se dispara sola.

#### FR-128: Marcar y gobernar todo artefacto generado por IA
Todo `AIGeneratedArtifact` lleva `generated_by_ai`, modelo, hora de generación, identificador de prompt/tarea, datos fuente, estado de revisión humana, revisor y aprobación; nunca cambia por sí mismo un estado de cumplimiento a implementado o conforme. Cubre PR-CAP-171, PR-DR-008. Fase F2.
- Emite `AI_ARTIFACT_GENERATED` y `AI_ARTIFACT_REVIEWED`; una prueba verifica que ningún servicio de asistencia tiene permiso de escritura sobre estados de requisito, control, riesgo, evidencia o CAPA.
- La IA nunca inventa identificadores de cláusula o control: toda referencia normativa en un artefacto se valida contra el catálogo antes de mostrarse.

#### FR-129: Abstraer el proveedor de LLM y controlar su coste
El sistema usa una abstracción de proveedor (Azure OpenAI, OpenAI, Anthropic intercambiables) con `LLMProviderConfig` por tenant, `LLMBudget`, caché de respuestas y `LLMUsageRecord` con tokens por llamada; al superar el presupuesto aplica bloqueo suave. Cubre PR-CAP-172, PR-NFR-019, PR-EXT-004. Fase F2.
- Superar el presupuesto emite `LLM_BUDGET_EXCEEDED` y notifica; el dominio no referencia ningún SDK de proveedor.

#### FR-130: Inventariar las funciones LLM de la plataforma como sistemas de IA
Las funciones asistidas por LLM se registran como sistemas de IA en el inventario del tenant plataforma, con evaluación de impacto, información a usuarios (A.8.2) y uso responsable (A.9) documentados, y sujetas a los controles A.6, A.8 y A.9. Cubre PR-CAP-027, PR-DR-026. Fase F2.
- `AI_ARTIFACT_GENERATED` alimenta el registro de uso del sistema de IA correspondiente en AI Portfolio.
- La matriz de trazabilidad del propio desarrollo se expone como evidencia de A.6.2.3 y A.6.2.7 del tenant plataforma (`TRACEABILITY_MODEL.md` §7).

### 6.22 Navegación y experiencia de uso

**Descripción.** UI empresarial: limpia, densa pero legible, responsive, accesible, navegable por teclado, consistente y amigable para el auditor. Escritorio primero. Bilingüe ES/EN con terminología UNE desde el primer commit. Realiza todas las UJ. Los NFR de accesibilidad y calidad UX están en §7.4.

#### FR-131: Navegar según la estructura de P2 §37 sin botones falsos
La navegación principal es: Dashboard; AI Portfolio (AI Systems, Models, Datasets, Providers, Lifecycle); ISO 42001 (Requirements, Statement of Applicability, Controls, Policies, Objectives); Risk (Risk Register, Assessments, Treatment, Risk Acceptance); Impact (Assessments, Findings, Mitigations); Evidence (Evidence Library, Requests, Reviews); Audit (Audit Programme, Audits, Findings, CAPA); Operations (Incidents, Tasks, Changes, Exceptions); Reports (Dashboards, Reports, Report Builder); Integrations (Providers, Connections, Discovery, Sync History); Administration (Organization, Users, Roles, Teams, Notifications, Settings). Las entradas de módulos no implementados en la fase vigente no se muestran. Cubre PR-CAP-175, PR-NFR-017. Fase MVP* .
- En MVP no aparecen: Lifecycle, Incidents, Changes, Exceptions, Report Builder, Discovery ni Sync History (salvo el mock etiquetado); una prueba E2E recorre cada entrada visible y verifica que tiene backend.
- Un usuario solo ve las entradas para las que tiene al menos permiso de lectura.

#### FR-132: Operar la UI en español o inglés con terminología oficial
Un usuario puede elegir idioma (ES/EN) por perfil; toda cadena de UI, correo, informe y explicación existe en ambos idiomas; las enumeraciones canónicas se almacenan en inglés y se traducen en la capa i18n conforme a `resumenes/13` y a las trampas de traducción de `GLOSSARY.md` §7. Cubre PR-XC-012. Fase MVP. Realiza UJ-7, UJ-6.
- Una prueba de cobertura falla si existe una clave sin traducción en cualquiera de los dos idiomas.
- Términos prohibidos en ES: "monitoreo", "stakeholder", "alta gerencia", "declaración de aplicación"; una prueba de cadenas lo verifica.

## 7. Requisitos no funcionales

*Convenciones.* Cada NFR indica `PR-*` cubiertos, fase y umbral verificable. Donde P2 o los ajustes no fijan umbral, el PRD propone uno con `[ASSUMPTION]` o `[ESTIMACIÓN]`, revisable en arquitectura. Los NFR se aplican a todo el producto; los específicos de una capacidad están inline en §6.

### 7.1 Seguridad

#### NFR-001: Seguridad de la aplicación por defecto
Cifrado en tránsito (TLS 1.2+) y en reposo, cabeceras seguras, protección CSRF, rate limiting, validación de toda entrada con esquemas Zod, codificación de salida, URLs de descarga firmadas y de corta vida, mínimo privilegio y diseño seguro de API. Cubre PR-NFR-001. Fase MVP.
- Rate limiting por usuario y por tenant con límites configurables; valor inicial 100 peticiones/min por usuario `[ASSUMPTION]`.
- URL firmada válida ≤ 15 min `[ASSUMPTION]` y ligada al `tenant_id` del objeto.
- Un escáner de cabeceras (p. ej., OWASP ZAP baseline) sin hallazgos altos en CI.

#### NFR-002: Ningún secreto ni dato ajeno llega al cliente
El cliente (navegador o consumidor de API) nunca recibe claves de API, secretos de proveedor, credenciales internas, evidencia sin permiso ni datos de otro tenant. Cubre PR-NFR-004. Fase MVP.
- Pruebas de exposición de secretos y de filtración cross-tenant en cada cambio; los secretos viven solo en el gestor (FR-116).

#### NFR-003: Cadena de suministro segura
Escaneo de dependencias, SAST y detección de secretos en cada cambio de CI; ninguna dependencia con licencia copyleft o de tercero no auditado sin aprobación humana (A12). Cubre PR-NFR-003, PR-EXT-011. Fase MVP.
- El pipeline falla ante vulnerabilidad alta o crítica sin excepción documentada.

#### NFR-004: Archivos subidos seguros
Validación de tipo y tamaño (≤ 100 MB por archivo `[ASSUMPTION]`) en MVP; escaneo antimalware y procesamiento de documentos aislado en F2. Cubre PR-NFR-002, PR-EXT-010. Fase MVP* .
- Un archivo rechazado por tipo, tamaño o malware queda registrado con motivo y nunca se almacena como evidencia.

#### NFR-005: Pruebas de seguridad en cada cambio
Toda entrega incluye pruebas automáticas de acceso cross-tenant, escalada de privilegios, acceso no autorizado a evidencia, exposición de secretos y autorización rota a nivel de objeto. Cubre PR-NFR-014 (bloque de seguridad). Fase MVP.
- Son bloqueantes del pipeline; el criterio de salida del MVP las exige en verde (§16.2).

### 7.2 Escalabilidad y rendimiento

#### NFR-006: Escalabilidad de diseño
El diseño soporta por tenant grande miles de sistemas de IA, decenas de miles de riesgos, cientos de miles de evidencias, historiales de auditoría grandes, muchos usuarios concurrentes y sincronizaciones programadas; verificación con carga en F2. Cubre PR-NFR-005. Fase MVP (diseño) / F2 (verificación).
- Prueba de carga F2: 200 usuarios concurrentes y 100 000 evidencias en un tenant sin degradar NFR-007 `[ESTIMACIÓN]`.

#### NFR-007: Rendimiento percibido
Paginación y filtrado en servidor, caché solo justificada, generación asíncrona de informes e índices en `tenant_id`, `status`, `owner_id`, `requirement_id`, `control_id`, `risk_id`, `ai_system_id`, `created_at`, `due_date`. Cubre PR-NFR-006. Fase MVP.
- p95 < 500 ms en listas paginadas y < 300 ms en páginas de detalle con datos de la organización demo `[ESTIMACIÓN]`; un informe esencial de 10 000 filas se genera en ≤ 60 s `[ESTIMACIÓN]`.

### 7.3 Disponibilidad y recuperación

#### NFR-008: Disponibilidad, copias y recuperación ante desastres
Objetivos de diseño (no compromisos contractuales): disponibilidad mensual ≥ 99,5 % `[ASSUMPTION]`, RPO ≤ 1 h y RTO ≤ 8 h `[ASSUMPTION heredada de PR-NFR-009]`, copias diarias cifradas dentro de la región del tenant, prueba de restauración semestral `[ASSUMPTION]`. Cubre PR-NFR-009. Fase MVP (Should). Pendiente de ratificación (§17 D-7) y de la pregunta Q6.

### 7.4 Accesibilidad y calidad de la experiencia

#### NFR-009: Accesibilidad WCAG 2.2 AA
Toda pantalla cumple WCAG 2.2 nivel AA, es navegable por teclado, responsive (escritorio primero; usable desde 768 px `[ASSUMPTION]`) y densa pero legible. Cubre PR-NFR-007. Fase MVP.
- Auditoría automática (axe o equivalente) sin errores críticos en todas las pantallas del flujo E2E; revisión manual de foco y lectores de pantalla en formularios de decisión (SoA, aceptación, revisión de evidencia).

#### NFR-010: Calidad UX verificada por módulo
Ningún módulo se declara completo sin estados vacíos, de carga y de error, paginación, accesibilidad y comportamiento responsive verificados. Cubre PR-NFR-018. Fase MVP.

#### NFR-011: Sin implementación solo mock
Toda acción importante recorre `UI → API → Domain Service → Database → Audit Log`; no existen botones ni entradas de menú sin implementación detrás. Cubre PR-NFR-017, PR-CAP-175. Fase MVP.
- Prueba E2E que recorre toda entrada de navegación visible y verifica respuesta de backend y entrada en el registro de auditoría para cada mutación.

#### NFR-012: Errores útiles y uniformes
Toda operación devuelve errores consistentes con mensaje comprensible, código, `correlation_id` y guía de remediación, en el idioma del usuario. Cubre PR-XC-020. Fase MVP.

### 7.5 Observabilidad

#### NFR-013: Observabilidad con OpenTelemetry
Logs estructurados, métricas, trazas, seguimiento de jobs, salud de conectores y aplicación, seguimiento de errores, exportables a un backend compatible. Cubre PR-NFR-008, PR-EXT-012. Fase MVP.
- Ningún log contiene secretos ni datos personales de evidencia; una prueba de redacción lo verifica `[ASSUMPTION]`.

#### NFR-014: Correlación de extremo a extremo
Toda operación síncrona y asíncrona lleva `correlation_id` propagado hasta el registro de auditoría y los jobs. Cubre PR-XC-019. Fase MVP.

### 7.6 Residencia y retención de datos

#### NFR-015: Residencia de datos por tenant
Cada tenant declara `data_region`; en MVP el campo existe y el despliegue es mono-región; en F2 la región determina la ubicación de base de datos y almacenamiento de objetos. Cubre PR-NFR-011. Fase MVP* . Regiones candidatas: UE, LatAm, EE. UU. `[ASSUMPTION, pregunta Q6]`.

#### NFR-016: Retención configurable sin borrado silencioso
Retención por clase de registro configurable; prohibido el borrado silencioso de registros de cumplimiento (ver FR-008). Cubre PR-NFR-012. Fase MVP.

### 7.7 Auditabilidad de la aplicación

#### NFR-017: Toda operación de gobernanza es atribuible y exportable
Un usuario autorizado puede responder para cualquier objeto "quién cambió esto, cuándo, por qué, cuál era el valor anterior y quién lo aprobó", y exportar la respuesta. Cubre PR-NFR-010, PR-XC-006. Fase MVP.
- Verificación de la cadena de hashes disponible en UI y API (FR-007); cualquier decisión de cumplimiento del flujo E2E aparece con actor, motivo y valor anterior.

### 7.8 Internacionalización

#### NFR-018: Bilingüe ES/EN desde la fundación
UI, correos, informes, explicaciones y documentación de usuario en español e inglés; enumeraciones canónicas en inglés traducidas en la capa i18n; terminología oficial de `resumenes/13`. Cubre PR-XC-012. Fase MVP.
- 100 % de cobertura de claves en ambos idiomas (FR-132); glosario de UI accesible desde cualquier término técnico.

### 7.9 Datos, mantenibilidad y documentación

#### NFR-019: Cambios de esquema solo por migración
Toda modificación de base de datos se hace por migración versionada, reversible cuando sea práctico, con compatibilidad de semilla, claves foráneas e índices; nunca modificación manual en producción; las migraciones irreversibles exigen aprobación humana. Cubre PR-NFR-013. Fase MVP.

#### NFR-020: Derechos de autor del texto normativo
La semilla y la base de datos contienen solo identificador, título, paráfrasis propia, tipo, evidencia esperada y referencias; nunca texto literal de ISO, IEC o UNE; cada registro cita archivo y sección de `resumenes/`; el material licenciado permanece fuera de Git. Cubre PR-NFR-020. Fase MVP.
- Revisión humana registrada de cada versión de semilla; una prueba busca coincidencias largas con los resúmenes para detectar copia literal `[ASSUMPTION sobre el mecanismo]`.

#### NFR-021: Extensibilidad sin reescritura
Una nueva revisión de norma no reescribe la aplicación; un nuevo conector no toca el dominio; un nuevo informe no crea un nuevo *code path*; una nueva jurisdicción no exige *hardcoding*. Cubre PR-NFR-021. Fase MVP.

#### NFR-022: Documentación viva en `docs/`
Arquitectura, seguridad, modelo de datos, API, integraciones, modelo de cumplimiento, informes, modelo de auditoría, riesgo, sistema de IA, despliegue, operaciones, pruebas y modelo de amenazas viven en el árbol único `docs/` (A3) y se actualizan con cada cambio archivado, junto con `docs/compliance/TRACEABILITY_MATRIX.md`. Cubre PR-NFR-022. Fase MVP (Should).

### 7.10 Pruebas

#### NFR-023: Pirámide de pruebas completa
Unitarias (puntuación, cálculo de riesgo, aplicabilidad, permisos, transiciones, cálculos de informes), integración (BD, API, conectores, ingestión de evidencia, workflow), E2E del flujo de §16 y seguridad (NFR-005). Cubre PR-NFR-014. Fase MVP.
- Cada regla de dominio de §9 tiene al menos una prueba unitaria que la demuestra.

#### NFR-024: Pruebas de dominio de cumplimiento y de contrato de conectores
Fixtures derivados de `resumenes/`: exactamente 38 controles con la distribución A.2:3, A.3:2, A.4:5, A.5:4, A.6:9, A.7:5, A.8:4, A.9:3, A.10:3; 26 términos de la cláusula 3; 11 objetivos y 7 fuentes del Anexo C; pruebas de contrato del SDK contra el adaptador mock; ninguna prueba afirma integración validada sin credenciales reales. Cubre PR-NFR-015, PR-NFR-016. Fase MVP.

## 8. Requisitos de datos

### 8.1 Modelo de normas versionado
El conocimiento normativo es global y de solo lectura para los tenants (`Standard`, `StandardVersion`, `Clause`, `SubClause`, `Requirement`, `Annex`, `ControlDomain`, `Control`, `ControlGuidance`, `RequirementControlMapping`, `StandardRelationship`, `AuditQuestion`, `SeedRecordSource`). Versiones soportadas desde el inicio: ISO/IEC 42001:2023 + Amd 1:2024; UNE-ISO/IEC 42001:2025 (adopción idéntica, relación `identical_adoption`); ISO 19011:2026 (sustituye a 2018); ISO/IEC 23894:2023 (cláusulas 1-4 y 5.1-5.3 disponibles; resto `SOURCE_DETAIL_REQUIRED`); ISO/IEC 42006:2025 registrada como dependencia pendiente sin contenido (FR-015, FR-018). Cada registro lleva `record_type` (FR-017) y `SeedRecordSource` con `resumenes/NN §sección` (A10).

### 8.2 Semilla desde `resumenes/`
La semilla vive en `packages/standards-seed/`, versionada con semver, construida a partir de `resumenes/01-05, 08-13` y revisada por humanos antes de publicar. Campos mínimos por registro: `identifier`, `title`, `requirement_summary` (paráfrasis propia), `record_type`, `expected_evidence`, `references` (documento fuente y sección), `standard_version`. Ningún módulo de la aplicación lee `resumenes/` en tiempo de ejecución (A4). El detalle ausente se marca `SOURCE_DETAIL_REQUIRED` y se muestra como tal (PR-DR-019). Pruebas de dominio de NFR-024 bloquean la publicación de una versión de semilla inválida. Cambiar una paráfrasis o un identificador es un cambio de interpretación normativa y exige aprobación humana (`CLAUDE.md`).

### 8.3 Derechos de autor
La plataforma nunca almacena, muestra ni exporta texto literal de ISO, IEC o UNE; la carpeta de material licenciado queda fuera de Git; los títulos de cláusulas y controles se usan como identificadores públicos de la norma y las descripciones son paráfrasis propias (NFR-020, brief R3). Los informes exportados citan la norma por identificador, nunca reproducen su texto.

### 8.4 Separación catálogo / estado del tenant
El catálogo (`Requirement`, `Control`) es global; el estado de cumplimiento es del tenant: `RequirementImplementation` (FR-020), `SoAItem` (FR-060), `ControlImplementation` (FR-065). Esto evita duplicar la norma por tenant y permite publicar nuevas versiones sin tocar el estado de nadie. `[ASSUMPTION, DOMAIN_MAP §1.3; pendiente de ratificación §17 D-3]`.

### 8.5 Campos estándar y enumeraciones canónicas
Toda entidad importante lleva `id, tenant_id, created_at, created_by, updated_at, updated_by, status, version, deleted_at` (FR-009). Enumeraciones canónicas en inglés, traducidas por i18n: aplicabilidad (`Applicable`, `Not Applicable`, `Applicable with Alternative Control`, `Pending Assessment`); estado de requisito (10 valores, FR-020); ciclo de control (9 valores, FR-065); evidencia (8 valores, FR-072); documento/política (8 valores, FR-029); auditoría (11 fases, FR-088); hallazgo (configurable, FR-090); incidente (8 valores, FR-085); tratamiento (`Avoid`, `Mitigate`, `Transfer/Share`, `Accept/Retain`); conector (`mock`, `configured`, `validated`); disponibilidad de dato (5 valores, FR-120); preparación (`Ready for internal review`, `Audit-ready`, `Certification preparation status`); dimensiones de impacto con `safety` y `security` distintas. El catálogo completo está en `addendum.md` §C.

### 8.6 Eventos de dominio
Los nombres de evento son los de P2 §65 y los de §41/§17 normalizados a `MAYUSCULAS_CON_GUION_BAJO`; el catálogo consolidado (obligatorios más añadidos [A]) es `DOMAIN_MAP.md` §5. Todo cambio de estado relevante escribe su evento en la misma transacción (outbox, ADR-003); Administration es el único escritor del registro de auditoría y consume todos los eventos (ADR-005).

### 8.7 Propiedad de entidades
Cada entidad pertenece a un único bounded context (`DOMAIN_MAP.md` §4); los demás la referencian por identificador. Los 21 contextos existen en el modelo desde el MVP aunque algunos no tengan UI hasta F2/F3 (A1).

### 8.8 Clasificación y datos personales
La evidencia y los sistemas de IA pueden contener datos personales o sensibles: cada evidencia lleva `classification` (FR-071), cada sistema declara `personal_data` y `sensitive_data` (FR-034), la residencia se declara por tenant (NFR-015) y las exportaciones respetan clasificación y permisos (FR-013). La plataforma no es un sistema de tratamiento de datos de los usuarios finales de los sistemas de IA gobernados; solo almacena lo que la organización decide aportar como evidencia `[ASSUMPTION]`.

## 9. Reglas de dominio (invariantes)

Las reglas de dominio son invariantes que el código debe hacer imposibles de violar; cada una tiene al menos una prueba unitaria (NFR-023). Se conservan los identificadores `PR-DR-nnn` de `REQUIREMENTS_ANALYSIS.md` §4 (enunciado completo allí). La columna FR indica dónde se hace efectiva.

| ID | Regla (resumen) | Fase | FR que la impone |
|---|---|---|---|
| PR-DR-001 | La SoA gobierna los flujos: un control `Not Applicable` no genera tareas, evidencia recurrente ni métricas activas; sigue visible y auditable y entra en la completitud de la SoA. | MVP | FR-061, FR-064, FR-067, FR-100 |
| PR-DR-002 | Al pasar a `Applicable` se activan por evento flujos, tareas, evidencia, seguimiento y métricas. | MVP | FR-064 |
| PR-DR-003 | `Not Applicable` exige justificación documentada; sin ella la transición se rechaza. | MVP | FR-061 |
| PR-DR-004 | `Applicable` exige propietario, estado, expectativas de evidencia, riesgos y requisitos mapeados antes de aprobar la SoA. | MVP | FR-062 |
| PR-DR-005 | Riesgo tratado ≠ plan existente: exige plan aprobado, controles implementados y residual evaluado y aceptado o dentro de criterios. | MVP | FR-045, FR-044 |
| PR-DR-006 | Aceptación de riesgo con rol autorizado, justificación, residual y expiración; al vencer vuelve a revisión. | MVP | FR-046 |
| PR-DR-007 | Un cambio significativo marca riesgo e impacto como "reevaluación requerida" y abre revisión de controles. | MVP* | FR-047, FR-052 |
| PR-DR-008 | El contenido generado por IA se marca, requiere aprobación humana y nunca cambia un estado de cumplimiento. | F2 | FR-128, FR-081 |
| PR-DR-009 | Nunca "certificado ISO"; solo *readiness / coverage / implementation / effectiveness / exposure* y los tres estados de preparación. | MVP | FR-100, FR-113 |
| PR-DR-010 | Toda puntuación expone fórmula, numerador, denominador, exclusiones, registros fuente y marca temporal. | MVP | FR-100, FR-102 |
| PR-DR-011 | Evidencia subida ≠ aceptada: solo un revisor autorizado, distinto del que la subió, la pasa a `Accepted`. | MVP | FR-074, FR-072 |
| PR-DR-012 | El descubrimiento automático nunca marca cumplimiento; solo crea candidatos. | F2 | FR-039 |
| PR-DR-013 | Un sistema no avanza de etapa con puertas obligatorias incompletas salvo excepción autorizada. | F2 | FR-057 |
| PR-DR-014 | Ninguna excepción es permanente. | F2 | FR-070 |
| PR-DR-015 | Ningún registro de cumplimiento se borra en silencio. | MVP | FR-008 |
| PR-DR-016 | Las importaciones nunca sobreescriben sin previsualización, duplicados y confirmación. | F2 | FR-013 |
| PR-DR-017 | Toda decisión de cumplimiento genera registro inmutable con actor, momento, motivo, valor anterior, nuevo y aprobador. | MVP | FR-007, FR-083 |
| PR-DR-018 | UI y datos distinguen siempre `normative_requirement` de orientación; el Anexo B nunca es requisito ni se justifica en la SoA. | MVP | FR-017, FR-061 |
| PR-DR-019 | Nunca se inventan requisitos, controles, identificadores, texto ni capacidades de API; `SOURCE_DETAIL_REQUIRED`. | MVP | FR-018, FR-128 |
| PR-DR-020 | Datos demo y mock etiquetados; nunca representan cumplimiento real. | MVP | FR-040, FR-115 |
| PR-DR-021 | Ningún conector se declara funcional sin validarlo contra la API real; estados `mock`, `configured`, `validated`. | MVP | FR-115 |
| PR-DR-022 | Clasificación y graduación de hallazgos configurables por organización. | MVP | FR-090, FR-006 |
| PR-DR-023 | La conclusión de auditoría deriva de hallazgos y evidencia; no se emite sin hallazgo o conformidad por criterio. | MVP | FR-091 |
| PR-DR-024 | Una CAPA solo se cierra tras verificar la eficacia. | MVP | FR-096 |
| PR-DR-025 | Los paquetes sectoriales son guía, nunca requisitos ISO. | F3 | FR-023 |
| PR-DR-026 | Las funciones LLM de la plataforma son sistemas de IA de su propio inventario. | F2 | FR-130 |
| PR-DR-027 | Los 21 elementos de información documentada obligatoria tienen registro identificable y aparecen en la preparación. | MVP | FR-113, FR-079 |
| PR-DR-028 | El impacto se considera en el riesgo: una evaluación de riesgo no se completa con un impacto posterior sin revisar. | MVP | FR-053 |
| PR-DR-029 | La organización declara sus roles respecto a la IA y estos condicionan la aplicabilidad sugerida. | MVP | FR-025 |
| PR-DR-030 | Los objetivos de la IA son coherentes con la política, medibles y con responsable, recursos, plazo y evaluación. | MVP | FR-031 |

## 10. Métricas de éxito

*Cada métrica cita los FR que valida; cada contramétrica nombra lo que no debe empeorar. Metas heredadas del brief §7 con `[ESTIMACIÓN]`; definiciones operativas en brief `addendum.md` §F. Vocabulario prescrito: preparación, cobertura, implementación, eficacia, exposición.*

**Primarias**

- **SM-1 Operar un SGIA de punta a punta.** Organizaciones piloto reales que completan el flujo de §16.1 (15 pasos, sin conectores) con aprobación humana registrada en cada decisión. Meta: ≥ 1 real + demo al cierre del MVP; ≥ 3 a los 6 meses del lanzamiento `[ESTIMACIÓN]`. Valida FR-001 a FR-007, FR-025 a FR-031, FR-034, FR-041 a FR-047, FR-050 a FR-053, FR-059 a FR-067, FR-071 a FR-077, FR-087 a FR-098, FR-106, FR-113.
- **SM-2 Trazabilidad útil.** % de controles `Applicable` con ≥ 1 requisito, ≥ 1 riesgo y ≥ 1 evidencia `Accepted` enlazados. Meta: ≥ 90 % en pilotos `[ESTIMACIÓN]`. Valida FR-019, FR-062, FR-075, FR-125.
- **SM-3 Evidencia defendible.** % de evidencia `Accepted` con hash y procedencia completa. Meta: 100 %. Valida FR-071, FR-073, FR-074, FR-076.
- **SM-4 Honestidad métrica.** % de puntuaciones mostradas que exponen fórmula y registros fuente. Meta: 100 %. Valida FR-100, FR-102, FR-113.

**Secundarias**

- **SM-5 Valor para el auditor.** Tiempo de un usuario `Auditor` para seguir un hilo requisito → control → evidencia aceptada en la organización demo. Meta: < 2 min. Valida FR-021, FR-089, FR-125, FR-126.
- **SM-6 Bilingüismo.** Cobertura de cadenas ES/EN y conformidad terminológica con `GLOSSARY.md`. Meta: 100 % de la UI del MVP. Valida FR-132, NFR-018.
- **SM-7 Autogobernanza.** Funciones LLM del producto registradas en el inventario del tenant plataforma con controles A.6/A.8/A.9. Meta: 100 % (F2). Valida FR-130, FR-128.
- **SM-8 Negocio** `[SUPUESTO heredado del brief]`. Pilotos pagados o cartas de intención (empresa y consultora). Meta: 2-3 antes de GA `[ESTIMACIÓN]`. Valida la tesis de §3; depende de Q2-Q4.

**Contramétricas (no optimizar)**

- **SM-C1** Pasos del flujo E2E completados sin aprobación humana registrada = 0. Contrapesa SM-1 (impide "acelerar" saltándose decisiones). Valida FR-083, FR-007.
- **SM-C2** % de enlaces creados por IA sin revisor = 0. Contrapesa SM-2 (impide inflar la trazabilidad con sugerencias no revisadas). Valida FR-128.
- **SM-C3** Evidencia `Accepted` por el mismo usuario que la subió = 0. Contrapesa SM-3. Valida FR-074.
- **SM-C4** Apariciones de "compliance score", "certificado" o "certified" en UI, API o informes = 0. Contrapesa SM-4. Valida FR-100, PR-DR-009.
- **SM-C5** Hallazgos de auditoría atribuibles a datos inconsistentes de la plataforma = 0. Contrapesa SM-5 (rapidez no a costa de fiabilidad). Valida FR-014 (F2), FR-125.
- **SM-C6** Incidencias de terminología reportadas por implementadores sin tendencia creciente entre versiones. Contrapesa SM-6. Valida FR-132.
- **SM-C7** Coste LLM por tenant dentro del presupuesto configurado; bloqueos suaves explicados. Contrapesa SM-7. Valida FR-129.
- **SM-C8** Tiempo de onboarding de un tenant nuevo ≤ 1 día laborable. Contrapesa SM-8 (crecer sin que cada cliente sea un proyecto). Valida FR-001 a FR-006 (UJ-7).

## 11. Alcance por fases

Hoja de ruta única (ajuste A2) detallada en `docs/product/ROADMAP.md`; recuento por `PR-*` en `PRODUCT_SCOPE.md`: 141 requisitos en MVP, 54 en Fase 2, 10 en Fase 3 (205).

### 11.1 MVP: corte vertical operable

Principio A1: arquitectura de dominio completa (21 contextos, todas las entidades, todo el catálogo de eventos) e implementación solo de la UI, API y flujos que una organización real necesita para operar un SGIA mínimo y llegar a una auditoría interna con evidencia. Cada acción recorre `UI → API → Domain Service → Database → Audit Log`.

| Capacidad | FR en MVP (incl. MVP*) |
|---|---|
| Fundación, identidad, RBAC y administración | FR-001 a FR-013 (FR-013 solo exportación CSV/JSON) |
| Motor de normas y requisitos | FR-015 a FR-021, FR-024 |
| SGIA: contexto, partes, alcance, política, roles, objetivos | FR-025 a FR-031 |
| Inventario de IA | FR-034, FR-035, FR-036, FR-040 |
| Riesgo | FR-041 a FR-047 |
| Impacto | FR-050 a FR-053 |
| SoA y controles | FR-059 a FR-067 |
| Evidencia | FR-071 a FR-075, FR-077 |
| Documentos | FR-079, FR-080 |
| Tareas, aprobaciones, notificaciones | FR-082 a FR-084 |
| Auditoría interna | FR-087 a FR-093 |
| CAPA | FR-094 a FR-096 |
| Revisión por la dirección | FR-098, FR-099 |
| KPI, puntuación, dashboard | FR-100 a FR-102 |
| Informes esenciales | FR-105, FR-106 |
| Preparación para certificación | FR-113 |
| Integraciones (SDK + mock) | FR-114 a FR-116 |
| Búsqueda, trazabilidad, explicación | FR-123, FR-125, FR-126 |
| Navegación y bilingüismo | FR-131, FR-132 |
| NFR | NFR-001 a NFR-003, NFR-005 a NFR-024 (NFR-004, NFR-006, NFR-008, NFR-015 en su parte MVP) |

### 11.2 Fuera del MVP: Fase 2 (SGIA conectado y asistido)

FR-014, FR-022, FR-032, FR-033, FR-037, FR-039, FR-048, FR-049, FR-054 a FR-058, FR-068 a FR-070, FR-076, FR-078, FR-081, FR-085, FR-086, FR-097, FR-103, FR-107 a FR-110, FR-117 a FR-121, FR-127 a FR-130; más la parte diferida de los `MVP*` (dimensiones adicionales de riesgo, roles por unidad y por sistema, SAML, revisiones de acceso, rotación de credenciales, exportación DOCX/XLSX, `legal hold` y borrado gobernado, visualización gráfica de trazabilidad, explicaciones generadas, importación, residencia efectiva, antimalware, verificación de carga).

`[NOTE FOR PM]` Los conectores reales y todas las funciones LLM están fuera del MVP por decisión A1, no por falta de valor: son las dos capacidades que la competencia sí ofrece (brief `addendum.md` §A.3) y deben ser lo primero de Fase 2. El primer conector real depende del piloto (Q9).

### 11.3 Fuera del MVP: Fase 3 (extensiones)

FR-023, FR-038, FR-104, FR-111, FR-112, FR-122, FR-124; ABAC (parte diferida de FR-004). Fase 3 es opcional por bloques: cada elemento puede adelantarse si un cliente lo exige sin afectar al núcleo.

## 12. No-objetivos explícitos

Lo que el producto **no es ni hará en ninguna fase**:

- **No certifica.** Nunca emite, sugiere ni insinúa un certificado ISO/IEC 42001; la certificación la realizan organismos acreditados (PR-DR-009).
- **No reproduce la norma.** No muestra ni almacena texto literal de ISO, IEC o UNE (NFR-020).
- **No sustituye el juicio humano.** Ninguna función de IA aprueba, acepta, cierra ni declara conforme (PR-DR-008).
- **No ejecuta ni aloja modelos de IA** de los clientes; gobierna, no infiere.
- **No es organismo de auditoría de tercera parte** ni implementa ISO/IEC 42006 (fuera del corpus, PR-EXT-013).
- **No asesora legalmente.** El módulo regulatorio mapea obligaciones cargadas por la organización; no interpreta leyes.
- **No lee `resumenes/` en tiempo de ejecución** (A4).
- **No es un GRC generalista multi-marco** en el MVP: no compite en amplitud de marcos (27001, SOC 2…); el mapeo cruzado con ISO 27001 es candidato de fase posterior `[NOTE FOR PM]`.
- **No es un marketplace de consultoras** `[SUPUESTO heredado del brief]`.
- **No ofrece esquema de base de datos por tenant ni microservicios** en el MVP (ADR-002, ADR-003).

`[NON-GOAL for MVP]` recordatorios distribuidos en §6: vista agregada multi-tenant para consultoras (UJ-6); estado `AI Generated` alcanzable (FR-029); borrado físico de registros (FR-008).

## 13. Supuestos de producto

Todos requieren confirmación humana en el checkpoint de planificación (P1 §40, A13). Los supuestos puntuales inline están indexados en §19.

1. `[ASSUMPTION]` **Cliente objetivo inicial:** organización mediana o grande hispanohablante (España/LatAm) que **usa** IA de terceros más que la desarrolla; por eso A.9 y A.10 tienen la misma prioridad que A.6 en la semilla y el ciclo de vida completo espera a Fase 2 (PRODUCT_SCOPE §7.1; brief S1).
2. `[ASSUMPTION]` **Canal consultora** como segmento secundario, posicionado como "herramienta de la consultora" (brief D10, S1).
3. `[ASSUMPTION]` **Modelo comercial** SaaS por suscripción con niveles por sistemas de IA/usuarios y plan consultora; precios no definidos (brief S2).
4. `[ASSUMPTION]` **Equipo pequeño** (1-3 personas más agentes), lo que justifica monolito modular y pg-boss (A8).
5. `[ASSUMPTION]` **Política de IA y objetivos entran en MVP** como gestión documental sin generación LLM (C2, C3; §17 D-1).
6. `[ASSUMPTION]` **El tenant demo** se construye al final del MVP y no sustituye a la organización real en los criterios de salida.
7. `[ASSUMPTION]` **Autenticación local + MFA TOTP + OIDC** basta para el MVP; SAML se difiere.
8. `[ASSUMPTION]` **RPO ≤ 1 h, RTO ≤ 8 h, disponibilidad ≥ 99,5 %** son objetivos de diseño, no compromisos contractuales (NFR-008).
9. `[ASSUMPTION]` **El 23 % de ISO/IEC 23894** disponible basta para la semilla del MVP porque el Anexo C de 42001 cubre la taxonomía de fuentes de riesgo (C11).
10. `[ASSUMPTION]` **"Informes esenciales"** = #2, #3, #5 y #13 de P2 §27.
11. `[ASSUMPTION]` **Piloto:** Micronotes/Eligen podría ser el primer tenant real (brief S3; Q4).
12. `[ASSUMPTION]` **Primer conector real** tras el MVP: uno de Azure AI Foundry, OpenAI o Anthropic, según el piloto (Q9).

## 14. Riesgos del producto

| # | Riesgo | Impacto | Mitigación en este PRD |
|---|---|---|---|
| R1 | **Alcance desbordado**: 73 secciones de P2; tentación de construir todo | Nunca se lanza | MVP vertical A1 (§11); un cambio OpenSpec por capacidad (`ROADMAP.md`); checkpoint humano antes de codificar |
| R2 | **Interpretación normativa incorrecta** en la semilla o en la lógica de flujos | Pérdida de credibilidad ante auditores | Semilla revisada por humano con cita a `resumenes/NN` (FR-018, §8.2); pruebas de dominio (NFR-024); `SOURCE_DETAIL_REQUIRED`; C5 marcado como decisión pendiente (§17 D-2) |
| R3 | **Derechos de autor** ISO/IEC/UNE | Riesgo legal | Solo paráfrasis e identificadores (NFR-020, §8.3); material licenciado fuera de Git |
| R4 | **Competencia** GRC anglófona con marca y capital | Presión de precio y de funcionalidades | Vertical, en español, que opera el SGIA (§3); conectores y IA asistida primeros en F2 (§11.2) |
| R5 | `[SUPUESTO]` **Demanda hispanohablante** suficiente y dispuesta a pagar | Modelo de negocio | Validar con 2-3 pilotos y una consultora (SM-8); `bmad-deep-recon` de mercado pendiente |
| R6 | **Confianza en la IA asistente**: alucinación de requisitos u obligaciones | Decisiones erróneas de cumplimiento | IA sugiere/humano aprueba (FR-128); validación de referencias contra el catálogo; sin LLM en MVP |
| R7 | **Seguridad multi-tenant** (fuga cross-tenant, escalada) | Fin del producto | RLS + `tenant_id` (FR-002), pruebas de seguridad en cada cambio (NFR-005), secretos vía gestor (FR-116), cadena de hashes (FR-007) |
| R8 | **Corpus incompleto**: 23894 al 23 %, 42006 ausente | Módulos de riesgo y auditoría externa limitados | Marcar vacíos (§8.1); adquisición de fuentes (Q8) |
| R9 | `[SUPUESTO]` **Equipo pequeño y método pesado** (BMAD + OpenSpec) | Ceremonia > entrega | Agentes en paralelo; la trazabilidad del desarrollo se convierte en evidencia del tenant plataforma (FR-130) |
| R10 | **Deriva regulatoria** (AI Act, LatAm) | Requisitos externos cambiantes | Módulo regulatorio desacoplado (FR-022); `StandardVersion` (FR-015) |
| R11 | **Matriz rol × permiso mal calibrada** (C15) | Bloqueos operativos o privilegios excesivos en pilotos | Propuesta en `addendum.md` §B con `[ASSUMPTION]`; roles personalizables (FR-004); decisión §17 D-6 |
| R12 | **Umbrales inventados por el PRD** (plazos, tamaños, p95) resultan inadecuados | Fricción en pilotos | Todos etiquetados y configurables por tenant donde tiene sentido (S3, §19); revisión en arquitectura |
| R13 | **Semántica de `Not Applicable` en requisitos** (C5) contradice la norma si se aplica sin restricción | Hallazgo de auditoría de certificación | Restricción a orientación y normas de apoyo, advertencia y justificación (FR-020); decisión §17 D-2 |

## 15. Dependencias

### 15.1 Externas (PR-EXT)

| ID | Dependencia | Fase | FR/NFR |
|---|---|---|---|
| PR-EXT-001 | Azure / Azure AI Foundry (APIs de gestión y proyectos) | F2 | FR-117 |
| PR-EXT-002 | OpenAI API | F2 | FR-118 |
| PR-EXT-003 | Anthropic API | F2 | FR-119 |
| PR-EXT-004 | Proveedor de LLM tras la abstracción | F2 | FR-129 |
| PR-EXT-005 | Almacenamiento de objetos compatible S3 / Azure Blob con prefijo por tenant y URLs firmadas | MVP | FR-071, FR-002 |
| PR-EXT-006 | Correo transaccional | MVP | FR-084 |
| PR-EXT-007 | Proveedor de identidad OIDC (SAML F2) | MVP* | FR-003 |
| PR-EXT-008 | Gestor de secretos (Key Vault / Secrets Manager / Vault) tras abstracción | MVP | FR-116 |
| PR-EXT-009 | PostgreSQL con Row Level Security | MVP | FR-002 |
| PR-EXT-010 | Motor antimalware | F2 | NFR-004 |
| PR-EXT-011 | Bibliotecas de renderizado PDF/DOCX/XLSX con licencia compatible | MVP* | FR-080, NFR-003 |
| PR-EXT-012 | Backend de observabilidad OpenTelemetry | MVP | NFR-013 |
| PR-EXT-013 | ISO/IEC 42006 (conocimiento; no está en el corpus) | F3 | §12, Q8 |
| PR-EXT-014 | Conectores futuros | F3 | FR-122 |
| PR-EXT-015 | Fuentes de requisitos regulatorios externos | F2 | FR-022 |

### 15.2 De conocimiento y de proceso

- `resumenes/01-13` como nivel 0/1; cualquier corrección de un resumen obliga a revisar la semilla afectada (§8.2).
- Adquisición del resto de ISO/IEC 23894 y de ISO/IEC 42006 (Q8) antes de completar el módulo de riesgo y cualquier soporte a auditoría de tercera parte.
- Aprobación humana del checkpoint de planificación (P1 §40, A13) antes de `foundation-project-bootstrap`; y de los puntos de §17 antes de los cambios OpenSpec que dependen de ellos (indicados en `ROADMAP.md`).
- Salidas BMAD siguientes: `bmad-ux`, `bmad-architecture` (ADR-001 a ADR-005 ya decididos en el brief; faltan algoritmo de residual, criterios de preparación, modelo de permisos por objeto), `bmad-create-epics-and-stories`.

## 16. Criterios de aceptación

### 16.1 Flujo E2E del MVP (prueba Playwright principal)

El MVP se considera completo cuando **una organización real** (no el tenant demo) ejecuta este flujo en la aplicación desplegada, con un usuario por rol, dejando registros persistentes, trazables y auditables en cada paso. Sustituye los pasos "Connect Azure/OpenAI/Anthropic → Discover → Validate" de P2 §69, que vuelven en Fase 2 (C10). Corresponde a UJ-7 → UJ-2 → UJ-1 → UJ-3 → UJ-4 → UJ-5.

| Paso | Acción | Condición de aceptación verificable | FR |
|---|---|---|---|
| 1 | Crear organización | Tenant con `data_region`, administrador invitado y activado; RLS impide ver datos de otro tenant (prueba automática) | FR-001 a FR-005 |
| 2 | Definir contexto | ≥ 1 cuestión interna, ≥ 1 externa, ≥ 1 rol de IA de la organización y una revisión registrada | FR-025 |
| 3 | Definir partes interesadas | ≥ 3 partes con necesidades, expectativas, influencia y fecha de revisión | FR-026 |
| 4 | Definir alcance del SGIA | ≥ 1 unidad de negocio y ≥ 1 sistema de IA, una exclusión justificada, aprobado y versionado (v1); `SCOPE_CHANGED` en el registro | FR-027, FR-007 |
| 5 | Inventariar sistemas de IA | Un sistema con modelo, versión, proveedor, dataset, uso previsto, mal uso previsible, supervisión humana y propietarios | FR-034, FR-035, FR-036 |
| 6 | Evaluar riesgos | Metodología aprobada; ≥ 2 riesgos con fuente del Anexo C, inherente y residual con la escala del tenant; un plan aprobado; una aceptación con expiración | FR-041 a FR-046 |
| 7 | Evaluar impacto | Una evaluación con la plantilla semilla, ≥ 1 hallazgo con mitigación, aprobada y enlazada al riesgo | FR-050 a FR-053 |
| 8 | Completar la SoA | 38 controles con decisión; ≥ 1 `Not Applicable` con justificación; SoA aprobada (v1) | FR-059 a FR-063, FR-067 |
| 9 | Activar controles | Los aplicables tienen propietario y generan tareas y solicitudes de evidencia por evento; los no aplicables no generan nada y siguen visibles | FR-064, FR-065 |
| 10 | Recoger evidencia | ≥ 1 evidencia por control activo probado, con hash, aceptada por revisor distinto del que la subió; una rechazada con motivo | FR-071 a FR-075 |
| 11 | Auditoría interna | Programa anual; auditoría con plan, criterios (requisitos y controles), evidencia por criterio, hallazgos de ≥ 2 tipos, conclusión e informe | FR-087 a FR-091 |
| 12 | Registrar hallazgo | Una no conformidad derivada de la auditoría, enlazada a requisito, control y evidencia | FR-090, FR-094 |
| 13 | Gestionar CAPA | Corrección, causa raíz (5 porqués), acción con propietario y plazo, verificación de eficacia y cierre; cierre sin verificación rechazado | FR-094 a FR-096 |
| 14 | Revisión por la dirección | Entradas agregadas (riesgos, auditoría, CAPA, KPI), decisiones y acciones registradas, acta exportada a PDF | FR-098, FR-099, FR-080 |
| 15 | Informe de preparación | *Audit Readiness Report* y lista de comprobación con puntuaciones explicadas (fórmula, numerador, denominador, exclusiones); ninguna pantalla dice "certificado" | FR-100, FR-106, FR-113 |

### 16.2 Criterios técnicos de salida del MVP

1. Pruebas de dominio en verde: 38 controles con distribución A.2:3, A.3:2, A.4:5, A.5:4, A.6:9, A.7:5, A.8:4, A.9:3, A.10:3; 26 términos; 11 objetivos y 7 fuentes del Anexo C (NFR-024).
2. Pruebas de seguridad en verde: cross-tenant, escalada de privilegios, evidencia sin permiso, exposición de secretos, autorización a nivel de objeto (NFR-005).
3. Cadena de hashes verificable por tenant; toda decisión de cumplimiento del flujo aparece con actor, motivo y valor anterior (FR-007, NFR-017).
4. UI completa en ES y EN; ninguna clave sin traducción (FR-132).
5. Auditoría automática WCAG 2.2 AA sin errores críticos en las pantallas del flujo (NFR-009).
6. Ningún botón ni entrada de menú sin implementación (NFR-011, FR-131).
7. Semilla sin texto literal; cada registro cita `resumenes/NN §sección` (NFR-020, FR-018).
8. SDK de conectores con adaptador mock etiquetado y pruebas de contrato; estado `mock` visible (FR-115).
9. `docs/compliance/TRACEABILITY_MATRIX.md` con una fila por `PR-*` del MVP (`TRACEABILITY_MODEL.md`).
10. Checkpoint de aceptación humana (P1 §40, A13) firmado antes de declarar el MVP cerrado.

### 16.3 Definición de hecho por FR

Un FR se declara implementado solo cuando: sus consecuencias verificables tienen prueba automatizada; su cambio OpenSpec está archivado; su fila de la matriz de trazabilidad está `verified` con implementación, prueba y evidencia; y, si es MVP, participa del flujo de §16.1 o de los criterios de §16.2. "Implementado" sin evidencia es `unverified`.

## 17. Decisiones que requieren aprobación humana

Este PRD adopta **decisiones provisionales** para no bloquear la arquitectura; cada una puede revertirse en el checkpoint. Están registradas en `.memlog.md`. Ninguna cambia la interpretación de la norma sin pasar por aprobación (`CLAUDE.md`).

| ID | Decisión provisional | Alternativa considerada | Impacto si se revierte | Dónde |
|---|---|---|---|---|
| **D-1** (C2, C3) | La **política de IA** y los **objetivos de la IA** entran en el MVP como información documentada gestionada (ciclo de vida, aprobación, versiones) **sin** generación por LLM; los objetivos con métricas manuales. | Diferirlos a F2 como sugería la lista de A1. Se descarta porque 5.2 y 6.2 son información documentada obligatoria y aparecen en la lista de preparación de §53 y en el flujo §69. | Sin ellos el paso 15 del E2E no puede marcar "política aprobada" ni "objetivos"; la preparación queda incompleta. Revertir reduce el MVP en dos cambios OpenSpec. | FR-029, FR-031, §13.5 |
| **D-2** (C5) | El estado `Not Applicable` de **requisito** se conserva en la enumeración pero la UI lo restringe a registros `implementation_guidance` y a normas de apoyo; para un `normative_requirement` de ISO/IEC 42001 exige justificación y advertencia. Razón: la norma no admite excluir requisitos de las cláusulas 4-10 para declarar conformidad; solo los controles del Anexo A admiten exclusión justificada. | (a) Permitirlo sin restricción, como P2 §6. (b) Eliminarlo de la enumeración. | (a) expone a los clientes a hallazgos de certificación; (b) rompe compatibilidad con P2. **Es una interpretación normativa: punto de aprobación humana obligatorio.** | FR-020, R13 |
| **D-3** | **Separación catálogo / estado del tenant**: `Requirement` y `Control` globales; `RequirementImplementation`, `SoAItem` y `ControlImplementation` por tenant. | Duplicar la norma por tenant (más simple de consultar, imposible de actualizar de versión sin migración masiva). | Afecta al modelo de datos de casi todos los contextos; debe fijarse antes de `standards-and-requirements-engine`. | §8.4, FR-020, FR-060, FR-065 |
| **D-4** (C7, C14) | **`Finding` genérico** con `source_type`; `AuditFinding` e `ImpactAssessmentFinding` como especializaciones. **`AIProvider`** (proveedor tecnológico), **`Supplier`** (tercero contractual A.10.3) e **`Integration`** (conector) son tres entidades distintas. | Tres entidades de hallazgo independientes; una sola entidad "proveedor". | Sin `Finding` genérico, CAPA necesita tres flujos; sin la triple distinción, A.10 y el hub de conectores se mezclan. | FR-094, FR-036, FR-114 |
| **D-5** (C13) | La etapa `Monitoring` del ciclo de vida se nombra `Operation & Monitoring` (ES "operación y seguimiento") para alinearse con A.6.2.6. | Mantener `Monitoring`. | Solo nomenclatura; F2. | FR-055 |
| **D-6** (C15) | **Matriz rol × permiso por defecto** propuesta en `addendum.md` §B: `controls.activate/deactivate` y `soa.approve` para `Compliance Manager` y `AI Governance Manager`; `risks.accept` para `Risk Manager` y `Executive`; `evidence.approve` para `Reviewer`, `Compliance Manager`, `AI Governance Manager`; `audits.execute` solo `Auditor`; `management_review.approve` solo `Executive`. | Que cada tenant la configure desde cero. | Roles personalizables (FR-004) mitigan; pero los pilotos necesitan un punto de partida. | FR-004, R11 |
| **D-7** | **Umbrales operativos** del PRD (RPO/RTO/disponibilidad, plazos de aviso a 30 días, retención 7 años, invitación 7 días, tamaño de archivo, p95, MFA por defecto para ejecutivos y administradores). | Dejarlos abiertos hasta arquitectura. | Bajo; casi todos son configurables por tenant. | S3, §19, NFR-007, NFR-008 |
| **D-8** | **Nombres de los primeros cambios OpenSpec** según P1 §38 (`foundation-project-bootstrap`, `organization-tenancy`, `identity-and-rbac`, `immutable-audit-trail`, `standards-and-requirements-engine`), lo que parte `identity-tenancy-rbac` de PRODUCT_SCOPE en dos y renombra otros dos; total 24 cambios MVP. | Mantener los 23 nombres de PRODUCT_SCOPE. | Solo nomenclatura y granularidad; ninguno afecta al alcance. | `ROADMAP.md` §3 |
| **D-9** | **Política y objetivos se implementan justo después del contexto y alcance** (fase 3 de P1), antes del inventario, en lugar del puesto 18 de P1 §37. | Seguir P1 §37 literalmente. | Ninguno funcional; P2 §64 fase 3 (AIMS Core) los agrupa así. | `ROADMAP.md` §3 |

**Preguntas abiertas heredadas del brief (§10) que siguen sin decisión** y que este PRD trata como sigue:

| Q | Pregunta | Efecto en el PRD mientras no se responde |
|---|---|---|
| Q1 | Nombre comercial y marca (¿Eligen/Micronotes o marca nueva?) | Título de trabajo; sin impacto funcional |
| Q2 | Segmento de lanzamiento: ¿empresa final o consultora? ¿Qué países? | UJ-6 y las capacidades multi-cliente quedan en F2/F3 `[ASSUMPTION S2]`; SM-8 |
| Q3 | Modelo de negocio y precios | Sin FR de facturación en ninguna fase; supuesto §13.3 |
| Q4 | Organización piloto (¿Micronotes/Eligen? ¿consultora o academia PECB?) | SM-1 exige ≥ 1 real; el criterio de salida del MVP depende de tenerla |
| Q5 | ¿ES/EN basta o PT-BR pronto? | NFR-018 diseñado para N idiomas; solo ES/EN en MVP |
| Q6 | Residencia de datos y regiones; RPO/RTO objetivo | NFR-008, NFR-015 con valores `[ASSUMPTION]` |
| Q7 | ¿AI Act (y qué normativa LatAm) en MVP o después? | FR-022 en F2 sin jurisdicción fijada |
| Q8 | ¿Se adquirirá ISO/IEC 42006 y el resto de 23894? | §8.1 marca vacíos; R8 |
| Q9 | Primer conector real tras el MVP | Orden dentro de F2 en `ROADMAP.md` marcado como dependiente del piloto |
| Q10 | ¿Certificación 42001/27001 del propio producto y plazo? | FR-130 y `TRACEABILITY_MODEL.md` §7 preparan la evidencia; sin FR adicional |

## 18. Preguntas abiertas de producto y arquitectura

1. **Algoritmo de riesgo residual** (FR-044): cómo pondera la eficacia declarada frente a la probada, y qué hace con controles `Planned`. Decisión de arquitectura con revisión del `Risk Manager` piloto.
2. **Criterios exactos de `Ready for internal review` / `Audit-ready` / `Certification preparation status`** (FR-113): qué combinación de la lista de 13 puntos y de los 21 elementos documentales define cada estado.
3. **Recalculo de estado de requisito** (FR-020): sugerencia frente a cambio automático; el PRD elige sugerencia con confirmación humana; validar con el piloto si genera demasiadas tareas.
4. **`Competency`/`Training*`**: ¿contexto propio "People & Competence" en F2? (`DOMAIN_MAP.md` §7.2).
5. **`Exception`**: ¿vive en Controls & SoA o en Workflow como aprobación especializada? (`DOMAIN_MAP.md` §7.3).
6. **AI Assistance** como contexto separado con inventario propio en AI Portfolio (`DOMAIN_MAP.md` §7.4): confirmar en arquitectura.
7. **Administration como único escritor del registro de auditoría** y consumidor del outbox completo (`DOMAIN_MAP.md` §7.5): confirmar en arquitectura (ADR-005).
8. **Separación de funciones cuando el tenant es muy pequeño** (FR-074, FR-096): ¿se permite excepción registrada cuando solo hay un usuario con permiso?
9. **Plantillas de consultora y panel multi-cliente** (UJ-6): alcance y fase, tras validar con una consultora real.
10. **Mapeo cruzado con ISO/IEC 27001**: la competencia lo ofrece; ¿fase y forma? (§12).

## 19. Índice de supuestos

*Toda etiqueta `[ASSUMPTION]` o `[SUPUESTO]` inline del documento, para confirmación explícita. Los heredados del brief conservan su origen.*

| # | Sección | Supuesto |
|---|---|---|
| A1 | §1, §2.1 | Las organizaciones hispanohablantes operan hoy el SGIA con hojas de cálculo, plantillas y GRC anglófonas (heredado del brief) |
| A2 | §4.3 UJ-4, FR-093 | Acceso de solo lectura para auditor externo (S1) |
| A3 | §4.3 UJ-6, FR-004 | Plantillas de consultora y panel multi-cliente en F2/F3; sin vista agregada en MVP (S2) |
| A4 | §4.3 UJ-7, FR-008 | `data_region` de ejemplo `eu-west`; retención por defecto 7 años |
| A5 | FR-003 | MFA obligatoria por defecto para `Executive`, `Organization Administrator`, `Platform Administrator`; contraseña mínima 12 caracteres |
| A6 | FR-004, §17 D-6 | Matriz rol × permiso por defecto (C15) |
| A7 | FR-005 | Invitación expira a los 7 días |
| A8 | FR-020, §17 D-2 | Restricción del estado `Not Applicable` de requisito (C5) |
| A9 | FR-020 | `Implemented` exige ≥ 1 evidencia `Accepted` |
| A10 | FR-025 | Los roles respecto a la IA condicionan la aplicabilidad sugerida (heredado de PR-DR-029) |
| A11 | FR-027 | El alcance no se aprueba sin ≥ 1 unidad de negocio y ≥ 1 sistema de IA |
| A12 | FR-034 | Campos obligatorios mínimos de un sistema de IA para entrar en alcance |
| A13 | FR-044, §18.1 | Un control `Planned` o sin evidencia aceptada no reduce el residual |
| A14 | FR-046, FR-070, FR-077 | Avisos de expiración a 30 días |
| A15 | FR-055, §17 D-5 | Etapa `Operation & Monitoring` (C13) |
| A16 | FR-065 | `Implemented` exige evidencia aceptada; `Operating` exige evidencia vigente |
| A17 | FR-073 | Hash SHA-256 |
| A18 | FR-074 | Exigir un segundo usuario con permiso cuando el único revisor es quien subió (heredado de PR-DR-011) |
| A19 | FR-085 | Cerrar un incidente exige lecciones aprendidas |
| A20 | FR-087 | Preaviso de auditoría a los auditados 15 días antes |
| A21 | FR-088 | Un auditor no audita controles de los que es propietario |
| A22 | FR-095 | Causa raíz obligatoria antes de definir acción correctiva |
| A23 | FR-096 | Verificador de eficacia distinto del propietario de la acción |
| A24 | FR-113, §18.2 | Los criterios exactos de los estados de preparación se fijan en arquitectura |
| A25 | NFR-001 | Rate limit inicial 100 peticiones/min por usuario; URL firmada ≤ 15 min |
| A26 | NFR-004 | Tamaño máximo de archivo 100 MB |
| A27 | NFR-006, NFR-007 | Umbrales de carga y p95 `[ESTIMACIÓN]` |
| A28 | NFR-008, §13.8 | Disponibilidad ≥ 99,5 %, RPO ≤ 1 h, RTO ≤ 8 h, prueba de restauración semestral |
| A29 | NFR-009 | Usable desde 768 px |
| A30 | NFR-013 | Prueba de redacción de logs |
| A31 | NFR-015 | Regiones candidatas UE / LatAm / EE. UU. |
| A32 | NFR-020 | Mecanismo de detección de copia literal frente a los resúmenes |
| A33 | §8.4, §17 D-3 | Separación catálogo / estado del tenant |
| A34 | §8.8 | La plataforma no trata datos de usuarios finales de los sistemas gobernados salvo lo aportado como evidencia |
| A35 | §10 SM-8, §13.3, §12 | Modelo de negocio, cifras objetivo y no-marketplace (heredados del brief) |
| A36 | §13.1-13.12 | Los doce supuestos de producto de §13 |
| A37 | §11.2 `[NOTE FOR PM]` | Conectores e IA asistida son lo primero de F2 |

---

*Fin del PRD. Siguiente paso BMAD sugerido: `bmad-ux` (especificación de experiencia sobre UJ-1 a UJ-7 y FR-131/FR-132) y `bmad-architecture` (con las decisiones de §17 ratificadas), después `bmad-create-epics-and-stories`. Invocar `bmad-help` para el enrutado autoritativo.*
