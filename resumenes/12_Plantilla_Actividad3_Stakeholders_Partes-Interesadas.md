---
titulo: "Plantilla Excel: Actividad 3 – Stakeholders. Identificación de partes interesadas, matriz poder/interés y ficha de requerimientos por stakeholder"
archivo_fuente: "Plantillas para ejercicios y proyectos/Actividad3 STAKEHOLDERS.xlsx"
tipo_documento: plantilla_excel
organismo_emisor: "Material de curso (autor no identificado en el archivo; ejercicio de implementación ISO/IEC 42001)"
fecha_publicacion_fuente: "no consta"
idioma_fuente: es
paginas_o_hojas: "2 hojas: STAKEHOLDERS, 01STAKEHOLDER"
version_resumen: "1.0"
fecha_resumen: "2026-09-17"
generado_por: "Claude (agente Analista de Plantillas – Análisis de Riesgo SGIA, FODA vs Riesgo y Stakeholders)"
palabras_clave: [partes interesadas, stakeholders, grupos de interés internos y externos, poder, interés, influencia, matriz poder-interés, mantener satisfecho, principal stakeholder, mínimo esfuerzo, mantener informados, requerimientos, satisfacción, ANPDP, OEFA, ISO/IEC 42001 4.2, 7.4]
documentos_relacionados_en_corpus: ["00_INDICE.md", "01_ISO-IEC-42001-2023_Requisitos_Clausulas-4-10.md", "02_ISO-IEC-42001-2023_Anexo-A_Controles_y_Anexo-B_Guia-Implementacion.md", "06_Guia-Implementacion-Inicial-ISO42001_Impulsa360.md", "10_Plantilla_Analisis-de-Riesgo-SGIA.md", "11_Plantilla_Actividad2_FODA-vs-Riesgo_Contexto-Organizacion.md"]
---

## 1. Resumen ejecutivo

"Actividad3 STAKEHOLDERS.xlsx" es la plantilla del curso destinada a cumplir la cláusula **4.2 de ISO/IEC 42001 (comprensión de las necesidades y expectativas de las partes interesadas)**. Se compone de dos hojas. La hoja **"STAKEHOLDERS"** identifica los **grupos interesados internos y externos**, les asigna una puntuación de **poder** y de **interés/influencia** en una escala de **1 (relación débil) a 5 (relación fuerte)**, y los ubica en una **matriz poder/interés** de cuatro cuadrantes: **Mantener Satisfecho** (mucho poder, bajo interés), **Principal Stakeholder** (mucho poder, alto interés), **Mínimo Esfuerzo** (poco poder, bajo interés) y **Mantener Informados** (poco poder, alto interés). A cada cuadrante se asocia una **estrategia**: los dos cuadrantes de mucho poder exigen "establecer acciones a sus requerimientos", los dos de poco poder "no requieren acciones obligatoriamente". Dos tablas gráficas incrustadas definen los **criterios de poder** y los **criterios de influencia** en cinco niveles con ejemplos (proveedores de eventos, servicios básicos, empleados, bancos/IT, aseguradoras/SBS). La hoja **"01STAKEHOLDER"** es una ficha individual titulada "Descubriendo oportunidades con los stakeholders": lista los **requerimientos** de una parte interesada, los prioriza, califica la **satisfacción actual y objetivo** (1 totalmente insatisfecho – 5 totalmente satisfecho) y propone cómo **innovar la relación**. El ejemplo es peruano: **Colaboradores**, la **ANPDP** (Autoridad Nacional de Protección de Datos Personales, que emite normas y sanciones), el **Ministerio del Ambiente** y el **OEFA** (Organismo de Evaluación y Fiscalización Ambiental). La plantilla es relevante porque los requisitos de las partes interesadas alimentan la política de IA, los objetivos, la evaluación de impacto (6.1.4) y la comunicación (7.4), y porque un auditor de 42001 verifica que las expectativas relevantes se hayan determinado y traducido en requisitos del SGIA.

## 2. Identificación y contexto del documento

- **Tipo**: libro Excel (.xlsx) con dos hojas, dos imágenes PNG incrustadas (tablas de criterios) y validaciones de datos activas. No contiene fórmulas.
- **Autoría y fecha**: no constan. Es la "Actividad 3" de la serie del curso, posterior a la Actividad 2 (FODA vs Riesgo).
- **Origen**: contexto peruano, evidenciado por las entidades del ejemplo (ANPDP, Ministerio del Ambiente, OEFA) y por la mención de la SBS (Superintendencia de Banca, Seguros y AFP) en los criterios de poder. El énfasis en "PDP" (protección de datos personales) indica, como en las otras plantillas, reutilización de una herramienta de sistema de gestión de protección de datos.
- **Carácter**: informativo, herramienta de taller.
- **Público**: equipo implementador del SGIA, dirección y responsables de comunicación y cumplimiento.
- **Licencia**: no consta.

## 3. Estructura del documento

| Hoja | Bloques | Propósito |
|---|---|---|
| STAKEHOLDERS | (a) Escala 1–5 de relación débil a fuerte; (b) tabla de grupos internos y externos con poder e interés/influencia; (c) matriz poder/interés de cuatro cuadrantes con stakeholders posicionados; (d) estrategia por cuadrante; (e) imágenes "Criterios de Poder" y "Criterios de Influencia" | Identificar y priorizar partes interesadas (4.2) |
| 01STAKEHOLDER | Regla de calificación 1–5; tabla de requerimientos, priorización, calificación actual/objetivo e ideas de innovación de la relación | Detallar necesidades y expectativas de un stakeholder concreto y planificar la mejora de la relación |

## 4. Contenido detallado

### 4.1 Hoja "STAKEHOLDERS"

Título (B3): "IDENTIFICACIÓN DE LOS PRINCIPALES STAKEHOLDERS".

#### 4.1.1 Escala de valoración (E5–I6)

Escala numérica **1, 2, 3, 4, 5** con leyenda "Relación Débil" bajo el 1 y "Relación Fuerte" bajo el 5. Las celdas de poder e interés (E13:F22 y K13:L22) tienen validación de lista que solo admite estos cinco valores.

#### 4.1.2 Tabla de identificación (fila 8 en adelante)

| Grupos interesados INTERNOS (B) | Poder (E) | Interés/Influencia (F) | Grupos interesados EXTERNOS (H) | Poder (K) | Interés/Influencia (L) |
|---|---|---|---|---|---|
| COLABORADORES | 3 | 3 | ANPDP (emite normas y sanciones) | 5 | 3 |
| | | | Ministerio del Ambiente (emite normas) | 2 | 5 |

Hay espacio para diez grupos internos y diez externos (filas 13–22). El ejemplo deja la descripción del rol del stakeholder entre paréntesis ("emite normas y sanciones"), buena práctica para justificar la puntuación de poder.

#### 4.1.3 Criterios de Poder (imagen junto a O2)

Tabla de cinco niveles (transcripción parafraseada de la imagen):

| Nivel | Descripción | Ejemplos |
|---|---|---|
| 1 – Muy bajo | Proveedores ocasionales sin impacto en la continuidad | Catering, eventos |
| 2 – Bajo | Proveedores de servicios básicos regulados, con bajo margen de negociación | Agua, luz, internet |
| 3 – Medio | Impacta en soporte interno, pero no en procesos core | Empleados, mantenimiento |
| 4 – Alto | Influye en la eficiencia operativa y el cumplimiento normativo | Bancos, IT, consultores regulatorios |
| 5 – Muy alto | Puede detener o transformar procesos core del negocio | Aseguradoras, SBS |

El poder mide, por tanto, la **capacidad del stakeholder de afectar los procesos esenciales** de la organización, desde nula hasta la de un regulador que puede detenerlos.

#### 4.1.4 Criterios de Influencia (imagen junto a O15)

| Nivel | Descripción | Ejemplos |
|---|---|---|
| 1 – Muy bajo | Busca resultados directos y constantes, con alta expectativa de desempeño | Clientes corporativos, aseguradoras |
| 2 – Bajo | Requiere cumplimiento regular y seguimiento | Bancos, IT |
| 3 – Medio | Necesita soporte confiable, pero no exige supervisión constante | Empleados, proveedores de capacitación |
| 4 – Alto | Relación estable y rutinaria, sin exigencias adicionales | Servicios básicos |
| 5 – Muy alto | Relación ocasional, sin expectativas críticas | Proveedores de eventos |

Obsérvese que esta tabla está **invertida** respecto a la lógica de la matriz: el nivel 1 describe la relación más exigente y el nivel 5 la más ocasional, cuando en la matriz un interés 5 lleva al stakeholder a "Principal" o "Mantener informados". Es probablemente un error de la plantilla (ver sección 8); el estudiante debe leerla como "nivel de exigencia/expectativa" y, si la usa tal cual, invertir la escala.

#### 4.1.5 Matriz poder/interés (D24–H34)

Ejes: **INTERÉS** en horizontal (BAJA = 1–2; ALTA = 3–4–5) y **PODER** en vertical (MUCHO = 3–4–5; POCO = 1–2).

| | Interés BAJA (1–2) | Interés ALTA (3–4–5) |
|---|---|---|
| **Poder MUCHO (3–4–5)** | **Mantener Satisfecho** | **Principal Stakeholders** — ejemplo: Colaboradores, OEFA |
| **Poder POCO (1–2)** | **Mínimo Esfuerzo** | **Mantener Informados** — ejemplo: Ministerio del Ambiente |

Aplicando los umbrales a la tabla de identificación: Colaboradores (3, 3) → Principal Stakeholder, coherente con la matriz; Ministerio del Ambiente (2, 5) → Mantener Informados, coherente; ANPDP (5, 3) debería ser Principal Stakeholder pero no está posicionada en la matriz del ejemplo; OEFA aparece en la matriz sin estar en la tabla de identificación.

#### 4.1.6 Estrategia por cuadrante (D42–F45)

| Cuadrante | Estrategia |
|---|---|
| Mantener Satisfecho | Establecer acciones a sus requerimientos |
| Principal Stakeholders | Establecer acciones a sus requerimientos |
| Mínimo Esfuerzo | No requiere acciones obligatoriamente |
| Mantener Informado | No requiere acciones obligatoriamente |

La regla resultante es que el **poder** determina la obligación de actuar: los stakeholders con poder 3 a 5 generan requisitos que la organización debe atender con acciones; los de poder 1 a 2 se gestionan con comunicación o esfuerzo mínimo. Esta simplificación es útil para decidir qué expectativas se convierten en **requisitos** del SGIA (4.2 exige determinar cuáles de esos requisitos se abordarán a través del sistema de gestión).

### 4.2 Hoja "01STAKEHOLDER"

Título (A3): "DESCUBRIENDO OPORTUNIDADES CON LOS STAKEHOLDERS". El nombre "01" indica que se crea una copia por stakeholder principal (02STAKEHOLDER, 03…).

#### 4.2.1 Regla de calificación (J6–K11)

| Valor | Significado |
|---|---|
| 1 | Totalmente insatisfecho |
| 2 | Algo insatisfecho |
| 3 | Indiferente |
| 4 | Algo satisfecho |
| 5 | Totalmente satisfecho |

Las celdas de calificación (G17:H26) validan contra esta lista.

#### 4.2.2 Tabla de requerimientos (fila 15 en adelante)

Propósito (A13): "Determinar los principales requerimientos de los stakeholders y cómo la empresa puede satisfacerlos". Columnas:

| Col. | Campo |
|---|---|
| A | ITEM |
| B | ¿Cuáles son los principales requerimientos (necesidades) de los stakeholders para con la empresa? |
| F | PRIORIZACIÓN: 1 Alto, 2 Medio, 3 Bajo |
| G | CALIFICACIÓN ACTUAL (1–5) |
| H | CALIFICACIÓN OBJETIVO (1–5) |
| I | ¿Cómo se podría innovar la relación con el stakeholder de modo que se sienta realmente encantado y satisfecho? |

**Ejemplo (fila 17)**: ítem 1, requerimiento "ANPDP: cumplimiento de los requisitos legales en materia de PDP", priorización ALTA, calificación actual **3** (indiferente), objetivo **5** (totalmente satisfecho), innovación: (a) tener un **procedimiento para la identificación de requisitos legales (RL)**; (b) establecer una **matriz de requisitos legales** para su seguimiento; (c) realizar **auditorías legales al menos una vez al año**.

Las filas 19–25 quedan disponibles (ítems 2, 3, 3 y 4, con un duplicado de numeración en el original). La brecha entre calificación actual y objetivo funciona como indicador de desempeño de la relación, medible en el tiempo (cláusula 9.1).

## 5. Conceptos y términos clave

| Término (ES) | Término (EN) | Definición breve | Dónde aparece |
|---|---|---|---|
| Parte interesada / stakeholder | Interested party / stakeholder | Persona u organización que puede afectar, verse afectada o percibirse afectada por una decisión o actividad | Título B3 |
| Grupo interesado interno / externo | Internal / external stakeholder group | Dentro de la organización (colaboradores) o fuera (reguladores, ministerios) | B8, H8 |
| Poder | Power | Capacidad de detener o transformar procesos core (1–5) | E8, K8; imagen Criterios de Poder |
| Interés / Influencia | Interest / Influence | Grado de expectativa y atención del stakeholder sobre la organización (1–5) | F8, L8; imagen Criterios de Influencia |
| Matriz poder/interés | Power/interest grid (Mendelow) | Clasificación en cuatro cuadrantes según poder e interés | D24–H34 |
| Mantener satisfecho | Keep satisfied | Mucho poder, bajo interés | F26 |
| Principal stakeholder | Key player / manage closely | Mucho poder, alto interés | H26 |
| Mínimo esfuerzo | Monitor / minimal effort | Poco poder, bajo interés | F33 |
| Mantener informados | Keep informed | Poco poder, alto interés | H33 |
| Requerimiento | Requirement / need | Necesidad o expectativa del stakeholder hacia la empresa | 01STAKEHOLDER B15 |
| Calificación actual / objetivo | Current / target rating | Nivel de satisfacción 1–5 hoy y deseado | G15, H15 |
| Requisitos legales (RL) | Legal requirements | Obligaciones normativas identificadas y seguidas en una matriz | I17 |
| ANPDP | National Personal Data Protection Authority (Peru) | Autoridad que emite normas y sanciona en protección de datos | H13 |
| OEFA | Environmental Assessment and Enforcement Agency (Peru) | Fiscalizador ambiental peruano | H28 |
| SBS | Superintendency of Banking, Insurance and Pension Funds (Peru) | Regulador financiero citado como poder nivel 5 | Imagen Criterios de Poder |

## 6. Relación con otros documentos del corpus

- **ISO/IEC 42001, 4.2 (partes interesadas)**: la norma exige determinar las partes interesadas pertinentes para el SGIA, sus requisitos pertinentes y cuáles de ellos se abordarán mediante el sistema. La hoja "STAKEHOLDERS" cubre la identificación y la priorización; la hoja "01STAKEHOLDER" cubre los requisitos concretos y su tratamiento. La regla "poder alto → establecer acciones" es el criterio de la plantilla para decidir qué requisitos se incorporan al SGIA.
- **4.1 (contexto)**: los reguladores identificados (ANPDP, Ministerio del Ambiente, OEFA) son cuestiones externas de tipo regulatorio/legal, que en la plantilla 11 (FODA vs Riesgo) se registran como oportunidades o amenazas. Ambas plantillas se usan juntas para construir el contexto.
- **4.4 (sistema de gestión de IA)**: los procesos del SGIA deben satisfacer los requisitos determinados en 4.2; la matriz de requisitos legales propuesta en la ficha es un ejemplo de proceso derivado.
- **5.2 (política de IA) y 6.2 (objetivos)**: la política debe ser coherente con los requisitos de las partes interesadas y estar disponible para ellas cuando corresponda; el objetivo "pasar de calificación 3 a 5 con la ANPDP" es un objetivo medible del SGIA.
- **6.1.4 (evaluación de impacto del sistema de IA)**: la evaluación de impacto considera a individuos, grupos y sociedad; la identificación de stakeholders es su punto de partida.
- **7.4 (comunicación)**: la norma exige determinar qué comunicar, cuándo, a quién y cómo; los cuadrantes "Mantener informados" y "Mantener satisfecho" definen directamente el público y la intensidad de la comunicación.
- **Anexo A**: el control **A.3.2 (roles y responsabilidades de IA)** y **A.3.3 (reporte de preocupaciones)** requieren canales para que las partes interesadas, internas en particular, planteen inquietudes; el objetivo de control **A.8 (información para las partes interesadas)**, con controles como A.8.2 (documentación del sistema e información para usuarios), A.8.3 (informes externos), A.8.4 (comunicación de incidentes) y A.8.5 (información para partes interesadas sobre sus obligaciones), operacionaliza la comunicación con los stakeholders que esta plantilla identifica. La plantilla no cita estos controles; la relación es funcional.
- **Plantilla 10 (Análisis de Riesgo SGIA)**: los impactos "Legal/Sancionador" y "Reputacional" de esa plantilla se explican por los stakeholders reguladores y sociales identificados aquí.

## 7. Aplicación práctica y puntos de examen

Secuencia de uso: (1) en taller, listar grupos internos (colaboradores, dirección, áreas de datos, sindicatos) y externos (reguladores, clientes, proveedores de IA, usuarios afectados, sociedad civil); (2) puntuar poder e interés/influencia con los criterios 1–5; (3) posicionar cada grupo en la matriz aplicando los umbrales (poder ≥ 3 = mucho; interés ≥ 3 = alto); (4) para cada Principal Stakeholder y cada Mantener Satisfecho crear una ficha "0nSTAKEHOLDER" con sus requerimientos, priorización, satisfacción actual y objetivo y acciones de mejora; (5) trasladar los requerimientos que se abordarán a la lista de requisitos del SGIA (4.2), a la matriz de requisitos legales y al plan de comunicación (7.4); (6) revisar periódicamente las calificaciones.

Puntos "debes saber":

1. La cláusula 4.2 pide tres cosas: **quiénes** son las partes interesadas, **qué** requisitos tienen y **cuáles** se abordarán mediante el SGIA.
2. La plantilla usa la matriz poder/interés (modelo de Mendelow) con umbrales: 1–2 = bajo/poco, 3–5 = alto/mucho.
3. Cuadrantes: mucho poder + bajo interés = **Mantener satisfecho**; mucho poder + alto interés = **Principal stakeholder**; poco poder + bajo interés = **Mínimo esfuerzo**; poco poder + alto interés = **Mantener informados**.
4. En esta plantilla, la obligación de actuar la determina el **poder**, no el interés.
5. Poder nivel 5 = puede detener procesos core (reguladores como SBS o ANPDP); nivel 1 = proveedores ocasionales.
6. La satisfacción se mide 1–5 (totalmente insatisfecho → totalmente satisfecho) y la brecha actual/objetivo es un indicador de 9.1.
7. Para un regulador de protección de datos, la acción típica es procedimiento de identificación de requisitos legales + matriz de seguimiento + auditoría legal anual.
8. Los reguladores del ejemplo son peruanos (ANPDP, Ministerio del Ambiente, OEFA, SBS); en otro país se sustituyen por la autoridad de protección de datos y los supervisores sectoriales locales.
9. En un SGIA, las partes interesadas incluyen también a los **sujetos afectados por las decisiones de la IA** aunque no sean clientes; la plantilla no los lista y el implementador debe añadirlos.
10. Los controles A.3 (roles, reporte de preocupaciones) y A.8 (información a partes interesadas) son la vía para satisfacer las expectativas identificadas.

Errores frecuentes: identificar solo reguladores y clientes y olvidar colaboradores, proveedores de modelos o datos y usuarios finales; puntuar sin criterios documentados; no revisar la matriz cuando cambia el contexto; dejar stakeholders en la matriz que no están en la tabla (como OEFA en el ejemplo) o al revés (ANPDP).

## 8. Limitaciones del análisis

- Las tablas "Criterios de Poder" y "Criterios de Influencia" están incrustadas como imágenes PNG; se transcribieron a partir de la imagen y se parafrasearon. La escala de influencia parece invertida respecto a la matriz (nivel 1 = relación más exigente; nivel 5 = ocasional); se ha señalado como probable error de la plantilla, sin corregir los datos.
- Inconsistencias del ejemplo: OEFA aparece en la matriz pero no en la tabla de identificación; ANPDP (poder 5, interés 3) no aparece en la matriz aunque debería ser Principal Stakeholder; los rótulos usan "Mantener Satisfecho / Mantener Informado(s)" con variación singular/plural.
- El archivo no contiene fórmulas: la ubicación en la matriz y la estrategia son manuales.
- La hoja 01STAKEHOLDER duplica el número de ítem "3" en las filas 21 y 23.
- Erratas del original corregidas: "STKEHOLDERS" → stakeholders; "Ministerio del ambiente" → Ministerio del Ambiente.
- No constan autor, fecha ni licencia; la relación con los controles del Anexo A es una interpretación funcional, no una cita del archivo.

## 9. Referencias

- ISO/IEC 42001:2023, cláusulas 4.2, 4.4, 5.2, 6.1.4, 7.4 y controles A.3.2, A.3.3, A.8.2–A.8.5 (referencia implícita; el archivo no cita la norma).
- Modelo de matriz poder/interés de Mendelow (referencia implícita por la estructura de cuatro cuadrantes).
- Entidades peruanas citadas: ANPDP (Autoridad Nacional de Protección de Datos Personales), Ministerio del Ambiente, OEFA (Organismo de Evaluación y Fiscalización Ambiental), SBS (Superintendencia de Banca, Seguros y AFP).
- Normativa de protección de datos personales (PDP) del Perú, mencionada en el requerimiento de ejemplo.
