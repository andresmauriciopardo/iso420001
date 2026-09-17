---
titulo: "Plantilla Excel: Análisis de Riesgo del SGIA (Sistema de Gestión de IA) v.03 – inventario de activos de IA, tabla de riesgos, mapa de riesgos y dashboard"
archivo_fuente: "Plantillas para ejercicios y proyectos/Analisis de Riesgo SGIA v.03.xls"
tipo_documento: plantilla_excel
organismo_emisor: "Material de curso (autor no identificado en el archivo; ejercicio de implementación ISO/IEC 42001)"
fecha_publicacion_fuente: "no consta (versión v.03; menciona ISO/IEC 42001:2023)"
idioma_fuente: es
paginas_o_hojas: "5 hojas: Información, Activos, Tabla de Riesgos, Mapa Riesgos, Dashboard"
version_resumen: "1.0"
fecha_resumen: "2026-09-17"
generado_por: "Claude (agente Analista de Plantillas – Análisis de Riesgo SGIA, FODA vs Riesgo y Stakeholders)"
palabras_clave: [evaluación de riesgos de IA, inventario de activos de IA, sensibilidad, impacto, amenaza, vulnerabilidad, probabilidad, riesgo inherente, riesgo residual, umbrales de riesgo, fuentes de riesgo, ciclo de vida de la IA, mapa de riesgos, dashboard, ISO/IEC 42001 6.1.2, ISO/IEC 23894]
documentos_relacionados_en_corpus: [11_Plantilla_Actividad2_FODA-vs-Riesgo_Contexto-Organizacion.md, 12_Plantilla_Actividad3_Stakeholders_Partes-Interesadas.md]
---

## 1. Resumen ejecutivo

"Analisis de Riesgo SGIA v.03.xls" es una plantilla de hoja de cálculo para ejecutar la **evaluación de riesgos de un Sistema de Gestión de Inteligencia Artificial (SGIA)**, el proceso que ISO/IEC 42001 exige en 6.1.2 (evaluación de riesgos de IA) y 8.2 (ejecución operativa), y cuyo resultado alimenta el tratamiento de 6.1.3. Parte de un **inventario de activos de IA** (hoja "Activos"), valora cada activo en siete dimensiones de **sensibilidad** propias de la IA (ética e inclusión, transparencia y explicabilidad, responsabilidad, seguridad y robustez, privacidad, beneficio social y supervisión humana), estima el **impacto al negocio** en cinco categorías (legal, reputacional, civil, operativo, sociedad), caracteriza la **vulnerabilidad** (severidad y exposición) y la **amenaza** (agente, capacidad y motivación), asigna una **probabilidad** y calcula un **riesgo total** multiplicativo cuyo máximo es 3.375, clasificado en BAJO, MEDIO o ALTO mediante umbrales y recalculado como **riesgo residual** tras describir los controles existentes. Un catálogo de **fuentes de riesgo (FR1-1 a FR5-2)** por fases del ciclo de vida de la IA conecta la plantilla con ISO/IEC 23894. Las hojas "Mapa Riesgos" y "Dashboard" consolidan los resultados para la dirección. El caso de ejemplo es una **telco** con dos activos: una IA de perfilamiento y marketing predictivo y una plataforma de Big Data de geolocalización GPS. La plantilla importa porque materializa el vínculo entre activos, principios de IA responsable, riesgo y controles del Anexo A.

## 2. Identificación y contexto del documento

- **Tipo**: libro Excel en formato binario antiguo (.xls), versión "v.03". No consta autor, organización emisora ni fecha; la hoja "Información" deja campos en blanco para Empresa, Dirección, Desarrollado por, Fecha de Aprobación, Versión y Fecha de próxima revisión.
- **Carácter**: herramienta de trabajo (informativa, no normativa) usada como ejercicio o proyecto de implementación. La columna BU16 de la hoja "Tabla de Riesgos" se titula "Controles Recomendados (Ejemplo con ISO/IEC 42001:2023)", lo que fija la norma de referencia.
- **Origen aparente**: el título de la hoja "Dashboard" es "Dashboard de Evaluación de Riesgos **SGPD**" (Sistema de Gestión de Protección de Datos) y varios ejemplos citan a la "ANPD" (Autoridad Nacional de Protección de Datos), lo que indica que la plantilla fue adaptada desde una herramienta de riesgos de protección de datos personales de contexto latinoamericano (probablemente Perú, coherente con las otras plantillas del corpus).
- **Público**: responsables de la implementación del SGIA, analistas de riesgo, propietarios de activos de IA y auditores que verifican la cláusula 6.1.2.
- **Licencia**: no consta.

## 3. Estructura del documento

| Hoja | Contenido | Uso |
|---|---|---|
| Información | Portada: Empresa, Dirección, Desarrollado por, Fecha de Aprobación, Versión, Fecha de próxima revisión | Control documental de la evaluación (cláusula 7.5) |
| Activos | Inventario de activos de IA (N°, nombre, descripción, categoría, ubicación) y listas de referencia (tipo primario/secundario, categorías) | Alcance de la evaluación y base de 8.2 |
| Tabla de Riesgos | Hoja central: escalas de sensibilidad, impacto, vulnerabilidad, amenaza y probabilidad; tabla de riesgos por activo; umbrales; criterios de aceptación; catálogo de fuentes de riesgo por fase del ciclo de vida | Evaluación (6.1.2) y base del tratamiento (6.1.3) |
| Mapa Riesgos | Registro consolidado: identificación, valoración inherente/residual, controles y plan de acción (acciones, recursos, responsable, plazo) | Plan de tratamiento (6.1.3) |
| Dashboard | Conteos por sensibilidad, impacto, fuente de riesgo, agente, riesgo inherente y residual | Reporte a la dirección (9.1, 9.3) |

## 4. Contenido detallado

### 4.1 Hoja "Información"

Portada con seis campos en blanco (A8–A20): **Empresa**, **Dirección**, **Desarrollado por**, **Fecha de Aprobación**, **Versión** y **Fecha de Próxima / Revisión**, bajo el encabezado "EVALUACION DE RIESGO". Deja evidencia de aprobación y ciclo de revisión (información documentada, 7.5; repetición planificada de la evaluación, 8.2).

### 4.2 Hoja "Activos"

Inventario de activos del SGIA. Columnas (fila 2):

| Columna | Campo | Ejemplo CIMA-01 | Ejemplo CIMA-02 |
|---|---|---|---|
| B | N° Activo | CIMA-01 | CIMA-02 |
| C | Nombre del Activo | IA de Perfilamiento y Marketing Predictivo (Targeted) | IA Centralizada de Big Data Analítica (Ubicación GPS) |
| D | Descripción del Activo | Sistema algorítmico con modelos predictivos de aprendizaje automático (Machine Learning) que analiza de forma automatizada el comportamiento histórico, hábitos de consumo, tráfico de llamadas/datos y preferencias de los abonados | Plataforma de procesamiento masivo (Data Warehouse / Data Lakehouse) con capacidades de IA para correlacionar y explotar flujos masivos de geolocalización en tiempo real (coordenadas GPS y triangulación de celdas) de los móviles conectados a la red |
| E | Categoría del Activo | Algoritmo/ Modelo | Infraestructura tecnológica |
| F | Ubicación del activo | Servidor de información | Nube pública corporativa |

El prefijo "CIMA" no se explica en el archivo. Las columnas I–J contienen la **lista de referencia** de tipos de activo: *Primario* (Información, Proceso, Algoritmo/Modelo, Datos) y *Secundario* (Software y Aplicaciones, Hardware/Infraestructura técnica, Personal, Infraestructura física, Infraestructura tecnológica), división que sigue la lógica de activos primarios y de soporte de ISO/IEC 27005. Ambos activos del caso (telco) procesan datos personales de abonados a gran escala.

### 4.3 Hoja "Tabla de Riesgos"

Es la hoja de trabajo principal (44 filas, 80 columnas, 107 rangos combinados). Se divide en bandas señaladas en la fila 1: *Sensibilidad → Impacto → Vulnerabilidades → Amenazas → Riesgos iniciales → Controles → Umbrales*. La fila 14 indica que las descripciones y métricas están agrupadas con controles de esquema (+/−) para mostrar u ocultar.

#### 4.3.1 Escala de Sensibilidad de los activos (columnas F–N, filas 8–12)

Escala de **1 a 5** aplicada a **siete dimensiones** de IA responsable. Niveles genéricos (columnas F–G):

| Valor | Nivel | Descripción de la brecha |
|---|---|---|
| 1 | Muy Bajo | Poca o nula pérdida o daño |
| 2 | Bajo | Pérdida o daño menor |
| 3 | Medio | Pérdida o daño serio; procesos afectados negativamente |
| 4 | Alto | Pérdida o daño serio; procesos pueden fallar o interrumpirse |
| 5 | Muy Alto | Altas pérdidas; los procesos del negocio fallarán |

Dimensiones (fila 7) y significado de los extremos 1 y 5 (paráfrasis de las descripciones por nivel):

| Dimensión (abrev. fila 17) | Nivel 1 | Nivel 5 |
|---|---|---|
| Ética e Inclusión (E) | Datos puramente técnicos; sin posibilidad de perpetuar sesgos | Define acceso a libertades civiles o recursos vitales; discriminación sistémica |
| Transparencia y Explicabilidad (T) | Arquitectura abierta (reglas, árboles simples) auditable de inmediato | Modelos fundacionales / LLM con comportamientos emergentes; indefensión jurídica |
| Responsabilidad / Accountability (R) | Fallas resueltas con reinicio; sin asignación compleja de responsabilidades | Sistemas autónomos cuyas acciones pueden costar vidas; vacío de responsabilidad |
| Seguridad y Robustez (SR) | Inmune a ataques adversarios comunes | Misión crítica; una perturbación mínima (un píxel) causa daño catastrófico |
| Privacidad (P) | Solo datos sintéticos, públicos o desvinculados de personas | Procesamiento masivo de datos sensibles; alta probabilidad de reidentificación |
| Beneficio Social (B) | Impacto limitado a un proceso administrativo interno | Puede socavar procesos democráticos o el tejido socioeconómico |
| Supervisión Humana (S) | Control humano total: aprobación manual por ciclo (human-in-the-loop) | Autonomía total sin mecanismo viable de anulación humana |

Niveles intermedios relevantes: Transparencia 3 = modelos que requieren explicabilidad post-hoc (SHAP, LIME); Supervisión 3 = *human-on-the-loop* y 4 = *human-out-of-the-loop* con solo auditoría posterior; Seguridad 4 = propenso a *prompt injection* y envenenamiento de datos; Privacidad 4 = riesgo de exfiltración por inversión de modelo.

En la tabla de riesgos (fila 17), **Total1 (col. O) = suma de las siete dimensiones** (7–35), **Valor1 (P)** es el nivel textual y **Valor2 (Q)** un nivel 1–3; la fórmula del riesgo usa Total1 como factor "Sensibilidad" (ver 4.3.6).

#### 4.3.2 Evaluación de impactos al negocio (columnas S–Y, filas 8–13)

Escala de **1 a 3** (Bajo, Medio, Alto) en **cinco categorías**, cada una con descripción y ejemplo:

| Categoría | 1 Bajo | 2 Medio | 3 Alto |
|---|---|---|---|
| Legal / Sancionador | Infracciones administrativas menores (ej.: amonestación de la ANPD por no actualizar el registro de un modelo) | Fiscalización formal, multas moderadas (ej.: sanción por no tener Política de Gobernanza de IA) | Multas máximas, suspensión u orden de destrucción del modelo (ej.: red neuronal de scoring crediticio discriminatoria) |
| Reputacional | Quejas aisladas sin eco mediático | Cobertura negativa local | Crisis internacional, caída en bolsa, boicot (ej.: reclutamiento que descartaba por origen étnico) |
| Civil (Demandas) | Reclamos individuales (ej.: reembolso de 50 USD por error de tarifa) | Litigios individuales o arbitrajes | Demandas colectivas multimillonarias (ej.: filtración de historiales médicos por inversión de modelo en un LLM) |
| Operativo | Interrupción menor de sistemas no esenciales | Degradación de servicios clave por horas | Paralización total (ej.: ransomware sobre servidores de ML de una telco) |
| Impacto en la Sociedad | Inconvenientes temporales a un grupo reducido | Afectación moderada de un sector | Daños masivos e irreversibles (ej.: deepfakes electorales) |

**Total2 (Z) = suma de las cinco categorías** (5–15), **Valor3 (AA)** nivel textual y **Valor4 (AB)** valor 1–3 usado como factor "Impacto". En los dos ejemplos Total2 = 9 da "Alto" y Valor4 = 3, lo que sugiere que Valor4 toma el máximo de las categorías; al ser .xls, la fórmula no pudo verificarse.

#### 4.3.3 Análisis de vulnerabilidades (columnas AE–AJ, filas 8–13)

Dos métricas de **1 a 3**:

- **Severidad** (facilidad de explotar la falla): 1 = requiere inversión masiva, supercómputo o un zero-day; 2 = requiere conocimientos sólidos de ciencia de datos y ciberseguridad con herramientas estándar (scripts de GitHub, Adversarial Robustness Toolbox); 3 = explotable a costo cero con lenguaje natural, herramientas gratuitas o simple error humano.
- **Exposición** (alcance del daño): 1 = aislada, imperceptible; 2 = segmentada, degradación parcial reversible; 3 = masiva, destruye la validez del modelo o causa daño irreversible a terceros (discriminación masiva, robo de identidad).

Columnas: **Fuente de Riesgos (AF)**, **Código (AG)** del catálogo FR, **Severidad (AH)**, **Exposición (AI)** y **Valor5 (AJ)**. Por los valores observados (3+2 → 4; 2+2 → 3; vacío → −1), **Valor5 = Severidad + Exposición − 1** (1–5).

#### 4.3.4 Análisis de amenazas (columnas AL–AR, filas 8–13)

Dos métricas de **1 a 3** referidas al **agente** de amenaza:

- **Capacidad**: 1 = usuario común con herramientas públicas (ej.: engaña a un chatbot con plantillas de foros); 2 = desarrollador o estudiante con GPU de consumo (ej.: extrae datos de una API desprotegida); 3 = equipo especializado con clústeres en la nube (ej.: cibercrimen o espionaje industrial).
- **Motivación**: 1 = curiosidad o validación académica (investigador ético); 2 = ventaja comercial o venganza laboral menor (empleado descontento); 3 = extorsión, robo de propiedad intelectual o desestabilización geopolítica (competidor agresivo, sindicato criminal).

Columnas: **Agente (AM)**, **Int/Ext (AN)**, **Evento de amenaza (AO)**, **Capacidad (AP)**, **Motivación (AQ)** y **Valor6 (AR)**, con **Valor6 = Capacidad + Motivación − 1** (1–5).

#### 4.3.5 Probabilidad de ocurrencia (columnas AT–AV)

Escala **1–3**: 1 = Baja, sin historial, rara; 2 = Media, se han presentado casos; 3 = Alta, suficientes casos y la amenaza seguramente ocurrirá. Se ingresa directamente en la columna AW.

#### 4.3.6 Cálculo del riesgo, umbrales y aceptación

Fórmula declarada en AT12/AU16: **Riesgo Total = Amenaza × Vulnerabilidad × Probabilidad × Sensibilidad × Impacto**, donde Amenaza = Valor6, Vulnerabilidad = Valor5, Probabilidad = 1–3, Sensibilidad = Total1 (7–35) e Impacto = Valor4 (1–3). Máximo teórico = 5 × 5 × 3 × 35 × 3 = **3.375** (celda BY6).

**Umbrales de riesgo** (BW2–BZ5), que dividen el rango en tercios (33,3 %):

| Nivel | Límite inferior | Límite superior |
|---|---|---|
| BAJO | 1 | 1.125 |
| MEDIO | 1.126 | 2.250 |
| ALTO | 2.251 | 3.375 |

**Criterios de aceptación del riesgo** (BA8, BA10): los riesgos de nivel BAJO se **aceptan y pueden retenerse**, aunque la organización puede optar por reducir, evitar o transferir; los riesgos MEDIO o ALTO **deben** tratarse mediante **reducir, evitar o transferir**. La columna BA muestra el nivel y las columnas BB–BD marcan visualmente el nivel (el carácter "Ä" es un símbolo de fuente tipo Wingdings). Las filas vacías muestran "ERROR".

**Riesgo con control / residual** (columnas BF–BT): se describe el **control existente (BF)**, se reevalúa **Severidad (BG)** y **Exposición (BH)**, se obtiene un nuevo **Valor (BI)** y se recalcula **Riesgo Residual (BP)** con la misma fórmula, manteniendo amenaza, probabilidad, sensibilidad e impacto. El nivel residual (BQ) usa los mismos umbrales. La columna **BU "Controles Recomendados (Ejemplo con ISO/IEC 42001:2023)"** está prevista para vincular controles del Anexo A, pero **en el archivo está vacía**: no aparece ningún control A.x citado.

#### 4.3.7 Ejemplos de riesgo del caso (filas 18–19)

| Campo | Riesgo 1 | Riesgo 2 |
|---|---|---|
| Proceso / Propietario | Gestión Comercial / Gerente Comercial | Operaciones / Gerente Operaciones |
| Activo / Clasificación | IA de Perfilamiento y Marketing Predictivo / Algoritmo-Modelo / Servidor de información | IA de Big Data Analítica (GPS) / Infraestructura tecnológica / Nube pública |
| Sensibilidad E-T-R-SR-P-B-S | 4-4-4-3-4-2-4 → Total1 25, "Medio", Valor2 3 | 2-3-4-4-5-3-3 → Total1 24, "Alto", Valor2 3 |
| Descripción del impacto | Diseñado para operar sin human-in-the-loop, alterando tarifas directamente | Microdegradaciones térmicas en procesadores cloud producen bit-flips que corrompen silenciosamente históricos de geolocalización |
| Impacto L-R-C-O-S | 2-2-1-3-1 → Total2 9, Alto, Valor4 3 | 1-2-2-3-1 → Total2 9, Alto, Valor4 3 |
| Fuente de riesgo / código | Nivel de automatización / FR1-1 | Problemas con el hardware del sistema (Fallas) / FR5-1 |
| Vulnerabilidad Sev-Exp → Valor5 | 3-2 → 4 | 2-2 → 3 |
| Agente / Int-Ext / Evento | Algoritmo autónomo descontrolado / Interno / Modificación errónea de tarifas fijas a miles de usuarios sin veto humano | Ola de calor / Externo / Bit-flips en memoria en tiempo de ejecución |
| Capacidad-Motivación → Valor6 | 1-1 → 1 | 1-1 → 1 |
| Probabilidad | 2 | 2 |
| Riesgo Total | 1 × 4 × 2 × 25 × 3 = **600 → BAJO** | 1 × 3 × 2 × 24 × 3 = **432 → BAJO** |
| Control existente / residual | "No se evidencia" / 600 BAJO | "No se evidencia" / 432 BAJO |

#### 4.3.8 Catálogo de fuentes de riesgo por fase del ciclo de vida (AF33–AI44)

| Fase del ciclo de vida | Código | Fuente de riesgo | Foco de control para el analista |
|---|---|---|---|
| Fase 1: Planificación y diseño (definición del objetivo, evaluación de riesgos éticos y de privacidad) | FR1-1 | Complejidad del entorno | Incertidumbre del rendimiento en entornos amplios o no controlados (ej.: redes públicas de telecomunicaciones) |
| | FR1-2 | Nivel de automatización | Riesgos del grado de autonomía delegado: seguridad física, equidad, seguridad corporativa |
| | FR1-3 | Preparación tecnológica | Tecnologías inmaduras (límites desconocidos, deriva) o muy maduras (complacencia del operador) |
| Fase 2: Preparación y tratamiento de datos (recopilación, limpieza) | FR2-1 | Fuentes de riesgo del aprendizaje automático (Datos) | Calidad de datos, fallas de recolección y etiquetado |
| | FR2-2 | Diversidad de datos | Conjuntos heterogéneos que exponen datos confidenciales o retrasan el procesamiento |
| Fase 3: Desarrollo y entrenamiento del modelo | FR3-1 | Falta de transparencia y explicabilidad | Incapacidad de dar información clara y auditable a las partes interesadas |
| | FR3-2 | Fuentes de riesgo del aprendizaje automático (Robustez) | Perturbaciones, sobreajuste, envenenamiento de datos en entrenamiento |
| Fase 4: Despliegue y comercialización | FR4-1 | Problemas del ciclo de vida (Implementación) | Defectos heredados del diseño o implementación inadecuada en producción |
| | FR4-2 | Problemas con el hardware (Transferencia) | Incompatibilidades o pérdida de precisión al migrar modelos entre arquitecturas o nubes |
| Fase 5: Explotación, uso y supervisión (monitoreo, reentrenamiento) | FR5-1 | Problemas con el hardware del sistema (Fallas) | Errores de hardware en ejecución por componentes defectuosos o degradación |
| | FR5-2 | Problemas del ciclo de vida (Mantenimiento y Retiro) | Falta de monitoreo de deriva (model drift) y vacíos de seguridad en la retirada del servicio |

Este catálogo reproduce, adaptado, las fuentes de riesgo del Anexo B de ISO/IEC 23894 (complejidad del entorno, nivel de automatización, preparación tecnológica, falta de transparencia y explicabilidad, riesgos del aprendizaje automático, problemas de hardware y del ciclo de vida del sistema) y las fases del ciclo de vida del Anexo C de la misma norma.

### 4.4 Hoja "Mapa Riesgos"

Registro consolidado de 10 filas (A4–A13), organizado en tres bloques: **Identificación del riesgo** (N°, Activo, Riesgo, Causas, Consecuencia), **Valoración del riesgo** (Evaluación, Nivel de riesgo inherente, Controles existentes, Eficacia del control SI/NO, Grado de exposición residual, Nivel de riesgo residual) y **Plan de Acción** (Controles recomendados, Acciones, Recursos, Responsable, Plazo). Las filas se alimentan de "Tabla de Riesgos": riesgo 1 = "Modificación errónea de tarifas fijas a miles de usuarios sin veto humano" (causa: nivel de automatización; inherente 600; control "No se evidencia"; eficacia "SI"; residual 600); riesgo 2 = "Bit-flips en memoria…" (causa: fallas de hardware; 432/432). La fila 14 totaliza 2 riesgos. Las columnas de plan de acción (acciones, recursos, responsable, plazo) están vacías en el ejemplo. Esta hoja es el **plan de tratamiento de riesgos** de la cláusula 6.1.3.

### 4.5 Hoja "Dashboard"

Título "Dashboard de Evaluación de Riesgos SGPD". Seis tablas de conteo que alimentan gráficos:

- **Valor Sensibilidad**: Bajo 9 (filas vacías), Medio 0, Alto 2.
- **Impacto al Negocio** por categoría y valor 1/2/3: Legal 1/1/0; Reputacional 0/2/0; Civil 1/1/0; Operativo 0/0/2. No se incluye la categoría "Impacto en la Sociedad".
- **Fuente de Riesgo**: conteo por código FR1-1 … FR5-2 (FR1-1 = 1, FR5-1 = 1).
- **Agente**: Interno 1, Externo 1.
- **Riesgo Inherente** y **Riesgo Residual**: BAJO 2, MEDIO 0, ALTO 0.

## 5. Conceptos y términos clave

| Término (ES) | Término (EN) | Definición breve | Dónde aparece |
|---|---|---|---|
| Activo de IA primario / secundario | Primary / supporting asset | Primario: información, proceso, algoritmo/modelo, datos; secundario: software, hardware, personal, infraestructura | Activos I3–J11 |
| Sensibilidad del activo | Asset sensitivity | Valoración 1–5 en siete dimensiones de IA responsable | Tabla de Riesgos F–N |
| Supervisión humana en / sobre / fuera del bucle | Human-in / on / out-of-the-loop | Grados de control humano sobre la IA (aprobación por ciclo, supervisión general, solo auditoría posterior) | Tabla de Riesgos N8–N12 |
| Impacto al negocio | Business impact | Consecuencia 1–3 en legal, reputacional, civil, operativo y social | Tabla de Riesgos S–Y |
| Severidad / Exposición | Severity / Exposure | Facilidad de explotación y alcance del daño de una vulnerabilidad | AE8–AG13 |
| Capacidad / Motivación | Capability / Motivation | Atributos del agente de amenaza | AL8–AN13 |
| Fuente de riesgo (FR) | Risk source | Origen del riesgo según fase del ciclo de vida de la IA | AF33–AI44 |
| Riesgo inherente / residual | Inherent / residual risk | Riesgo antes y después de considerar controles | Mapa Riesgos G, K |
| Umbral de riesgo | Risk threshold | Límites 1.125 / 2.250 / 3.375 que definen BAJO, MEDIO, ALTO | BW2–BZ5 |
| Retener, reducir, evitar, transferir | Retain, reduce, avoid, transfer (share) | Opciones de tratamiento del riesgo | BA8, BA10 |
| Inversión de modelo, envenenamiento, prompt injection | Model inversion, data poisoning, prompt injection | Ataques adversarios propios de IA citados en las escalas | K11, L11, W13 |
| Deriva del modelo | Model drift | Pérdida de rendimiento con el tiempo, fuente FR5-2 | AI44 |

## 6. Relación con otros documentos del corpus

- **ISO/IEC 42001, 6.1.2 (evaluación de riesgos de IA)**: la norma exige definir y aplicar un proceso que establezca criterios de riesgo, identifique riesgos, los analice (consecuencias potenciales y probabilidad realista) y los evalúe frente a los criterios. La plantilla operacionaliza cada paso: criterios = umbrales BW2–BZ5 y reglas de aceptación BA8/BA10; identificación = columnas AF–AO (fuente, evento, agente); análisis = escalas de sensibilidad, impacto, vulnerabilidad, amenaza y probabilidad; evaluación = columna BA (nivel).
- **6.1.3 (tratamiento de riesgos de IA)**: la selección de opciones de tratamiento y controles se refleja en las columnas BF–BU y en el bloque "Plan de Acción" de "Mapa Riesgos"; la columna BU está pensada para citar controles del Anexo A y así enlazar con la Declaración de Aplicabilidad.
- **6.1.4 (evaluación de impacto del sistema de IA)**: las dimensiones "Impacto en la Sociedad", "Ética e Inclusión" y "Beneficio Social" anticipan la evaluación de impacto sobre individuos y sociedad, aunque la plantilla no la separa como documento propio.
- **8.2 (evaluación de riesgos de IA en operación)**: la plantilla es el registro que se ejecuta a intervalos planificados o ante cambios; la hoja "Información" guarda la fecha de próxima revisión.
- **ISO/IEC 23894 (gestión del riesgo de IA)**: el catálogo FR1-1 a FR5-2 y su organización por fases reproducen la estructura de fuentes de riesgo (Anexo B) y ciclo de vida (Anexo C) de esa guía; las siete dimensiones de sensibilidad coinciden con los objetivos relacionados con IA (equidad, transparencia, responsabilidad, seguridad, privacidad, robustez, supervisión).
- **Plantillas hermanas**: el archivo 11 (FODA vs Riesgo) cubre los riesgos del **contexto** (6.1.1) con una matriz probabilidad × impacto 3 × 3, mientras que esta plantilla cubre los riesgos **por activo de IA** (6.1.2); el archivo 12 (Stakeholders) aporta las partes interesadas cuyas expectativas explican los impactos legales y reputacionales.

## 7. Aplicación práctica y puntos de examen

Secuencia de uso: (1) completar la portada; (2) inventariar activos de IA, clasificarlos como primarios o secundarios y ubicarlos; (3) para cada activo, valorar las siete dimensiones de sensibilidad (1–5); (4) describir el impacto y puntuar las cinco categorías (1–3); (5) elegir la fuente de riesgo del catálogo FR y puntuar severidad y exposición; (6) describir agente, origen interno/externo y evento de amenaza, y puntuar capacidad y motivación; (7) asignar probabilidad; (8) leer el riesgo total y su nivel; (9) documentar controles existentes y recalcular el residual; (10) trasladar a "Mapa Riesgos" las acciones, recursos, responsables y plazos; (11) presentar el "Dashboard" en la revisión por la dirección.

Puntos "debes saber":

1. La fórmula del riesgo es multiplicativa de cinco factores y su máximo es 3.375; los tres niveles se obtienen dividiendo ese rango en tercios.
2. La **sensibilidad** se mide en siete dimensiones de IA responsable, no solo en confidencialidad-integridad-disponibilidad como en seguridad de la información.
3. **Impacto** y **probabilidad** usan escalas 1–3; sensibilidad usa 1–5.
4. Vulnerabilidad = severidad + exposición − 1; amenaza = capacidad + motivación − 1.
5. Los riesgos BAJOS pueden **retenerse**; los MEDIOS y ALTOS **deben** reducirse, evitarse o transferirse.
6. El riesgo residual solo cambia si el control modifica la severidad o la exposición de la vulnerabilidad.
7. Las fuentes de riesgo se codifican por fase del ciclo de vida (FR1 diseño … FR5 operación y retiro), alineadas con ISO/IEC 23894.
8. Distinguir human-in-the-loop, human-on-the-loop y human-out-of-the-loop.
9. Un "algoritmo autónomo descontrolado" puede ser un **agente de amenaza interno** y una "ola de calor" un agente externo: las amenazas a la IA no son solo humanas ni maliciosas.
10. La evaluación debe repetirse a intervalos planificados (campo "Fecha de próxima revisión").
11. Error frecuente: dejar vacía la columna de controles recomendados del Anexo A, con lo que la evaluación no conecta con la Declaración de Aplicabilidad.
12. Error frecuente: registrar "No se evidencia" como control existente y marcar la eficacia como "SI", como ocurre en el ejemplo.

Preguntas típicas de auditoría: ¿existe un inventario de sistemas de IA en el alcance? ¿Los criterios de aceptación están aprobados por la dirección? ¿Se identifican propietarios de riesgo? ¿El plan de tratamiento tiene responsables y plazos? ¿Se vinculan los controles con el Anexo A?

## 8. Limitaciones del análisis

- El archivo es .xls binario; el volcado no incluye fórmulas. Las reglas de cálculo (Valor5, Valor6, Riesgo Total) se dedujeron de los valores y son consistentes con los dos ejemplos y con las filas vacías, pero las reglas que convierten Total1 en nivel textual (Valor1) y Total2 en Valor4 no pudieron confirmarse.
- Inconsistencias internas de la plantilla: Total1 = 25 se etiqueta "Medio" y Total1 = 24 "Alto" (posible error de fórmula); el ejemplo 1 usa código FR1-1 ("Complejidad del entorno") para la fuente "Nivel de automatización", que en el catálogo es FR1-2; el Dashboard cuenta 2 sensibilidades "Alto" aunque la tabla muestra "Medio" y "Alto"; el Dashboard omite la categoría "Impacto en la Sociedad"; el título del Dashboard dice "SGPD" en lugar de "SGIA".
- Erratas del original corregidas en este resumen: "Sansionador" → Sancionador, "Descrpción", "Cóidigo", "ocurrrir", "debeaplicar", "ANPD/ANPDP".
- El carácter "Ä" de las columnas BB/BR es un símbolo de fuente decorativa cuyo glifo real no se puede reproducir en texto.
- Los gráficos del Dashboard no se extrajeron; solo se describen sus tablas fuente.
- No consta autor, fecha ni licencia.

## 9. Referencias

- ISO/IEC 42001:2023, *Information technology — Artificial intelligence — Management system* (cláusulas 6.1.2, 6.1.3, 6.1.4, 8.2; Anexo A), citada en la columna BU16 de la hoja "Tabla de Riesgos".
- ISO/IEC 23894:2023, *Artificial intelligence — Guidance on risk management* (fuentes de riesgo y ciclo de vida), referencia implícita del catálogo FR.
- ISO/IEC 27005 (tipología de activos primarios y de soporte), referencia implícita de la hoja "Activos".
- Ley de protección de datos personales y autoridad ANPD (contexto peruano), citadas en los ejemplos de impacto legal.
- Herramientas citadas en las escalas: SHAP, LIME, Adversarial Robustness Toolbox.
