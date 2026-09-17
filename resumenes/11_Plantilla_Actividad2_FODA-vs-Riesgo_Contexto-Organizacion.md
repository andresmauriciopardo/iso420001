---
titulo: "Plantilla Excel: Actividad 2 – FODA vs Riesgo. Análisis del contexto de la organización, conversión del FODA en riesgos y oportunidades priorizados y seguimiento de acciones"
archivo_fuente: "Plantillas para ejercicios y proyectos/Actividad2 FODA VS RIESGO.xlsx"
tipo_documento: plantilla_excel
organismo_emisor: "Material de curso (autor no identificado en el archivo; ejercicio de implementación ISO/IEC 42001)"
fecha_publicacion_fuente: "no consta (el dato de ejemplo tiene fecha 30.11.2024)"
idioma_fuente: es
paginas_o_hojas: "7 hojas: DATOS, ESCALAS, CONTEXTO ORGANIZACIÓN, ANÁLISIS RIESGOS, SEGUIMIENTO, ANÁLISIS2, ANÁLISIS"
version_resumen: "1.0"
fecha_resumen: "2026-09-17"
generado_por: "Claude (agente Analista de Plantillas – Análisis de Riesgo SGIA, FODA vs Riesgo y Stakeholders)"
palabras_clave: [contexto de la organización, cuestiones internas y externas, FODA, DAFO, SWOT, fortalezas, debilidades, oportunidades, amenazas, riesgos positivos, riesgos negativos, priorización, probabilidad, impacto, matriz 3x3, tratamiento del riesgo, seguimiento, ISO/IEC 42001 4.1, 6.1.1, 9.1]
documentos_relacionados_en_corpus: ["00_INDICE.md", "01_ISO-IEC-42001-2023_Requisitos_Clausulas-4-10.md", "03_ISO-IEC-42001-2023_Anexos-C-D_Objetivos-Fuentes-de-Riesgo-y-Sectores.md", "04_ISO-IEC-23894-2023_Gestion-de-Riesgos-de-IA.md", "06_Guia-Implementacion-Inicial-ISO42001_Impulsa360.md", "10_Plantilla_Analisis-de-Riesgo-SGIA.md", "12_Plantilla_Actividad3_Stakeholders_Partes-Interesadas.md"]
---

## 1. Resumen ejecutivo

"Actividad2 FODA VS RIESGO.xlsx" es una plantilla didáctica que enlaza el **análisis del contexto de la organización** (cláusula 4.1 de ISO/IEC 42001) con las **acciones para abordar riesgos y oportunidades** (6.1.1) y con su **seguimiento** (9.1). La organización lista **cuestiones internas y externas** clasificadas por **factores** (recursos humanos, tecnología, administración estratégica, presupuesto, operaciones, políticas, económicas, ecológicas, sociales), describe cada **aspecto**, indica si **impacta en la estrategia**, lo tipifica como **riesgo positivo o negativo**, lo **prioriza** (1 alto, 2 medio, 3 bajo) y lo etiqueta como **Fortaleza, Debilidad, Oportunidad o Amenaza**. Cada aspecto pasa por fórmula a la hoja "ANÁLISIS RIESGOS", donde se redacta el riesgo u oportunidad, se estima la **probabilidad** (Alto/Medio/Bajo), se identifican **causas**, se elige el **tratamiento** (Compartir, Modificar, Aceptar) y se asigna **acción, responsable y fecha**. La hoja "SEGUIMIENTO" programa las acciones en un calendario de 12 meses × 4 semanas con marcas "P" (programado) y "E" (ejecutado) y calcula el **porcentaje de cumplimiento** y el **estado** (PENDIENTE, EN PROCESO, EJECUTADO). "ANÁLISIS" y "ANÁLISIS2" generan conteos y gráficos; "ESCALAS" contiene las tablas de probabilidad, impacto, matriz 3 × 3 y niveles de riesgo (Aceptable, Aceptable condicionado, Inaceptable). La plantilla importa porque muestra cómo pasar de un ejercicio cualitativo (FODA) a un registro de riesgos trazable y medible, la evidencia que un auditor pedirá para 4.1 y 6.1.1.

## 2. Identificación y contexto del documento

- **Tipo**: libro Excel (.xlsx) con 7 hojas, 14 gráficos y fórmulas activas (43 en ANÁLISIS RIESGOS, 192 en SEGUIMIENTO, 99 en ANÁLISIS2 y 50 en ANÁLISIS).
- **Autoría y fecha**: no constan. El nombre "Actividad2" indica que es el segundo ejercicio de una serie del curso (la "Actividad3" es la plantilla de stakeholders).
- **Origen aparente**: la tabla de niveles de riesgo menciona reportar "a la Alta Dirección y a los titulares de los órganos del **MIDIS**" (Ministerio de Desarrollo e Inclusión Social del Perú), y el ejemplo habla de "políticas de **PDP**" (protección de datos personales). La plantilla procede, por tanto, de un contexto de gestión pública peruana y de un sistema de gestión de protección de datos, reutilizada para el SGIA.
- **Carácter**: herramienta informativa de trabajo; no contiene requisitos normativos propios, sino que ayuda a cumplir los de la norma.
- **Público**: equipos de implementación del SGIA, responsables de planificación estratégica y de calidad, y estudiantes del curso.
- **Licencia**: no consta.

## 3. Estructura del documento

| Hoja | Descripción |
|---|---|
| DATOS | Listas maestras para los desplegables: cuestiones, factores, impacta en la estrategia, tipo de impacto, priorización, FODA, códigos de programación (P, RP, E, PR) y estados |
| ESCALAS | Marco conceptual del contexto (cuestiones, dimensiones, aspectos, FODA e impacto positivo/negativo) y tablas de evaluación del riesgo (probabilidad, impacto, matriz 3 × 3, niveles de riesgo) |
| CONTEXTO ORGANIZACIÓN | Registro de aspectos del contexto: cuestión, factor, aspecto, impacto en la estrategia, tipo de impacto, priorización, FODA |
| ANÁLISIS RIESGOS | Riesgo u oportunidad por aspecto, probabilidad, causas, tratamiento, acción, responsable y fecha |
| SEGUIMIENTO | Cronograma anual por semanas, conteo de programado/ejecutado, porcentaje de cumplimiento y estado |
| ANÁLISIS2 | Conteos y porcentajes por FODA, por probabilidad y por tratamiento; gráficos |
| ANÁLISIS | Conteos y porcentajes por cuestión, factor, impacto en la estrategia, tipo de impacto, priorización y FODA; gráficos |

## 4. Contenido detallado

### 4.1 Hoja "DATOS"

Contiene las listas de valores usadas por las validaciones de datos del libro (los desplegables aparecen con referencia rota "#REF!" en el archivo, ver sección 8):

| Lista | Valores |
|---|---|
| CUESTIONES | INTERNAS, EXTERNAS |
| FACTORES | Recursos Humanos; Informática o Software o Soporte Tecnológico; Administración estratégica; Presupuesto; Operaciones o procesos; Políticas; Económicas; Ecológicas; Sociales |
| ¿IMPACTA EN LA ESTRATEGIA? | Sí, No |
| TIPO DE IMPACTO | Riesgos positivos, Riesgos negativos |
| PRIORIZACIÓN | 1, 2, 3 |
| FODA | Fortaleza, Debilidad, Amenaza, Oportunidad |
| PROGRAMACIÓN (columna M–N) | Programado = **P**; Re Programado = **RP** |
| PROGRAMACIÓN (columna P–Q) | Ejecutado = **E**; Re Programado = **PR** |
| ESTADO | PENDIENTE, EN PROCESO, EJECUTADO |

Los primeros cinco factores son de naturaleza interna y los cuatro últimos (políticas, económicas, ecológicas, sociales) externa, reflejando un análisis tipo PESTEL simplificado.

### 4.2 Hoja "ESCALAS"

#### 4.2.1 Marco del contexto de la organización (A4–G21)

| Cuestiones | Dimensiones | Aspectos | FODA | Impacto |
|---|---|---|---|---|
| CUESTIONES INTERNAS | Administración; Mercadeo o Imagen; Operaciones o procesos; Presupuesto; Recursos Humanos; Informática o Software o Soporte Tecnológico | Cultura organizacional; Sistemas de información; Activos de información; Flujo de información; Toma de decisiones; Diseño de la estrategia; Controles operacionales | Fortalezas / Debilidades | Oportunidades / Fortalezas → **POSITIVO** |
| CUESTIONES EXTERNAS | Políticas; Económicas; Sociales; Tecnológicas; Ecológicas; Competencias o sociedad civil | Regulatorios; Legales; Financieros; Tecnológicos; Económicos; Naturales; Nivel internacional; nacional; regional; local | Oportunidades / Amenazas | Debilidades / Amenazas → **NEGATIVO** |

La regla de lectura es: lo interno se clasifica como fortaleza o debilidad; lo externo como oportunidad o amenaza; fortalezas y oportunidades generan **riesgos positivos**; debilidades y amenazas generan **riesgos negativos**.

#### 4.2.2 Tablas de evaluación del riesgo (A24–C51)

**Tabla N° 2 – Probabilidad**

| Nivel | Escala | Descripción |
|---|---|---|
| Rara vez | 1 | Es poco probable la materialización del riesgo |
| Ocasional | 2 | Es probable; se presenta ocasionalmente |
| Frecuente | 3 | Es muy probable; se presenta constantemente |

**Tabla N° 3 – Impacto (severidad)**

| Nivel | Escala | Descripción |
|---|---|---|
| Baja | 1 | Bajo impacto; no afecta al proceso |
| Media | 2 | Impacto medio; afecta al proceso medianamente |
| Alta | 3 | Alto impacto; afecta el desarrollo de los procesos y presenta daños |

**Tabla N° 4 – Matriz de probabilidad e impacto** (valor = probabilidad × impacto)

| Impacto \ Probabilidad | Rara vez (1) | Ocasional (2) | Frecuente (3) |
|---|---|---|---|
| Alta (3) | 3 | 6 | 9 |
| Media (2) | 2 | 4 | 6 |
| Baja (1) | 1 | 2 | 3 |

**Tabla N° 5 – Niveles de riesgo y acciones de control**

| Escala | Nivel del riesgo | Acciones de control |
|---|---|---|
| 1 y 2 | Aceptable | No se necesitan acciones; el riesgo está controlado, tiene poco impacto o se administra con procedimientos rutinarios |
| 3 y 4 | Aceptable condicionado | Deben hacerse esfuerzos para reducir el riesgo; las medidas se implementan en un periodo determinado |
| 6 y 9 | Inaceptable | Acción inmediata; planes de tratamiento requeridos, implementados y reportados a la Alta Dirección (y, en el original, a los titulares de los órganos del MIDIS) |

Nota: la hoja "ANÁLISIS RIESGOS" solo pide la **probabilidad** (Alto/Medio/Bajo); la matriz probabilidad × impacto de la hoja ESCALAS no está conectada por fórmula al registro. Es un marco de referencia que el usuario aplica manualmente.

### 4.3 Hoja "CONTEXTO ORGANIZACIÓN"

Registro principal de aspectos (filas 5–25). Columnas:

| Col. | Campo | Valores |
|---|---|---|
| A | N° | Correlativo (el ejemplo muestra huecos: 1–8, 14, 15, 17, 21, 22, 23, 27–30, señal de filas eliminadas) |
| B | CUESTIONES | INTERNAS / EXTERNAS |
| C | FACTORES | Lista de DATOS |
| D | ASPECTOS | Texto libre que describe la cuestión concreta |
| E | ¿IMPACTA EN LA ESTRATEGIA? | Sí / No |
| F | TIPO DE IMPACTO | Riesgos positivos / Riesgos negativos |
| G | PRIORIZACIÓN | 1: Alto, 2: Medio, 3: Bajo |
| H | FODA | Fortaleza / Debilidad / Oportunidad / Amenaza |

**Dato de ejemplo completo (fila 5)**: N° 1, INTERNAS, Recursos Humanos, aspecto "Alta rotación en directivos de la organización, lo que no permite que se implementen correctamente las políticas de PDP", impacta en la estrategia: Sí, Riesgos negativos, priorización 1, **Debilidad**.

El resto de filas solo tiene clasificación parcial: 7 Fortalezas (filas 6–12; varias marcadas incoherentemente como "Riesgos negativos"), 4 Debilidades, 3 Oportunidades y 6 Amenazas, los **20 aspectos** que cuentan las hojas de análisis. Las columnas W–X contienen apuntes sueltos del taller ("Indicadores de incidentes", "Proceso de compras", "Programa de multiplica Pro", "Qué cursos de ingeniería se tienen").

### 4.4 Hoja "ANÁLISIS RIESGOS"

Título "ANÁLISIS DE RIESGOS DEL CONTEXTO". Cada fila (6–26) se vincula por fórmula con la hoja anterior: **A = FODA** (`='CONTEXTO ORGANIZACIÓN'!H5`) y **B = Aspecto estratégico** (`='CONTEXTO ORGANIZACIÓN'!D5`). La fila 16 muestra `#REF!` porque la fila de origen fue borrada. Columnas:

| Col. | Campo | Contenido |
|---|---|---|
| A | FODA | Heredado |
| B | ASPECTO ESTRATÉGICO | Heredado |
| C | RIESGO / OPORTUNIDADES | Redacción del riesgo (negativo) o de la oportunidad (positivo) |
| D–F | PROBABILIDAD QUE OCURRA | Marcar "x" en ALTO, MEDIO o BAJO |
| G | CAUSAS | Causa raíz |
| H–J | TRATAMIENTO DEL RIESGO | Marcar "x" en **COMPARTIR**, **MODIFICAR** o **ACEPTAR** |
| K | ACCIÓN | Acción de tratamiento |
| L | RESPONSABLE | Persona |
| M | FECHA | Fecha compromiso |

**Ejemplo (fila 6)**: Debilidad → aspecto "Alta rotación en directivos…" → riesgo "Personal no cuenta con una cultura del SG (sistema de gestión)" → probabilidad ALTO → causa "Falta de políticas de retención del talento" → tratamiento MODIFICAR → acción "Establecer un plan línea de carrera dentro de la organización" → responsable Carlo Vargas → fecha 30.11.2024.

Totales (filas 27–29): `COUNTA` de cada columna de probabilidad y de tratamiento, suma (`D28`, `H28`) y porcentaje sobre el total de riesgos evaluados (`D29 = D27/$D$28`). En el ejemplo, 1 riesgo ALTO (100 %) y 1 tratamiento MODIFICAR (100 %).

Las tres opciones de tratamiento corresponden a la terminología de ISO 31000: **compartir** (transferir), **modificar** (reducir la probabilidad o el impacto) y **aceptar** (retener). La hoja no incluye "evitar" ni, para los riesgos positivos, "explotar/mejorar", aunque la hoja ANÁLISIS2 usa las etiquetas EVITAR/REDUCIR/ASUMIR (ver 4.6).

### 4.5 Hoja "SEGUIMIENTO"

Título "SEGUIMIENTO A LAS ACTIVIDADES DE RIESGOS DEL CONTEXTO". Cada par de filas (7–8, 9–10, …, 47–48) corresponde a un riesgo y hereda por fórmula RIESGO, CAUSAS, ACCIÓN, RESPONSABLE y FECHA desde "ANÁLISIS RIESGOS" (`A7='ANÁLISIS RIESGOS'!C6`, etc.). A la derecha hay un **cronograma** de ENERO a DICIEMBRE con cuatro semanas por mes (columnas F–BA, 48 celdas).

Mecánica: en la **fila superior** del par se marca la semana **programada** con "P" (o "RP" si se reprograma); en la **fila inferior** se marca la semana **ejecutada** con "E" (o "PR" si se reprograma la ejecución). Fórmulas:

- `BB7 = COUNTIF($F7:$BA7, DATOS!$N$2)` → número de semanas programadas ("P").
- `BB8 = COUNTIF($F8:$BA8, DATOS!$Q$2)` → número de semanas ejecutadas ("E").
- `BC7 = IF(BB7=0, 0%, BB8/BB7)` → **porcentaje de cumplimiento** = ejecutado / programado.
- `BD7 = IF(COUNTA(F7:BA7)=0, "", IF(BC7=0%, "PENDIENTE", IF(BC7=100%, "EJECUTADO", "EN PROCESO")))` → **estado**.
- Columna BE: OBSERVACIONES (texto libre).
- `BB51` calcula un indicador global de cumplimiento (muestra `#DIV/0!` porque no hay marcas en el ejemplo) que alimenta el gráfico "CUMPLIMIENTO DE ACTIVIDADES" incrustado en la hoja.

El ejemplo trae la acción de Carlo Vargas sin ninguna semana marcada, por lo que cuenta 0/0 y estado vacío.

### 4.6 Hoja "ANÁLISIS2"

Cuadro de mando del análisis de riesgos, con gráficos:

- **Conteo FODA** (A1–C5): `COUNTIF` sobre la columna A de ANÁLISIS RIESGOS. Ejemplo: Fortaleza 7 (35 %), Debilidad 4 (20 %), Amenaza 6 (30 %), Oportunidad 3 (15 %); total 20.
- **FODA × Probabilidad** (A8–H17): `COUNTIFS` que cruzan FODA con la marca "X" en las columnas ALTO/MEDIO/BAJO. Ejemplo: Debilidad-ALTO = 1; el resto 0. Filas 14–17 calculan porcentajes por columna (`#DIV/0!` donde el total es 0).
- **FODA × Tratamiento** (A19–H28): mismo esquema sobre las columnas H, I, J de ANÁLISIS RIESGOS, pero con las etiquetas **EVITAR / REDUCIR / ASUMIR**. Como las fórmulas cuentan por posición de columna, EVITAR se corresponde con COMPARTIR, REDUCIR con MODIFICAR y ASUMIR con ACEPTAR. Ejemplo: Debilidad-REDUCIR = 1.
- **Bloque auxiliar D30–F46**: tabla de 10 valores para dibujar un gráfico de medidor (gauge) con marcas INICIO/FIN y coordenadas X/Y; muestra `#DIV/0!` sin datos.

Gráficos vinculados (según la estructura del archivo): gráfico circular FODA (`ANÁLISIS2!A2:C5`), gráfico de barras FODA × probabilidad (`E8:G12`), cuatro gráficos por categoría FODA de tratamiento (`E20:G23`), un gráfico de tratamiento global (`'ANÁLISIS RIESGOS'!H29:J29`) y el medidor.

### 4.7 Hoja "ANÁLISIS"

Cuadro de mando del contexto, con seis tablas conteo/porcentaje basadas en `COUNTIF` sobre "CONTEXTO ORGANIZACIÓN":

| Tabla | Rango fuente | Ejemplo |
|---|---|---|
| Cuestiones de la organización | Columna B | Internas 1 (100 %), Externas 0 |
| Factores (total de aspectos identificados) | Columna C | Recursos Humanos 1; demás 0 |
| ¿Impacta en la estrategia? | Columna E | Sí 1, No 0 |
| Tipo de impacto | Columna F | Riesgos positivos 7 (50 %), negativos 7 (50 %) |
| Priorización | Columna G | 1 → 1; 2 y 3 → 0 |
| FODA | Columna H | Fortaleza 7, Debilidad 4, Amenaza 6, Oportunidad 3 (total 20) |

Cada tabla alimenta un gráfico (seis gráficos en la hoja). Los conteos revelan que el ejemplo está incompleto: solo un aspecto tiene cuestión, factor y priorización, mientras que 14 tienen tipo de impacto y 20 tienen FODA.

## 5. Conceptos y términos clave

| Término (ES) | Término (EN) | Definición breve | Dónde aparece |
|---|---|---|---|
| Cuestiones internas y externas | Internal and external issues | Factores del entorno de la organización relevantes para su propósito y para el SGIA (cláusula 4.1) | DATOS A2–A3; ESCALAS A5, A12 |
| FODA / DAFO | SWOT | Fortalezas, Oportunidades, Debilidades, Amenazas | DATOS K2–K5 |
| Riesgo positivo / negativo | Positive / negative risk (opportunity / threat) | Efecto favorable o desfavorable de la incertidumbre sobre los objetivos | DATOS G2–G3 |
| Aspecto | Issue / aspect | Descripción concreta de una cuestión del contexto | CONTEXTO ORGANIZACIÓN D |
| Priorización | Prioritisation | 1 alto, 2 medio, 3 bajo | CONTEXTO ORGANIZACIÓN G |
| Probabilidad | Likelihood | Rara vez (1), Ocasional (2), Frecuente (3) | ESCALAS A28–C30 |
| Impacto (severidad) | Impact / severity | Baja (1), Media (2), Alta (3) | ESCALAS A34–C36 |
| Nivel de riesgo | Risk level | Aceptable (1–2), Aceptable condicionado (3–4), Inaceptable (6–9) | ESCALAS A49–C51 |
| Compartir / Modificar / Aceptar | Share / Modify / Accept (retain) | Opciones de tratamiento del riesgo | ANÁLISIS RIESGOS H5–J5 |
| Evitar / Reducir / Asumir | Avoid / Reduce / Assume | Etiquetas alternativas de tratamiento en el cuadro de mando | ANÁLISIS2 B19–D19 |
| Programado / Reprogramado / Ejecutado | Scheduled / Rescheduled / Executed | Códigos P, RP, E, PR del cronograma | DATOS M2–Q3 |
| Estado | Status | PENDIENTE, EN PROCESO, EJECUTADO según % de cumplimiento | SEGUIMIENTO BD |

## 6. Relación con otros documentos del corpus

- **ISO/IEC 42001, 4.1 (contexto)**: la norma exige determinar las cuestiones externas e internas pertinentes al propósito que afectan los resultados previstos del SGIA, incluido el rol de la organización respecto a la IA. "ESCALAS" (marco) y "CONTEXTO ORGANIZACIÓN" (registro) son la evidencia directa; la plantilla no incluye el **rol respecto a la IA** y debe complementarse.
- **4.2 (partes interesadas)**: no se registran aquí; se complementa con el archivo 12 (Stakeholders).
- **6.1.1 (acciones para abordar riesgos y oportunidades)**: la norma pide considerar 4.1 y 4.2 para determinar riesgos y oportunidades, planificar acciones, integrarlas en el SGIA y evaluar su eficacia. "ANÁLISIS RIESGOS" convierte cada cuestión en riesgo/oportunidad con acción, responsable y fecha; "SEGUIMIENTO" mide la ejecución. A diferencia del archivo 10 (riesgos por activo de IA, 6.1.2), aquí el objeto son los riesgos **del sistema de gestión y del contexto**.
- **9.1 y 9.3**: el porcentaje de cumplimiento, el estado por acción y los gráficos de ANÁLISIS/ANÁLISIS2 son indicadores presentables en la revisión por la dirección.
- **ISO 31000 / ISO/IEC 23894**: la terminología compartir-modificar-aceptar, la matriz probabilidad × impacto y los niveles de aceptación siguen ISO 31000, que 23894 adapta a la IA.

## 7. Aplicación práctica y puntos de examen

Secuencia de uso: (1) acordar las escalas de ESCALAS con la dirección; (2) en taller, poblar "CONTEXTO ORGANIZACIÓN" recorriendo los nueve factores; (3) decidir impacto en la estrategia, priorizar y etiquetar FODA; (4) en "ANÁLISIS RIESGOS" redactar riesgo u oportunidad, probabilidad, causas, tratamiento, acción, responsable y fecha; (5) programar semanas con "P" y registrar "E" en "SEGUIMIENTO"; (6) presentar los gráficos en la revisión por la dirección; (7) actualizar el contexto cuando cambie.

Puntos "debes saber":

1. Lo **interno** se clasifica como fortaleza o debilidad; lo **externo** como oportunidad o amenaza.
2. Fortalezas y oportunidades generan **riesgos positivos**; debilidades y amenazas, **riesgos negativos**: la norma habla de riesgos **y oportunidades**.
3. Priorización 1 = alto, 2 = medio, 3 = bajo.
4. La matriz 3 × 3 multiplica probabilidad por impacto; valores posibles 1, 2, 3, 4, 6 y 9.
5. Niveles: 1–2 Aceptable; 3–4 Aceptable condicionado; 6–9 Inaceptable con reporte a la alta dirección.
6. Tratamiento: compartir (transferir), modificar (reducir), aceptar (retener); el cuadro de mando las llama evitar/reducir/asumir.
7. Estado = porcentaje ejecutado/programado: 0 % PENDIENTE, 100 % EJECUTADO, intermedio EN PROCESO.
8. "RP"/"PR" marcan reprogramación y no cuentan como programado ni ejecutado.
9. La cláusula 4.1 exige además el **rol de la organización respecto a la IA**, ausente en la plantilla.
10. La trazabilidad por fórmula entre hojas es la evidencia de que los riesgos de 6.1.1 derivan de 4.1 y 4.2.

Errores frecuentes: aspectos con FODA pero sin factor ni priorización (en el ejemplo, 20 etiquetados y solo 1 completo); una fortaleza clasificada como "riesgo negativo"; borrar filas y romper fórmulas (#REF!); no marcar semanas, con lo que el estado queda vacío y el indicador global da error.

## 8. Limitaciones del análisis

- Todas las validaciones de datos del libro (hojas DATOS, ANÁLISIS2, ANÁLISIS) tienen la fórmula de origen rota (`formula1=#REF!`), por lo que los desplegables no funcionan hasta que se reasignen a las listas de la hoja DATOS.
- La fila 16 de "ANÁLISIS RIESGOS" muestra `#REF!` por una fila eliminada en "CONTEXTO ORGANIZACIÓN"; la numeración de esa hoja tiene huecos (8→14, 15→17, 18→21, 23→27).
- Las etiquetas de tratamiento son inconsistentes entre hojas (COMPARTIR/MODIFICAR/ACEPTAR frente a EVITAR/REDUCIR/ASUMIR); el mapeo posicional indicado en 4.6 es una deducción a partir de las fórmulas.
- La matriz probabilidad × impacto y los niveles de riesgo de ESCALAS no están conectados por fórmula al registro; "ANÁLISIS RIESGOS" solo captura probabilidad.
- El ejemplo está incompleto: un solo riesgo desarrollado; el resto son etiquetas FODA sin texto.
- Los 14 gráficos y la imagen incrustada no se extrajeron; se describen por sus rangos de datos.
- Erratas del original corregidas: "politicas", "problable", "linea", "Re Programado", "MIDIS" mantenido como referencia institucional.
- No constan autor, fecha ni licencia.

## 9. Referencias

- ISO/IEC 42001:2023, cláusulas 4.1, 4.2, 6.1.1, 9.1 y 9.3 (referencia implícita: el archivo forma parte del material del curso; no cita la norma textualmente).
- ISO 31000, *Gestión del riesgo — Directrices* (terminología de tratamiento y matriz probabilidad-impacto), referencia implícita.
- ISO/IEC 23894:2023 (adaptación de ISO 31000 a la IA), referencia implícita.
- MIDIS (Ministerio de Desarrollo e Inclusión Social, Perú), mencionado en la Tabla N° 5 de la hoja ESCALAS.
- Ley de Protección de Datos Personales (PDP), mencionada en el aspecto de ejemplo.
