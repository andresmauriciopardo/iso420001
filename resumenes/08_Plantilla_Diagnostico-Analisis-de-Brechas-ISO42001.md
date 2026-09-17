---
titulo: "Plantilla Excel: Diagnóstico ISO/IEC 42001 - Análisis de Brechas (Estado de Implementación del SGIA y de los controles del Anexo A)"
archivo_fuente: "Plantillas para ejercicios y proyectos/Diagnostico ISO 42001 - A.Brechas.xlsx"
tipo_documento: plantilla_excel
organismo_emisor: "Material didáctico del curso (Impulsa360 Academy / plantilla adaptada de un diagnóstico ISO/IEC 27001); autor no consta en el archivo"
fecha_publicacion_fuente: "no consta (referencia a ISO/IEC 42001:2023)"
idioma_fuente: es
paginas_o_hojas: "3 hojas: 'Req. Obligatorios SGIA', 'Controles Anexo A', 'Metricas'"
version_resumen: "1.0"
fecha_resumen: "2026-09-17"
generado_por: "Claude (agente Analista de Plantillas - Diagnóstico de Brechas y Declaración de Aplicabilidad)"
palabras_clave: [análisis de brechas, gap analysis, diagnóstico, SGIA, ISO/IEC 42001, Anexo A, madurez, CMMI, Inexistente, Inicial, Repetible, Definido, Administrado, Optimizado, requisitos obligatorios, documentación obligatoria, métricas, gráfico circular]
documentos_relacionados_en_corpus: ["00_INDICE.md", "01_ISO-IEC-42001-2023_Requisitos_Clausulas-4-10.md", "02_ISO-IEC-42001-2023_Anexo-A_Controles_y_Anexo-B_Guia-Implementacion.md", "13_Glosario-Bilingue-ES-EN_ISO42001_y_Nota-Version-Inglesa.md", "06_Guia-Implementacion-Inicial-ISO42001_Impulsa360.md", "09_Plantilla_Declaracion-de-Aplicabilidad-SOA-ISO42001.md", "10_Plantilla_Analisis-de-Riesgo-SGIA.md", "04_ISO-IEC-23894-2023_Gestion-de-Riesgos-de-IA.md", "05_ISO-19011-2026_Directrices-Auditoria-Sistemas-de-Gestion.md"]
---

## 1. Resumen ejecutivo

La plantilla **"Diagnostico ISO 42001 - A.Brechas.xlsx"** es una herramienta de **análisis de brechas (gap analysis)** para determinar el estado inicial de una organización frente a la norma ISO/IEC 42001:2023, antes de planificar la implementación de un **Sistema de Gestión de Inteligencia Artificial (SGIA, en inglés AIMS)**. Consta de tres hojas: la primera evalúa los **30 requisitos obligatorios** del cuerpo de la norma (cláusulas 4 a 10), la segunda evalúa los **38 controles del Anexo A** (dominios A.2 a A.10) y la tercera calcula, con fórmulas `COUNTIF`, la **proporción de requisitos y controles en cada nivel de madurez** y la representa en dos gráficos circulares.

El "Estado" de cada requisito o control se califica con una **escala de madurez de seis niveles de inspiración CMMI** (Inexistente, Inicial, Repetible, Definido, Administrado, Optimizado) más dos valores auxiliares ("? Desconocido" para lo no verificado y "No aplicable", reservado a los controles del Anexo A, ya que los requisitos de las cláusulas 4-10 no admiten exclusión). La plantilla trae datos de ejemplo: cinco requisitos y seis controles calificados en niveles distintos y el resto en "? Desconocido", lo que ilustra el aspecto de una evaluación recién iniciada.

Para el estudiante de ISO/IEC 42001 la plantilla importa por tres razones: materializa el inventario de requisitos "debe" (shall) de la norma en una lista verificable; señala explícitamente cuáles requisitos generan **información documentada obligatoria** (columna "Preguntas"); y produce la línea base cuantitativa que alimenta el plan de implementación y, más adelante, la Declaración de Aplicabilidad. La plantilla es una adaptación evidente de un diagnóstico ISO/IEC 27001, y conserva residuos de esa procedencia (referencias a "seguridad de la información", "SGSI", contadores en inglés, listas desplegables rotas) que se detallan en las secciones 4 y 8.

## 2. Identificación y contexto del documento

- **Tipo**: libro Excel (.xlsx), plantilla de trabajo para ejercicios y proyectos del curso. No es un documento normativo; es material informativo/didáctico.
- **Título interno**: celda B1 de la primera hoja, "Estado de Implementación ISO 42001"; en la segunda hoja, "Estado de Implementación ISO 42001 | ANEXO A".
- **Norma de referencia**: ISO/IEC 42001:2023 (cabecera "CONTROL ISO/IEC 42001:2023" en la hoja de controles). No se cita edición de otra norma.
- **Origen**: el archivo no indica autor ni licencia. Por su estructura (nombres definidos `CMM`, `Applicability`, `ControlTotal`, contadores con etiquetas en inglés "Non Existent", "Limited", "managed", "Not Checked", encabezados "Proporción de requerimientos SGSI" y "Controles de Seguridad de la Información") procede de una plantilla anglófona de diagnóstico ISO/IEC 27001 traducida y readaptada a 42001. El formato condicional contiene la función `_xludf.STYLE(VLOOKUP(...))`, propia de LibreOffice Calc, lo que indica que en algún momento se editó fuera de Microsoft Excel.
- **Destinatarios**: responsable de IA / líder de implementación, consultores, auditores internos y estudiantes de Lead Implementer que necesitan una línea base del SGIA.
- **Relación con otras plantillas del curso**: es el primer paso del ciclo; su salida se usa en el análisis de riesgos ("Analisis de Riesgo SGIA v.03") y en la Declaración de Aplicabilidad ("Declaración de Aplicabilidad 42001 - SOAv2").

## 3. Estructura del documento

| Hoja | Rango usado | Contenido | Fórmulas |
|---|---|---|---|
| **Req. Obligatorios SGIA** | A1:AD77 (datos en B1:G64; contadores ocultos a la vista en A67:A75) | 30 requisitos de las cláusulas 4 a 10, con columnas Sección, Requerimientos, Estado, Recurso, Preguntas, Comentarios | `D64 = COUNTA(D5:D63)` (número de requisitos) y 9 `COUNTIF` auxiliares |
| **Controles Anexo A** | A1:AD61 (datos en B1:E51) | 38 controles del Anexo A agrupados en dominios A.2 a A.10 con su objetivo, columnas Control, Estado, Comentarios | `D51 = COUNTA(D3:D50)` (número de controles) |
| **Metricas** | B2:E16 | Tabla de 8 estados con su significado y la proporción de requisitos y de controles en cada estado; dos gráficos circulares | 16 `COUNTIF(...)/total` y 2 `SUM` |

## 4. Contenido detallado

### 4.1 Hoja "Req. Obligatorios SGIA"

#### 4.1.1 Propósito y columnas

Registra, para cada requisito del cuerpo de la norma, el nivel de madurez observado. Las columnas (fila 2) son:

| Columna | Encabezado | Uso |
|---|---|---|
| B | **Sección** | Número de cláusula o subcláusula (4, 4.1, 4.2 (a), 6.1.2, 7.5.1, etc.). Las filas de cláusula (4, 5, 6...) y de subcláusula-título (4.1, 4.2...) actúan como separadores; el estado solo se califica en las filas de requisito. |
| C | **Requerimientos ISO 42001** | Paráfrasis del requisito "debe" en forma de acción ("Determinar...", "Documentar...", "Establecer..."). |
| D | **Estado** | Nivel de madurez elegido de la escala de la sección 4.3.2. |
| E | **Recurso** | Vacía en la plantilla; destinada al responsable, área o recurso asignado. |
| F | **Preguntas** | Pregunta guía de diagnóstico. En la plantilla solo contiene la marca "Documentación obligatoria / ¿Existe el documento?" en los requisitos que exigen información documentada. |
| G | **Comentarios** | Vacía; destinada a evidencias, hallazgos y observaciones. |

La celda `D64` calcula con `=COUNTA(D5:D63)` el **número de requisitos** evaluados (valor 30, rotulado en F64 "Número de requerimientos"). Este total es el denominador de las proporciones de la hoja Metricas.

#### 4.1.2 Lista completa de requisitos evaluados (30)

| Sección | Requisito (paráfrasis de la plantilla) | Marcado como documentación obligatoria |
|---|---|---|
| 4.1 | Determinar los objetivos del SGIA y los problemas (internos y externos) que puedan afectar su eficacia | - |
| 4.2 (a) | Identificar las partes interesadas, incluidas leyes, regulaciones y contratos aplicables | - |
| 4.2 (b) | Determinar los requisitos y obligaciones relevantes (la plantilla dice "de seguridad de la información"; en 42001 son requisitos relativos a la IA) | - |
| 4.3 | Determinar y documentar el alcance del SGIA | Sí |
| 4.4 | Establecer, implementar, mantener y mejorar continuamente el SGIA conforme a la norma | - |
| 5.1 | La alta dirección debe demostrar liderazgo y compromiso con el SGIA | - |
| 5.2 | Documentar la Política de IA | Sí |
| 5.3 | Asignar y comunicar roles, responsabilidades y autoridades del SGIA | - |
| 6.1.1 | Diseñar el SGIA tratando riesgos e identificando oportunidades | - |
| 6.1.2 | Definir e implementar un proceso de evaluación (análisis) de riesgos | Sí |
| 6.1.3 | Documentar e implementar un proceso de tratamiento de riesgos | Sí ("Documentación obligatoria - Controles Anexo A") |
| 6.1.4 | Establecer un proceso de **Evaluación del Impacto de los Sistemas de IA** sobre individuos, grupos y sociedad | Sí ("Documentación obligatoria - Controles Anexo A") |
| 6.2 | Establecer y documentar objetivos del SGIA y planes para alcanzarlos | Sí |
| 6.3 | Establecer y documentar el método para abordar los cambios (planificación del cambio) | Sí |
| 7.1 | Determinar y asignar los recursos necesarios | - |
| 7.2 | Determinar, documentar y hacer disponibles las competencias necesarias | Sí |
| 7.3 | Implementar un programa de concienciación en IA, incluido el uso ético | - |
| 7.4 | Determinar las necesidades de comunicación internas y externas | - |
| 7.5.1 | Proveer la documentación requerida por la norma y por la organización | - |
| 7.5.2 | Dar a los documentos título, autor, formato consistente, revisión y aprobación (creación y actualización) | - |
| 7.5.3 | Mantener un control adecuado de la documentación | - |
| 8.1 | Planificar, implementar, controlar y documentar los procesos operativos (la plantilla lo enfoca al tratamiento de riesgos) | Sí |
| 8.2 | Evaluar y documentar los riesgos regularmente y ante cambios (apreciación de riesgos de IA) | Sí |
| 8.3 | Implementar el plan de tratamiento de riesgos y documentar los resultados | Sí |
| "8.3" (segunda fila; en la norma corresponde a **8.4**) | Evaluación del impacto de los sistemas de IA: evaluar y documentar regularmente y ante cambios | Sí |
| 9.1 | Seguimiento, medición, análisis y evaluación del SGIA y de los controles | Sí |
| 9.2 | Planificar y realizar auditorías internas del SGIA | Sí |
| 9.3 | Revisión periódica del SGIA por la dirección | Sí |
| 10.1 | Mejora continua del SGIA | - |
| 10.2 | Identificar, corregir y reaccionar ante no conformidades, evitando su recurrencia y documentando las acciones | Sí |

En total, **16 de los 30 requisitos** están marcados con la pregunta de documentación obligatoria. Es la única "pregunta de diagnóstico" que trae la plantilla; el resto de la columna F está vacía para que el evaluador añada las suyas.

#### 4.1.3 Datos de ejemplo

Los cinco primeros requisitos con estado tienen valores escalonados que sirven de muestra de la escala: 4.1 = **Inexistente**, 4.2 (a) = **Inicial**, 4.2 (b) = **Repetible**, 4.3 = **Definido**, 4.4 = **Administrado**. Los otros 25 requisitos están en **"? Desconocido"**. Representan una organización en la que solo se ha revisado la cláusula 4; no hay ningún requisito en "Optimizado" ni en "No aplicable".

#### 4.1.4 Contadores auxiliares y validación

En `A67:A75` hay nueve fórmulas `COUNTIF($D$5:$D$63, "...")` con criterios en inglés ("Non Existent", "Initial", "Limited", "Defined", "managed", "Optimized", "Not Applicable", "Not Checked") y una `SUM(A67:A74)`. Como los valores de la columna D están en español, **todos devuelven 0**: son restos de la plantilla original y no intervienen en Metricas. También revelan la escala original en inglés (Non Existent / Initial / **Limited** / Defined / Managed / Optimized), donde "Limited" se tradujo como "Repetible".

La validación de datos de la columna Estado está **rota**: el rango afectado (`D31:G31, D33:G33...`, filas de subtítulo, no de requisito) no tiene tipo ni lista, y el nombre definido `CMM`, que debía contener la lista de niveles y sus colores, apunta a `#REF!`. El formato condicional busca los textos en inglés "Nonexistent" e "Initial" y una `VLOOKUP` contra `#REF!`, por lo que **no colorea** las celdas. En la práctica el usuario debe teclear el estado exactamente como aparece en la hoja Metricas (columna B) para que las fórmulas `COUNTIF` lo reconozcan.

### 4.2 Hoja "Controles Anexo A"

#### 4.2.1 Propósito y columnas

Evalúa el estado de cada uno de los **38 controles** del Anexo A de ISO/IEC 42001:2023. Columnas: B = código del control (sin el prefijo "A."), C = título del control, D = **Estado** (misma escala), E = **Comentarios**. Las filas de dominio (B3, B7, B10, B16, B21, B24, B32, B38, B43, B47, combinadas B:C) contienen el nombre del dominio y su **objetivo de control** parafraseado de la norma. `D51 = COUNTA(D3:D50)` devuelve 38 (nombre definido `ControlTotal`). La celda A1 contiene el texto huérfano "No es posible", probablemente resto de una antigua lista desplegable.

#### 4.2.2 Lista completa de controles tal como aparecen (38)

| Dominio (objetivo resumido) | Controles |
|---|---|
| **A.2 Políticas** - dirección y soporte de la gestión para los sistemas de IA | 2.2 Política de IA; 2.3 Alineación con otras políticas de la organización; 2.4 Revisión de la política de IA |
| **A.3 Organización interna** - rendición de cuentas para un enfoque responsable de la IA | 3.2 Roles y responsabilidades de la IA; 3.3 Notificación de inquietudes |
| **A.4 Recursos para sistemas de IA** - inventariar recursos para comprender riesgos e impactos | 4.2 Documentación de recursos; 4.3 Recursos de datos; 4.4 Recursos de herramientas; 4.5 Recursos de sistema e informáticos; 4.6 Recursos humanos |
| **A.5 Evaluación de los impactos de los sistemas de IA** - impactos en personas, grupos y sociedad durante el ciclo de vida | 5.2 Proceso de evaluación del impacto; 5.3 Documentación de las evaluaciones de impacto; 5.4 Evaluación del impacto en personas o grupos; 5.5 Evaluación del impacto social |
| **A.6 Ciclo de vida del sistema de IA / A.6.1 Guía de gestión para el desarrollo** - objetivos y procesos para el desarrollo responsable | 6.1.2 Objetivos para el desarrollo responsable; 6.1.3 Procesos para el diseño y desarrollo responsable |
| **A.6 / A.6.2 Ciclo de vida del sistema de IA** - criterios y requisitos para cada etapa | 6.2.2 Requisitos y especificaciones; 6.2.3 Documentación del diseño y desarrollo; 6.2.4 Verificación y validación; 6.2.5 Implementación (despliegue); 6.2.6 Operación y monitoreo; 6.2.7 Documentación técnica; 6.2.8 "Registro de registros de eventos" (registro de eventos, event logs) |
| **A.7 Datos para sistemas de IA** - comprender la función e impacto de los datos | 7.2 "Documentación del sistema e información para los usuarios" (**errata**: en la norma es *Datos para el desarrollo y mejora del sistema de IA*); 7.3 "Informes externos" (**errata**: en la norma es *Adquisición de datos*); 7.4 Calidad de los datos; 7.5 Procedencia de los datos; 7.6 Preparación de los datos |
| **A.8 Información para las partes interesadas** - información necesaria para comprender y evaluar riesgos e impactos | 8.2 Documentación del sistema e información para los usuarios; 8.3 Informes externos; 8.4 Comunicación de incidentes; 8.5 Información para las partes interesadas |
| **A.9 Uso de los sistemas de IA** - uso responsable y conforme a las políticas | 9.2 Procesos para el uso responsable; 9.3 Objetivos para el uso responsable; 9.4 Uso previsto del sistema de IA |
| **A.10 Relaciones con terceros y clientes** - responsabilidades y distribución de riesgos con terceros | 10.2 Asignación de responsabilidades; 10.3 Proveedores; 10.4 Clientes |

El recuento por dominio (3+2+5+4+2+7+5+4+3+3 = 38) coincide con el Anexo A de la norma; **no falta ni sobra ningún control**, pero los títulos de A.7.2 y A.7.3 están copiados por error de A.8.2 y A.8.3 (la misma errata aparece en la plantilla SOAv2).

#### 4.2.3 Datos de ejemplo

Seis controles muestran la escala completa: 2.2 = Inexistente, 2.3 = Inicial, 2.4 = Repetible, 3.2 = Definido, 3.3 = Administrado, 4.2 = **Optimizado**; los 32 restantes están en "? Desconocido". Aquí sí aparece el nivel "Optimizado", ausente en la hoja de requisitos. El formato condicional de la columna D presenta el mismo problema (`#REF!`, textos en inglés) que en la primera hoja y no hay validación de lista.

### 4.3 Hoja "Metricas"

#### 4.3.1 Estructura

| Columna | Encabezado | Contenido |
|---|---|---|
| B | **Estado** | Los 8 valores posibles de la escala, en el orden: ? Desconocido, Inexistente, Inicial, Repetible, Definido, Administrado, Optimizado, No aplicable |
| C | **Significado** | Definición operativa de cada nivel (ver 4.3.2) |
| D | **Proporción de requerimientos SGSI** (sic; debería decir SGIA) | `=COUNTIF('Req. Obligatorios SGIA'!$D$3:$D$63, $Bn) / 'Req. Obligatorios SGIA'!$D$64` |
| E | **Proporción de Controles de Seguridad de la Información** (sic; debería decir controles del Anexo A) | `=COUNTIF('Controles Anexo A'!$D$3:$D$50, $Bn) / 'Controles Anexo A'!$D$51` |

La fila 11 suma cada columna (`D11 = SUM(D3:D10)`, `E11 = SUM(E3:E10)`), que deben dar 1 (100 %) si todos los estados escritos coinciden exactamente con los de la columna B; una suma inferior a 1 delata errores de escritura en los estados.

#### 4.3.2 Escala de madurez y su significado

La escala es de tipo **CMMI (Capability Maturity Model Integration)** adaptada a controles de gestión. **No tiene valores numéricos** en la plantilla actual (la tabla de referencia `CMM` se perdió), aunque el orden implícito es 0 a 5. Significados según la columna C, parafraseados:

| Nivel | Significado en la plantilla |
|---|---|
| **? Desconocido** | No ha sido verificado todavía. |
| **Inexistente** (0) | El control no se lleva a cabo. |
| **Inicial** (1) | Existen salvaguardas pero no se gestionan; no hay proceso formal; el éxito depende de la suerte y de la calidad del personal. |
| **Repetible** (2) | La medida se realiza de modo informal, con procedimientos propios; la responsabilidad es individual y no hay formación. |
| **Definido** (3) | El control sigue un procedimiento documentado, pero no aprobado por el Responsable de IA ni por el Comité de Dirección. |
| **Administrado** (4) | El control sigue un procedimiento documentado, aprobado y formalizado. |
| **Optimizado** (5) | Como el anterior y, además, su eficacia se mide periódicamente mediante indicadores. |
| **No aplicable** | Nota de la plantilla: para certificar un SGIA todos los requisitos principales de ISO/IEC 42001 son obligatorios; solo los controles pueden ser descartados por la dirección (con justificación en la SoA). |

#### 4.3.3 Valores de ejemplo y gráficos

Con los datos de muestra: requisitos, 25/30 = **83,3 %** Desconocido y 3,33 % (1/30) en cada uno de Inexistente, Inicial, Repetible, Definido y Administrado; 0 % en Optimizado y No aplicable. Controles: 32/38 = **84,2 %** Desconocido y 2,63 % (1/38) en cada uno de los seis niveles; 0 % No aplicable.

La hoja incluye **dos gráficos circulares (PieChart)** sin título: el primero representa `D3:D10` (requisitos) con categorías `B3:B10`; el segundo, `E3:E10` (controles). Muestran de un vistazo el porcentaje de la norma que sigue sin evaluar o en niveles bajos.

#### 4.3.4 Elementos residuales

`B3` conserva una validación de datos vacía y un formato condicional por igualdad con `$B$3...$B$10` (que sí funciona para colorear la leyenda si se definen los estilos). El nombre definido `Applicability` apunta a `Metricas!$B$14:$B$16`, rango vacío que originalmente contenía la lista Sí/No de aplicabilidad.

### 4.4 Procedimiento de uso paso a paso

1. **Preparación**: el responsable de IA (o el consultor líder) define el alcance provisional del SGIA (sistemas de IA, procesos, sedes) y nombra a los entrevistados por cláusula: dirección (5.x, 9.3), legal/cumplimiento (4.2, A.8, A.10), datos y desarrollo (A.4, A.6, A.7), RR. HH. (7.2, 7.3, A.4.6), calidad/auditoría (9.1, 9.2, 10.x).
2. **Recopilación de evidencia**: por cada fila se revisan documentos, registros y prácticas. La columna **Preguntas** recuerda pedir el documento en los 16 requisitos con información documentada obligatoria (alcance, política de IA, procesos de riesgo, evaluación de impacto, objetivos, competencias, resultados de auditoría, revisión por la dirección, no conformidades, etc.).
3. **Calificación**: se asigna un único **Estado** por fila usando exactamente los textos de Metricas!B3:B10. Regla práctica: si hay documento no aprobado, "Definido"; aprobado y en uso, "Administrado"; con indicadores de eficacia, "Optimizado". Lo no revisado permanece en "? Desconocido"; nunca se usa "No aplicable" en las cláusulas 4-10.
4. **Registro**: en **Recurso** se anota el dueño del requisito o control y en **Comentarios** la evidencia vista (nombre y versión del documento, fecha de la entrevista, hallazgo) y la brecha concreta.
5. **Anexo A**: se repite para los 38 controles. Un control puede marcarse "No aplicable" únicamente si el análisis de riesgos y la evaluación de impacto no lo requieren; esa decisión se documentará después en la SoA.
6. **Lectura de Metricas**: se comprueba que D11 y E11 sean 100 %, se revisan los gráficos y se redacta el informe de diagnóstico (nivel de madurez global, brechas críticas, documentación faltante).
7. **Salida hacia el plan de implementación**: cada fila en Inexistente/Inicial/Repetible se convierte en una acción del plan (entregable, responsable, fecha); las de "Definido" en tareas de aprobación formal; las de "Administrado" en tareas de medición (9.1). Los requisitos con documentación obligatoria faltante definen la lista de documentos a redactar. Las calificaciones del Anexo A sirven como estado inicial de la columna "Nivel de Implementación" de la plantilla SOAv2 y como insumo para el tratamiento de riesgos (6.1.3).
8. **Re-evaluación**: la misma plantilla se vuelve a completar al cierre de cada fase para medir el avance (comparando las proporciones de Metricas entre versiones).

## 5. Conceptos y términos clave

| Término (ES) | Término (EN) | Definición breve | Dónde aparece |
|---|---|---|---|
| Análisis de brechas | Gap analysis | Comparación entre el estado actual y los requisitos de la norma para identificar lo que falta | Nombre del archivo; todas las hojas |
| SGIA | AIMS (AI Management System) | Sistema de gestión de inteligencia artificial según ISO/IEC 42001 | Hoja 1, título y requisitos |
| Requisito obligatorio | Mandatory requirement ("shall") | Exigencia de las cláusulas 4-10 no excluible para la certificación | Hoja 1; nota "No aplicable" de Metricas |
| Control del Anexo A | Annex A control | Medida de referencia para tratar riesgos; su aplicabilidad se decide en la SoA | Hoja 2 |
| Objetivo de control | Control objective | Finalidad de cada dominio A.2-A.10 | Filas de dominio de la hoja 2 |
| Madurez (escala CMMI) | Maturity (CMMI-like scale) | Grado de formalización y eficacia: Inexistente, Inicial, Repetible, Definido, Administrado, Optimizado | Metricas B3:C10 |
| Información documentada obligatoria | Mandatory documented information | Documentos y registros que la norma exige mantener o conservar | Columna Preguntas, hoja 1 |
| Evaluación del impacto del sistema de IA | AI system impact assessment | Proceso (6.1.4, 8.4, A.5) para identificar consecuencias sobre personas y sociedad | Filas 6.1.4 y "8.3" (8.4) de la hoja 1; dominio A.5 |
| Proporción de requisitos/controles | Proportion of requirements/controls | Porcentaje de filas en cada estado (COUNTIF/total) | Metricas D y E |

## 6. Relación con otros documentos del corpus

- **ISO/IEC 42001 (norma y versión UNE)**: la hoja 1 es un índice operativo de las cláusulas 4.1 a 10.2 y la hoja 2 del Anexo A (A.2 a A.10). Para interpretar cada fila conviene leer la subcláusula correspondiente en el resumen de la norma; en particular 6.1.2 (evaluación de riesgos de IA), 6.1.3 (tratamiento de riesgos y SoA), 6.1.4 y 8.4 (evaluación de impacto), 7.5 (información documentada).
- **Plantilla SOAv2 (archivo 09 de este corpus)**: comparte la misma lista de 38 controles y las mismas erratas en A.7.2/A.7.3; el estado de madurez de la hoja 2 es la entrada natural de la columna "Nivel de Implementación" de la SoA.
- **Plantilla "Analisis de Riesgo SGIA v.03"**: los controles calificados como Inexistente/Inicial señalan riesgos con tratamiento insuficiente; el análisis de riesgos, a su vez, justifica qué controles son necesarios.
- **Guía de implementación del SGIA**: el diagnóstico corresponde a la fase inicial ("análisis de situación / gap analysis") descrita en la guía, previa a la planificación.
- **ISO/IEC 23894**: el requisito 6.1.2 y el dominio A.5 se desarrollan en la guía de gestión de riesgos de IA.
- **ISO 19011**: el diagnóstico se ejecuta con técnicas de auditoría (entrevistas, revisión documental, muestreo de evidencias) y su resultado se parece a un informe de auditoría de primera parte previo a la implementación.

## 7. Aplicación práctica y puntos de examen

Uso real: el diagnóstico se realiza al inicio del proyecto (semanas 1-3), lo lidera el responsable del SGIA con apoyo de un consultor, se completa mediante entrevistas y revisión de evidencias, y su informe se presenta a la alta dirección como base para aprobar el proyecto y los recursos (5.1, 7.1). Se repite antes de la auditoría interna (9.2) para confirmar la preparación.

**Debes saber**:

1. Los requisitos de las cláusulas 4 a 10 son **todos obligatorios**; solo los controles del Anexo A pueden declararse no aplicables, y siempre con justificación en la Declaración de Aplicabilidad (6.1.3).
2. El Anexo A de ISO/IEC 42001:2023 tiene **38 controles** en **9 dominios** (A.2 a A.10); A.6 se divide en A.6.1 (2 controles) y A.6.2 (7 controles).
3. La norma exige información documentada explícita en, entre otros: alcance (4.3), política de IA (5.2), procesos de evaluación y tratamiento de riesgos (6.1.2, 6.1.3), evaluación de impacto (6.1.4/8.4), objetivos (6.2), competencias (7.2), resultados de seguimiento (9.1), auditoría interna (9.2), revisión por la dirección (9.3) y no conformidades (10.2). La plantilla marca 16 filas con esta condición.
4. En una escala CMMI, la diferencia entre **Definido** y **Administrado** es la **aprobación formal** del procedimiento; entre Administrado y **Optimizado**, la **medición de eficacia con indicadores** (enlace con 9.1).
5. "? Desconocido" no es un nivel de madurez: significa pendiente de verificar y debe tender a 0 % al terminar el diagnóstico.
6. La evaluación de impacto de los sistemas de IA (6.1.4 planificación, 8.4 operación, A.5 controles) es la novedad principal de 42001 respecto a 27001; la plantilla la trata como documentación obligatoria.
7. La salida del diagnóstico alimenta tres artefactos: plan de implementación (acciones por brecha), análisis de riesgos (controles débiles) y SoA (estado inicial de controles).
8. En auditoría, el diagnóstico es evidencia de que la organización conoció su punto de partida (contexto 4.1) y planificó con base en ello (6.1.1, 6.2).
9. Los porcentajes de Metricas son proporciones de filas, no una media ponderada de madurez; para obtener un índice de madurez habría que asignar valores 0-5 (la plantilla no los trae).

**Errores frecuentes**: escribir el estado con mayúsculas o acentos distintos a los de Metricas (la fila desaparece de las proporciones); marcar "No aplicable" en requisitos de las cláusulas 4-10; calificar "Administrado" sin haber visto el documento aprobado; confundir la columna Preguntas con un campo de respuesta; no separar la evaluación del control (Anexo A) de la del requisito (6.1.3) que obliga a tener una SoA.

## 8. Limitaciones del análisis

- El texto procede de un volcado del libro Excel con openpyxl más una inspección directa del archivo (validaciones, formatos condicionales, nombres definidos y gráficos). Los colores de las celdas no se pudieron recuperar (las reglas de formato condicional apuntan a `#REF!`).
- **Terminología residual de ISO/IEC 27001** corregida en este resumen: la plantilla dice "seguridad de la información" en 4.2 (b), 6.1.2, 6.1.3, 6.2, 8.2 y 8.3, "SGSI" en Metricas D2 y "Controles de Seguridad de la Información" en E2; en ISO/IEC 42001 se refieren a riesgos de IA, objetivos del SGIA y controles del Anexo A.
- **Erratas ortográficas** en la fuente (oblicaciones, alcande, estandar, demostrart, oportinidades, necesaraios, concianciación, aprovacion, gestion, revision) corregidas en la paráfrasis.
- **Errores de numeración**: la fila 50 titula "8.3 Evaluación del impacto en los Sistemas IA" cuando en la norma es **8.4**, y su texto de requisito repite el de 8.2. La fila 4.4 rotula la cláusula simplemente "SGIA". La numeración del Anexo A omite el prefijo "A.".
- **Erratas de títulos**: A.7.2 y A.7.3 llevan los títulos de A.8.2 y A.8.3. A.6.2.8 se traduce como "Registro de registros de eventos".
- La plantilla **no trae valores numéricos** para la escala ni colores operativos; los contadores de A67:A75 y las validaciones no funcionan. Estos hallazgos se describen tal como están; no se modificó el archivo.
- La plantilla simplifica la norma: no desglosa 9.2.1/9.2.2, 9.3.1-9.3.3 ni los apartados de 5.1 y 6.2, y no evalúa el Anexo B (guía de implementación), C (objetivos y fuentes de riesgo) ni D (sectores).
- No consta fecha, versión ni autor del archivo.

## 9. Referencias

- ISO/IEC 42001:2023, *Information technology - Artificial intelligence - Management system*, cláusulas 4 a 10 y Anexo A (citada en el encabezado de la hoja "Controles Anexo A").
- Modelo de madurez de capacidades CMMI (fuente implícita de la escala Inexistente-Optimizado; no citada expresamente en el archivo).
- Plantillas relacionadas del mismo curso: "Declaración de Aplicabilidad 42001 - SOAv2.xlsx" y "Analisis de Riesgo SGIA v.03.xls".
