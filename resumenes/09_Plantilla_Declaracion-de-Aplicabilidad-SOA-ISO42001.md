---
titulo: "Plantilla Excel: Declaración de Aplicabilidad (SoA) ISO/IEC 42001 - controles del Anexo A, nivel de implementación y madurez por dominio"
archivo_fuente: "Plantillas para ejercicios y proyectos/Declaración de Aplicabilidad 42001 - SOAv2.xlsx"
tipo_documento: plantilla_excel
organismo_emisor: "Material didáctico del curso (Impulsa360 Academy / plantilla adaptada de una SoA ISO/IEC 27001); autor no consta en el archivo"
fecha_publicacion_fuente: "no consta (referencia a ISO/IEC 42001:2023; versión 'v2' según el nombre del archivo)"
idioma_fuente: es
paginas_o_hojas: "3 hojas: 'SOA-42001', 'Madurez 42001', 'Datos'"
version_resumen: "1.0"
fecha_resumen: "2026-09-17"
generado_por: "Claude (agente Analista de Plantillas - Diagnóstico de Brechas y Declaración de Aplicabilidad)"
palabras_clave: [Declaración de Aplicabilidad, SoA, Statement of Applicability, ISO/IEC 42001, cláusula 6.1.3, tratamiento de riesgos, Anexo A, 38 controles, aplicabilidad, justificación, nivel de implementación, madurez por dominio, gráfico radar, listas desplegables]
documentos_relacionados_en_corpus: ["resumen de la norma ISO/IEC 42001 (cláusula 6.1.3 y Anexo A)", "resumen de UNE-ISO/IEC 42001 (versión española)", "08_Plantilla_Diagnostico-Analisis-de-Brechas-ISO42001.md", "resumen de la plantilla 'Analisis de Riesgo SGIA v.03'", "resumen de la guía de implementación del SGIA", "resumen de ISO/IEC 23894 (gestión de riesgos de IA)", "resumen de ISO 19011 (auditoría de sistemas de gestión)"]
---

## 1. Resumen ejecutivo

La plantilla **"Declaración de Aplicabilidad 42001 - SOAv2.xlsx"** implementa el documento que la cláusula **6.1.3 (Tratamiento de riesgos de la IA)** de ISO/IEC 42001:2023 exige a toda organización: la **Declaración de Aplicabilidad (SoA, Statement of Applicability)**, que enumera los controles necesarios, justifica su inclusión o exclusión e indica si están implementados. La hoja principal, **"SOA-42001"**, lista los **38 controles del Anexo A** agrupados en los dominios A.2 a A.10, con el texto del control, una lista desplegable de **Aplicabilidad (Si/No)**, campos de **Justificación** y **Documento relacionado**, una lista desplegable de **Nivel de Implementación** y tres columnas calculadas: **% de implementación del control** (0 %, 15 %, 25 %, 50 %, 75 % o 100 %), **% del dominio** y **Grado de implementación** (etiqueta cualitativa de "No Diseñado" a "Completamente Implementado").

La hoja **"Madurez 42001"** promedia esos porcentajes por bloques de controles, los traduce a un nivel de madurez y los dibuja en un **gráfico radar** titulado "Madurez de Dominios"; la hoja **"Datos"** define la escala de seis niveles con su ponderación numérica y las bandas para calificar dominios. Los datos de ejemplo muestran una organización con los 38 controles "Completamente implementados" (100 %) y un solo control excluido (A.2.2 Política de IA, marcado "No"), sin justificaciones ni documentos.

La plantilla es útil para el estudiante porque hace tangible qué debe contener una SoA y cómo se puede medir el avance de implementación. Sin embargo, es una adaptación de una SoA de ISO/IEC 27001 con varios defectos heredados: los nombres de dominio de "Madurez 42001" (Organizacionales, Personas, Físicos, Tecnológicos) son los temas de ISO/IEC 27002 y no los del Anexo A de 42001; una fórmula devuelve `#REF!`; la lista desplegable ofrece solo 4 de los 6 niveles que reconocen las fórmulas; la columna "% del dominio" no promedia el dominio; y los títulos de A.7.2 y A.7.3 están equivocados. Estos hallazgos se detallan en las secciones 4 y 8.

## 2. Identificación y contexto del documento

- **Tipo**: libro Excel (.xlsx), plantilla de trabajo del curso. Informativo/didáctico; no es un documento normativo.
- **Norma de referencia**: ISO/IEC 42001:2023 (encabezado C3 "CONTROL ISO/IEC 42001:2023"; definiciones de la hoja Datos citan "el requisito de la norma ISO/IEC 42001").
- **Versión**: "v2" según el nombre del archivo; no hay historial de cambios ni autor. Los metadatos del gráfico radar indican configuraciones regionales es-MX y es-PE.
- **Origen**: adaptación de una SoA de ISO/IEC 27001:2022, como delatan los cuatro "dominios" de la hoja Madurez (los temas organizacional, personas, físico y tecnológico de ISO/IEC 27002:2022) y la fórmula `#REF!` que quedó al eliminar el bloque de controles tecnológicos.
- **Destinatarios**: responsable del SGIA, equipo de implementación, propietarios de riesgo, auditores internos y externos (la SoA es uno de los primeros documentos que el auditor solicita) y estudiantes de Lead Implementer / Lead Auditor.
- **Posición en el ciclo de trabajo del curso**: se completa después del diagnóstico de brechas y del análisis de riesgos, como resultado del proceso de tratamiento de riesgos (6.1.3), y se actualiza en cada revisión del SGIA.

## 3. Estructura del documento

| Hoja | Rango usado | Contenido | Fórmulas / objetos |
|---|---|---|---|
| **SOA-42001** | A1:L925 (datos reales en C3:L53) | 38 controles del Anexo A en 10 bloques de dominio con columnas de aplicabilidad, justificación, documento, nivel y porcentajes | 114 fórmulas (J, K, L en cada control); 2 validaciones de lista; formato condicional en K y L; C53 = 38 (constante) |
| **Madurez 42001** | B3:E26 | Promedio de implementación por 4 "dominios", nivel de madurez, promedio global y leyenda de bandas | 9 fórmulas (`AVERAGE`, `IF` anidados); 1 gráfico radar "Madurez de Dominios" |
| **Datos** | C3:E18 | Tabla de referencia: 6 niveles de implementación de controles con definición y valor; bandas de nivel de dominio | Sin fórmulas ni validaciones (tabla informativa, no enlazada) |

## 4. Contenido detallado

### 4.1 Hoja "SOA-42001"

#### 4.1.1 Columnas

Las columnas A y B están vacías. La fila 3 contiene los encabezados:

| Columna | Encabezado | Tipo | Descripción |
|---|---|---|---|
| C | **CONTROL ISO/IEC 42001:2023** (combinada C3:D3) | Texto fijo | Código del control sin el prefijo "A." (2.2, 3.2, 6.2.4, 10.4...). En las filas de dominio (C4, C8, C11, C17, C22, C23, C26, C34, C40, C45, C49, combinadas hasta L) figura el nombre del dominio y su **objetivo**. |
| D | (sin encabezado propio) | Texto fijo | Título del control. |
| E | (sin encabezado) | Texto fijo | Texto del control parafraseado ("La organización debe..."). |
| F | **Aplicabilidad** | Lista desplegable **"Si, No"** | Decisión de incluir o excluir el control. |
| G | **Justificación de la inclusión o exclusión** | Texto libre | Motivo de la decisión (riesgo tratado, requisito legal/contractual, no existe el escenario...). Vacía en la plantilla. |
| H | **Documento Relacionado** | Texto libre | Política, procedimiento o registro que evidencia el control. Vacía en la plantilla. |
| I | **Nivel de Implementación** | Lista desplegable **"No aplica, Diseñado, Parcialmente implementado, Completamente implementado"** | Estado de implementación observado. |
| J | **% Implement. / Del Control** | Fórmula | Convierte el nivel en porcentaje (ver 4.1.3). |
| K | **% Implementación del Dominio** | Fórmula | `=AVERAGE(Jn:Jn)` o `=+Jn`: en la práctica copia el valor de J del mismo control; **no** promedia el dominio. |
| L | **Grado de Implementación** | Fórmula | Etiqueta cualitativa derivada de K (ver 4.1.3). |

`C53` contiene el número **38** escrito como constante (no es una fórmula `COUNTA`), por lo que no se actualiza si se añaden controles propios.

#### 4.1.2 Listas desplegables y formato condicional

- **F (Aplicabilidad)**: lista literal `Si, No` aplicada a las 38 filas de control (`F5:F7, F9:F10, F12:F16, F18:F21, F24:F25, F27:F33, F35:F39, F41:F44, F46:F48, F50:F52`).
- **I (Nivel de Implementación)**: lista literal de **cuatro** opciones: `No aplica`, `Diseñado`, `Parcialmente implementado`, `Completamente implementado`, en las mismas filas. **No incluye** "Inicialmente diseñado" ni "Parcialmente diseñado", aunque las fórmulas de J y la hoja Datos sí los contemplan; el usuario solo puede obtener esos valores escribiéndolos a mano (la validación admite celdas en blanco pero rechaza textos fuera de la lista, salvo que se desactive).
- **Formato condicional en K** (solo hasta la fila 39, es decir, dominios A.2 a A.7): seis bandas de color para 0; 0,001-0,15; 0,1501-0,25; 0,2501-0,50; 0,5001-0,75; 0,7501-1. Los controles de A.8, A.9 y A.10 (K41:K52) quedan sin colorear.
- **Formato condicional en L** (las 38 filas): un color por cada una de las seis etiquetas de grado.

#### 4.1.3 Fórmulas y escala de ponderación

Para cada control n:

- `Jn = IF(In="Inicialmente diseñado";15%; IF(In="Parcialmente diseñado";25%; IF(In="Diseñado";50%; IF(In="Parcialmente implementado";75%; IF(In="Completamente implementado";1; 0)))))`. Cualquier otro valor (vacío, "No aplica", "No diseñado", errores tipográficos) vale **0 %**.
- `Kn = AVERAGE(Jn:Jn)` en la mayoría de filas y `=+Jn` en las filas 41, 42, 46, 50, 51 y 52; ambas expresiones equivalen a Jn.
- `Ln = IF(Kn>75%;"Completamente Implementado"; IF(Kn>50%;"Parcialmente Implementado"; IF(Kn>25%;"Diseñado"; IF(Kn>15%;"Parcialmente Diseñado"; IF(Kn>0%;"Inicialmente Diseñado";"No Diseñado")))))`.

Correspondencia resultante entre nivel elegido, ponderación y grado:

| Nivel de implementación (columna I) | Valor J | Grado L |
|---|---|---|
| (vacío) o **No aplica** o No diseñado | 0 % | No Diseñado |
| Inicialmente diseñado (no está en la lista) | 15 % | Inicialmente Diseñado |
| Parcialmente diseñado (no está en la lista) | 25 % | Parcialmente Diseñado |
| **Diseñado** | 50 % | Diseñado |
| **Parcialmente implementado** | 75 % | Parcialmente Implementado |
| **Completamente implementado** | 100 % | Completamente Implementado |

Observación importante: al valer 0 %, un control marcado **"No aplica"** se contabiliza como no diseñado y **penaliza los promedios** de la hoja Madurez; la aplicabilidad "No" de la columna F no excluye la fila de los cálculos. Para una SoA correcta habría que excluir los controles no aplicables del promedio (por ejemplo, con `AVERAGEIF`).

#### 4.1.4 Lista completa de controles tal como aparecen (38) y verificación

| Dominio (fila de objetivo) | Código y título en la plantilla | Observaciones |
|---|---|---|
| **A2. Politicas** (fila 4): dirección y soporte de gestión para los sistemas de IA según los requisitos del negocio | 2.2 Política de IA; 2.3 Alineación con otras políticas de la organización; 2.4 Revisión de la política de IA | Correctos. Rótulo sin punto ni tilde ("A2. Politicas"). |
| **A3. Organización interna** (fila 8): rendición de cuentas para un enfoque responsable de la IA | 3.2 Roles y responsabilidades de la IA; 3.3 Notificación de inquietudes | Correctos. |
| **A.4 Recursos para sistemas de IA** (fila 11): inventariar los recursos para comprender riesgos e impactos | 4.2 Documentación de recursos; 4.3 Recursos de datos; 4.4 Recursos de herramientas; 4.5 Recursos de sistema e informáticos; 4.6 Recursos humanos | Correctos. |
| **A.5 Evaluación de los impactos de los sistemas de IA** (fila 17): impactos sobre personas, grupos y sociedades durante el ciclo de vida | 5.2 Proceso de evaluación del impacto; 5.3 Documentación de las evaluaciones de impacto; 5.4 Evaluación del impacto en personas o grupos; 5.5 Evaluación del impacto social | Correctos. |
| **A.6 Ciclo de vida del sistema de IA** (fila 22) / **A.6.1 Guía de gestión para el desarrollo** (fila 23): objetivos y procesos para el desarrollo responsable | 6.1.2 Objetivos para el desarrollo responsable; 6.1.3 Procesos para el diseño y desarrollo responsable | Correctos. |
| **A.6.2 Ciclo de vida del sistema de IA** (fila 26): criterios y requisitos para cada etapa | 6.2.2 Requisitos y especificaciones; 6.2.3 Documentación del diseño y desarrollo; 6.2.4 Verificación y validación; 6.2.5 Implementación; 6.2.6 Operación y monitoreo; 6.2.7 Documentación técnica; 6.2.8 "Registro de registros de eventos" | 6.2.8 con traducción redundante (registro de eventos, *event logs*). |
| **A.7 Datos para sistemas de IA** (fila 34): comprender la función e impacto de los datos en el ciclo de vida | 7.2 "Documentación del sistema e información para los usuarios"; 7.3 "Informes externos"; 7.4 Calidad de los datos; 7.5 Procedencia de los datos; 7.6 Preparación de los datos | **Erratas en 7.2 y 7.3**: los títulos son los de A.8.2 y A.8.3. En la norma, A.7.2 es *Datos para el desarrollo y mejora del sistema de IA* y A.7.3 *Adquisición de datos*. Los textos de la columna E sí son los correctos (procesos de gestión de datos; adquisición y selección de datos). |
| **A.8 Información para las partes interesadas** (fila 40): información necesaria para comprender y evaluar riesgos e impactos | 8.2 Documentación del sistema e información para los usuarios; 8.3 Informes externos; 8.4 Comunicación de incidentes; 8.5 Información para las partes interesadas | Correctos. |
| **A.9 Uso de los sistemas de IA** (fila 45): uso responsable y conforme a las políticas | 9.2 Procesos para el uso responsable; 9.3 Objetivos para el uso responsable; 9.4 Uso previsto del sistema de IA | Correctos. |
| **A.10 Relaciones con terceros y clientes** (fila 49): responsabilidades y distribución de riesgos con terceros | 10.2 Asignación de responsabilidades; 10.3 Proveedores; 10.4 Clientes | Correctos. |

Recuento: 3+2+5+4+2+7+5+4+3+3 = **38**, igual que el Anexo A de ISO/IEC 42001:2023. **No falta ni sobra ningún control**; las únicas anomalías son las erratas de título en A.7.2/A.7.3 y detalles de rotulación. La columna E ofrece una paráfrasis fiel de cada control (por ejemplo, 6.2.6 exige definir los elementos para la operación continua, incluidos monitorización, reparaciones, actualizaciones y soporte; 6.2.8 exige habilitar el registro de eventos al menos cuando el sistema está en uso).

#### 4.1.5 Datos de ejemplo

Todas las filas tienen I = "Completamente implementado", J = 1, K = 1 y L = "Completamente Implementado". La columna F es "Si" en 37 controles y **"No" en A.2.2 Política de IA** (F5). Las columnas G y H están vacías. El ejemplo representa una SoA "perfecta" en cuanto a implementación, pero sirve también como caso de estudio de incoherencia: un control excluido aparece como implementado al 100 %, sin justificación; además, la exclusión de A.2.2 sería indefendible ante un auditor porque la cláusula 5.2 obliga a tener una política de IA.

### 4.2 Hoja "Madurez 42001"

| Celda | Contenido | Fórmula |
|---|---|---|
| B3:D3 | Encabezados: Dominios / % Implemen. / Nivel de Implementación | - |
| B4 **Controles Organizacionales** | 100 % / Completamente Implementado | `C4 = +AVERAGE('SOA-42001'!K5:K39)` (controles de A.2 a A.7; `AVERAGE` ignora las filas de dominio vacías) |
| B5 **Personas** | 100 % / Completamente Implementado | `C5 = AVERAGE('SOA-42001'!K41:K48)` (controles de A.8 y A.9) |
| B6 **Controles Físicos** | 100 % / Completamente Implementado | `C6 = +AVERAGE('SOA-42001'!J50:J52)` (controles de A.10) |
| B7 **Controles Tecnológicos** | `#REF!` | `C7 = AVERAGE('SOA-42001'!#REF!)`: el rango referenciado fue borrado |
| D4:D7 | Nivel de implementación | Misma cadena `IF` que la columna L de la SoA, aplicada a C |
| B18 **Promedio de Madurez de Dominios** | `#REF!` | `C18 = AVERAGE(C4:C17)`; hereda el error de C7 |
| B20:C26 | Leyenda "Nivel de Madurez de Dominios": No diseñado = 0; Inicialmente diseñado > 0 % - 15 %; Parcialmente diseñado > 15 % - 25 %; Diseñado > 25 % - 50 %; Parcialmente implementado > 50 % - 75 %; Completamente implementado > 75 % - 100 % | Texto |

El **gráfico radar** "Madurez de Dominios" toma los valores `C4:C7` con categorías `B4:B7`; con el `#REF!` de C7 solo se dibujan tres vértices. Los cuatro "dominios" no corresponden a los nueve dominios de ISO/IEC 42001 (A.2-A.10) sino a los cuatro temas de ISO/IEC 27002:2022, y el reparto de controles entre ellos (A.2-A.7 en "Organizacionales", A.8-A.9 en "Personas", A.10 en "Físicos") es arbitrario. Para un uso real habría que sustituir las filas 4-7 por los nueve dominios reales con sus rangos (`K5:K7`, `K9:K10`, `K12:K16`, `K18:K21`, `K24:K33`, `K35:K39`, `K41:K44`, `K46:K48`, `K50:K52`) y corregir C18.

### 4.3 Hoja "Datos"

Tabla de referencia, no enlazada por fórmulas ni validaciones (las fórmulas J y L tienen los porcentajes y etiquetas escritos literalmente y la lista desplegable de I es una lista fija de cuatro elementos).

**Nivel de Implementación de Controles** (C3:E9), con definiciones parafraseadas:

| Tipo | Definición | Valor |
|---|---|---|
| **No diseñado** | No se tiene el requisito ni se ha bosquejado su implementación | 0 |
| **Inicialmente diseñado** | La implementación se ha iniciado pero no culminado | 0,15 |
| **Parcialmente diseñado** | El requisito está definido pero no es del todo conforme con ISO/IEC 42001 | 0,25 |
| **Diseñado** | Los métodos son conformes con la norma, pero sin evidencias de aplicación | 0,50 |
| **Parcialmente implementado** | Conformes, con pocas evidencias o evidencia no continua | 0,75 |
| **Completamente implementado** | Conformes y con evidencias de aplicación permanentes | 1 |
| **NO APLICA** (C10) | Sin definición ni valor | - |

**Nivel de Implementación de Dominios** (D12:E18): las mismas seis etiquetas con las bandas 0; > 0 %-15 %; > 15 %-25 %; > 25 %-50 %; > 50 %-75 %; > 75 %-100 %, idénticas a la leyenda de la hoja Madurez.

La escala distingue **diseño** (existe el método, conforme o no) de **implementación** (hay evidencia de aplicación, continua o no), lo que se alinea con la lógica de auditoría: primero se verifica que el control está definido conforme a la norma y después que opera con eficacia. La ponderación no es lineal: pasar de "Diseñado" (50 %) a "Completamente implementado" (100 %) exige evidencias de operación sostenida.

### 4.4 Qué exige ISO/IEC 42001 6.1.3 y cómo lo cubre la plantilla

La cláusula 6.1.3 requiere que la organización defina y aplique un **proceso de tratamiento de riesgos de la IA** que, en paráfrasis: (a) seleccione opciones de tratamiento adecuadas a partir de los resultados de la evaluación de riesgos (6.1.2); (b) determine **todos los controles necesarios** para implementar las opciones elegidas; (c) compare esos controles con el Anexo A para verificar que no se ha omitido ninguno necesario (la norma aclara que el Anexo A no es exhaustivo y que pueden diseñarse controles propios o tomarse de otras fuentes); (d) elabore una **Declaración de Aplicabilidad** que contenga los controles necesarios, la justificación de su inclusión, si están implementados o no, y la **justificación de la exclusión** de cualquier control del Anexo A; (e) formule un **plan de tratamiento de riesgos**; y (f) obtenga la aprobación de los propietarios de los riesgos sobre el plan y la aceptación de los riesgos residuales. Además, el proceso debe conservarse como información documentada, y los controles se implementan con la guía del Anexo B.

| Elemento exigido por 6.1.3 d) | Cobertura en la plantilla |
|---|---|
| Lista de controles necesarios | Cubierto para los 38 controles del Anexo A. **No** contempla controles adicionales propios o de otras fuentes (habría que insertar filas; C53 no se actualiza). |
| Justificación de la inclusión | Columna G prevista, pero vacía en el ejemplo; no hay vínculo con el riesgo o requisito que motiva la inclusión. |
| Estado de implementación | Cubierto y ampliado: nivel cualitativo (I), porcentaje (J) y grado (L), más evidencia en "Documento Relacionado" (H). |
| Justificación de exclusiones | Misma columna G; la lista F permite marcar "No", pero la exclusión no se refleja en los cálculos (el control sigue promediando). |
| Vinculación con la evaluación y el plan de tratamiento de riesgos (a, b, e) | **No cubierto**: no hay columnas de riesgo asociado, opción de tratamiento, responsable ni fecha objetivo. |
| Aprobación de propietarios de riesgo y control de versiones (f, 7.5) | **No cubierto**: la plantilla no tiene campos de propietario del control, aprobador, fecha, versión ni alcance. |

En síntesis, la plantilla cumple el núcleo mínimo (controles, aplicabilidad, justificación, estado) y añade una métrica de madurez útil para el seguimiento, pero para ser una SoA auditable necesita completarse con campos de propietario, fecha/versión, referencia al riesgo tratado y una regla de exclusión de los "No aplica" en los promedios.

### 4.5 Procedimiento de uso

1. Partir del **análisis de riesgos** y de la **evaluación de impacto** (6.1.2, 6.1.4) y del diagnóstico de brechas.
2. Para cada control, decidir **Aplicabilidad**: "Si" si trata un riesgo identificado o responde a una obligación legal, contractual o de política; "No" solo si el escenario no existe (por ejemplo, A.10.3 Proveedores en una organización que desarrolla toda su IA internamente; en la práctica casi todos los controles de 42001 son aplicables a quien desarrolla o usa IA).
3. Escribir la **Justificación** con referencia explícita al riesgo, requisito o motivo de exclusión, y el **Documento Relacionado** (código y versión de la política/procedimiento/registro).
4. Fijar el **Nivel de Implementación** según la evidencia, usando las definiciones de la hoja Datos. Si el control es "No", elegir "No aplica" y recordar que la plantilla lo cuenta como 0 %.
5. Revisar los porcentajes y grados; corregir la hoja Madurez (dominios reales y `#REF!`) antes de usar el radar en informes.
6. Someter la SoA a la aprobación del responsable del SGIA y de los propietarios de riesgo; registrar versión y fecha; revisarla tras cada evaluación de riesgos, cambio significativo (6.3) o auditoría.

## 5. Conceptos y términos clave

| Término (ES) | Término (EN) | Definición breve | Dónde aparece |
|---|---|---|---|
| Declaración de Aplicabilidad | Statement of Applicability (SoA) | Documento que lista los controles necesarios, su justificación y su estado de implementación | Nombre del archivo; hoja SOA-42001 |
| Aplicabilidad | Applicability | Decisión de incluir o excluir un control del Anexo A | Columna F |
| Justificación de inclusión/exclusión | Justification for inclusion/exclusion | Motivo documentado de la decisión, exigido por 6.1.3 d) | Columna G |
| Nivel de implementación | Implementation level | Estado del control en la escala No diseñado ... Completamente implementado | Columna I; hoja Datos |
| Diseñado | Designed | Control definido conforme a la norma pero sin evidencia de aplicación (50 %) | Datos C7; fórmula J |
| Grado de implementación | Degree of implementation | Etiqueta cualitativa derivada del porcentaje | Columna L; Madurez D4:D7 |
| Madurez de dominio | Domain maturity | Promedio de los porcentajes de los controles de un dominio | Hoja Madurez; gráfico radar |
| Control necesario | Necessary control | Control determinado por el proceso de tratamiento de riesgos, del Anexo A o de otra fuente | Cláusula 6.1.3 b)-c) |
| Plan de tratamiento de riesgos | Risk treatment plan | Plan que operacionaliza los controles seleccionados; complemento de la SoA | Cláusula 6.1.3 e) (no incluido en la plantilla) |

## 6. Relación con otros documentos del corpus

- **ISO/IEC 42001 (norma y UNE)**: la plantilla operacionaliza 6.1.3 d) y el Anexo A completo. Cada fila de la SoA debería leerse junto con la guía de implementación del control correspondiente en el Anexo B (por ejemplo, B.6.2.6 para operación y monitoreo). El control A.2.2 se relaciona con la cláusula 5.2 (política de IA), A.5 con 6.1.4 y 8.4, A.3.2 con 5.3.
- **Plantilla de Diagnóstico de Brechas (archivo 08)**: misma lista de 38 controles y mismas erratas en A.7.2/A.7.3; su escala CMMI (Inexistente-Optimizado) puede mapearse a la escala de esta SoA (No diseñado-Completamente implementado) para trasladar el estado inicial.
- **Plantilla "Analisis de Riesgo SGIA v.03"**: fuente de los riesgos cuyo tratamiento justifica la inclusión de cada control; la SoA es el puente entre el análisis de riesgos y el plan de tratamiento.
- **ISO/IEC 23894**: detalla las opciones de tratamiento de riesgos de IA que se seleccionan en 6.1.3 a).
- **Guía de implementación del SGIA**: sitúa la SoA en la fase de planificación, tras la evaluación de riesgos y antes de la implementación de controles.
- **ISO 19011**: en la auditoría, la SoA es el documento de referencia para el plan de auditoría de controles; el auditor verifica coherencia entre aplicabilidad, justificación, evidencia (columna H) y estado declarado.

## 7. Aplicación práctica y puntos de examen

**Debes saber**:

1. La SoA es exigida por **6.1.3 d)** de ISO/IEC 42001 y debe contener: controles necesarios, justificación de inclusión, estado de implementación y justificación de exclusiones del Anexo A.
2. Los controles del Anexo A son de **referencia**: la organización debe compararlos con los controles que determinó necesarios (6.1.3 c)) y puede añadir controles propios. La plantilla solo lista los 38 del Anexo A.
3. El Anexo A tiene **38 controles en 9 dominios (A.2-A.10)**; los códigos empiezan en x.2 porque x.1 es el objetivo del dominio.
4. **"No aplicable" solo puede referirse a controles del Anexo A**, nunca a los requisitos de las cláusulas 4-10; y toda exclusión debe justificarse.
5. Excluir A.2.2 (Política de IA) es contradictorio con la cláusula 5.2; el dato de ejemplo de la plantilla ilustra una exclusión injustificable.
6. Diferencia entre **Diseñado** (método conforme sin evidencia) e **Implementado** (evidencia de aplicación): la auditoría de certificación busca evidencia de operación, no solo documentos.
7. La SoA se **revisa** tras cada evaluación de riesgos, cambio significativo (6.3) y como entrada de la revisión por la dirección (9.3); requiere control de versiones (7.5).
8. La SoA debe ser coherente con el **plan de tratamiento de riesgos** y con la aprobación de los propietarios de riesgo (6.1.3 e)-f)).
9. Los porcentajes de madurez son una herramienta interna de seguimiento; la norma no exige puntuaciones numéricas.
10. En la plantilla, la columna "% Implementación del Dominio" no promedia el dominio; el promedio real está en la hoja Madurez y necesita corrección.

**Errores frecuentes**: marcar "No" sin justificación; dejar vacía la columna "Documento Relacionado" (sin trazabilidad a la evidencia); calificar "Completamente implementado" sin evidencia continua; contabilizar controles no aplicables como 0 % y deprimir la madurez; usar dominios de ISO/IEC 27002 para un SGIA; no incluir controles propios fuera del Anexo A.

## 8. Limitaciones del análisis

- El análisis se basa en el volcado de texto del libro y en la inspección directa del archivo con openpyxl (fórmulas, validaciones, formato condicional, gráficos). Los colores concretos del formato condicional no se recuperaron.
- **Erratas de la fuente corregidas en el resumen**: títulos de A.7.2 y A.7.3 (copiados de A.8.2/A.8.3); "A2. Politicas" sin tilde; "Registro de registros de eventos" (A.6.2.8); "% Implement. / Del Control" con salto de línea.
- **Defectos funcionales documentados, no corregidos**: `#REF!` en Madurez C7 y C18; dominios de la hoja Madurez heredados de ISO/IEC 27002; lista desplegable de nivel con 4 de 6 valores; K no promedia el dominio; "No aplica" vale 0 %; formato condicional de K solo hasta la fila 39; C53 es una constante.
- Los textos de la columna E son paráfrasis del Anexo A realizadas por el autor de la plantilla, no el texto oficial de la norma; en este resumen se han resumido aún más por razones de derechos de autor.
- La plantilla no incluye Anexo B (guía de implementación), Anexo C (objetivos y fuentes de riesgo) ni Anexo D (sectores), ni campos de propietario, versión o fecha.
- No consta autor, fecha ni licencia del archivo.

## 9. Referencias

- ISO/IEC 42001:2023, *Information technology - Artificial intelligence - Management system*, cláusula 6.1.3 y Anexo A (citada en el encabezado C3 y en las definiciones de la hoja Datos).
- ISO/IEC 27001:2022 / ISO/IEC 27002:2022 (fuente implícita de la estructura de la plantilla y de los nombres de dominio de la hoja Madurez; no citadas expresamente).
- Plantillas relacionadas del mismo curso: "Diagnostico ISO 42001 - A.Brechas.xlsx" y "Analisis de Riesgo SGIA v.03.xls".
