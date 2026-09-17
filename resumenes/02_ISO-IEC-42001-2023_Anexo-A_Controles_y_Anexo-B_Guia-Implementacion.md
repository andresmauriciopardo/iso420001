---
titulo: "ISO/IEC 42001:2023 (UNE-ISO/IEC 42001:2025) – Anexo A: Objetivos de control y controles de referencia, y Anexo B: Orientaciones sobre la implementación de los controles de IA"
archivo_fuente: "UNE-ISO(IEC_42001=2025_en español.pdf"
tipo_documento: norma_nacional_adoptada
organismo_emisor: "UNE (Asociación Española de Normalización), adopción idéntica de ISO/IEC 42001:2023 elaborada por ISO/IEC JTC 1/SC 42"
fecha_publicacion_fuente: "2025-04"
idioma_fuente: es
paginas_o_hojas: "69 páginas en total; Anexo A en páginas 27-31 y Anexo B en páginas 32-63"
version_resumen: "1.0"
fecha_resumen: "2026-09-17"
generado_por: "Claude (agente Analista de Controles – ISO/IEC 42001 Anexos A y B)"
palabras_clave: [ISO/IEC 42001, Anexo A, Anexo B, controles de referencia, objetivos de control, declaración de aplicabilidad, tratamiento del riesgo de la IA, política de IA, evaluación del impacto del sistema de IA, ciclo de vida del sistema de IA, datos para sistemas de IA, uso responsable de la IA, proveedores, clientes, supervisión humana, registros de eventos, SGIA, AIMS]
documentos_relacionados_en_corpus: ["00_INDICE.md", "01_ISO-IEC-42001-2023_Requisitos_Clausulas-4-10.md", "03_ISO-IEC-42001-2023_Anexos-C-D_Objetivos-Fuentes-de-Riesgo-y-Sectores.md", "13_Glosario-Bilingue-ES-EN_ISO42001_y_Nota-Version-Inglesa.md", "04_ISO-IEC-23894-2023_Gestion-de-Riesgos-de-IA.md", "05_ISO-19011-2026_Directrices-Auditoria-Sistemas-de-Gestion.md", "06_Guia-Implementacion-Inicial-ISO42001_Impulsa360.md", "08_Plantilla_Diagnostico-Analisis-de-Brechas-ISO42001.md", "09_Plantilla_Declaracion-de-Aplicabilidad-SOA-ISO42001.md", "10_Plantilla_Analisis-de-Riesgo-SGIA.md"]
---

## 1. Resumen ejecutivo

Los Anexos A y B de ISO/IEC 42001:2023 (adoptada en España como UNE-ISO/IEC 42001:2025, texto idéntico en traducción oficial) constituyen el catálogo de **controles de referencia** del Sistema de Gestión de la Inteligencia Artificial (SGIA; en inglés *AI Management System, AIMS*). El **Anexo A (normativo)** contiene la Tabla A.1 con **9 dominios de control (A.2 a A.10)**, cada uno con un **objetivo de control** y un conjunto de controles, hasta un total de **38 controles**. El **Anexo B (normativo)** ofrece, control por control, una **orientación sobre la implementación** (equivalente funcional de ISO/IEC 27002 respecto a ISO/IEC 27001) y, en muchos casos, un apartado de "Otra información" con referencias cruzadas a otras normas ISO/IEC (23894, 22989, 5338, 38507, 27001, 27701, 29100, 37002, 5259, 24027, 24029-1, 24368, 25024, 25059, 23053, 19944-1, TS 4213, ISO 8000-2 e ISO 9241-210).

Su importancia práctica es doble. Primero, la **cláusula 6.1.3 (Tratamiento del riesgo de la IA)** exige comparar los controles que la organización determine necesarios con los del Anexo A para verificar que no se omite ninguno, y elaborar una **Declaración de Aplicabilidad** (DdA; *Statement of Applicability, SoA*) que justifique la inclusión o exclusión de cada control. Segundo, los controles del Anexo A no son obligatorios en sí mismos ni exhaustivos: la organización puede excluirlos con justificación, diseñar controles propios o tomarlos de otras fuentes; el Anexo B, en cambio, nunca debe reflejarse en la DdA, pues es solo guía.

Los 38 controles cubren toda la cadena de gobierno de la IA: políticas (A.2), organización interna y canal de inquietudes (A.3), inventario de recursos (A.4), evaluación del impacto sobre personas, grupos y sociedad (A.5), ciclo de vida completo desde objetivos de desarrollo responsable hasta registros de eventos (A.6), gestión de datos (A.7), información y comunicación a partes interesadas (A.8), uso responsable y supervisión humana (A.9) y relaciones con proveedores y clientes (A.10). Para quien prepara una certificación Lead Implementer o Lead Auditor, dominar los Anexos A y B es imprescindible: son el objeto de la fase 2 de la auditoría de certificación y el contenido central de la DdA.

## 2. Identificación y contexto del documento

- **Norma**: UNE-ISO/IEC 42001:2025, "Tecnología de la información. Inteligencia artificial. Sistema de gestión", publicada por UNE en abril de 2025. La portada declara que "esta norma es idéntica a la Norma Internacional ISO/IEC 42001:2023". Fue elaborada por el comité técnico CTN-UNE 71 Tecnologías habilitadoras digitales (THD).
- **Organismo internacional de origen**: ISO/IEC JTC 1/SC 42 (Inteligencia artificial). Primera edición internacional: diciembre de 2023.
- **Ámbito de este resumen**: Anexo A (Normativo) "Objetivos de control y controles de referencia" (páginas 27-31) y Anexo B (Normativo) "Orientaciones sobre la implementación de los controles de IA" (páginas 32-63). Las cláusulas 4 a 10 y los Anexos C y D se tratan en otros resúmenes del corpus.
- **Estado normativo**: ambos anexos se denominan "normativos". Sin embargo, la propia norma matiza su carácter: los controles del Anexo A son de referencia (A.1 y 6.1.3 NOTA 2) y el Anexo B es orientación cuya inclusión o exclusión no ha de justificarse en la DdA (B.1).
- **Lenguaje verbal**: en la Tabla A.1 los controles se redactan con "debe" (*shall*), que aplica cuando el control se ha seleccionado como necesario en la DdA. En el Anexo B, el mismo control se reformula con "debería" (*should*) y toda la orientación es recomendación. Excepciones: dentro de A.6.2.6 y A.6.2.8 el propio enunciado combina "debe" para la obligación principal y "debería" para el contenido mínimo, tal como ocurre en el original inglés; y B.7.6 conserva un "debe" que parece una inconsistencia de traducción (véase sección 8).
- **Relación con otras normas**: el Anexo A sigue el modelo de ISO/IEC 27001 (Anexo A + Declaración de Aplicabilidad), y el Anexo B el de ISO/IEC 27002. La gestión de riesgos de IA que alimenta la selección de controles se desarrolla en ISO/IEC 23894. Los conceptos de ciclo de vida y de recursos proceden de ISO/IEC 22989 y los procesos de ciclo de vida de ISO/IEC 5338.
- **Licencia**: "© UNE 2025. Prohibida la reproducción sin el consentimiento de UNE". El ejemplar fue adquirido por la Universidad Antonio de Nebrija a través de la suscripción AENORmás; para uso en red interna se requiere autorización previa de AENOR. Por ello este resumen parafrasea y solo cita frases breves.
- **Destinatarios**: organizaciones que desarrollan, proveen o usan sistemas de IA en cualquier rol (proveedor, productor, cliente, socio, tercero), implementadores del SGIA, auditores internos y de certificación, y responsables de gobierno de la IA.

## 3. Estructura del documento

### 3.1 Estructura general de los Anexos A y B

| Apartado | Título | Descripción |
|---|---|---|
| A.1 | Generalidades | Los controles son referencia para cumplir objetivos y tratar riesgos; no es obligatorio usarlos todos; la organización puede diseñar los suyos (remite a 6.1.3); el Anexo B da orientación para todos. |
| Tabla A.1 | Objetivos de control y controles | Nueve dominios A.2 a A.10, cada uno con objetivo, y columnas "Tema" y "Control". |
| B.1 | Generalidades | La orientación apoya la implementación; no se documenta ni justifica en la DdA; puede ampliarse, modificarse o sustituirse; es un "punto de partida". |
| B.2 a B.10 | Orientaciones por dominio | Para cada control: enunciado ("Control"), "Orientación sobre la implementación" y, cuando procede, "Otra información" con referencias. |

### 3.2 Los nueve dominios de control del Anexo A

| Dominio | Título | Objetivo de control (parafraseado) | Controles | Nº |
|---|---|---|---|---|
| A.2 | Políticas relativas a la IA | Dar dirección y apoyo de la dirección a los sistemas de IA conforme a los requisitos del negocio. | A.2.2, A.2.3, A.2.4 | 3 |
| A.3 | Organización interna | Establecer la rendición de cuentas interna para mantener un enfoque responsable en la implementación, operación y gestión de sistemas de IA. | A.3.2, A.3.3 | 2 |
| A.4 | Recursos para los sistemas de IA | Asegurar que la organización contabiliza los recursos del sistema de IA (componentes y activos) para comprender y abordar riesgos e impactos. | A.4.2 a A.4.6 | 5 |
| A.5 | Evaluación de los impactos de los sistemas de IA | Evaluar los impactos del sistema de IA sobre personas, grupos de personas y sociedad durante todo su ciclo de vida. | A.5.2 a A.5.5 | 4 |
| A.6 | Ciclo de vida del sistema de IA | A.6.1: identificar y documentar objetivos e implementar procesos para el diseño y desarrollo responsables. A.6.2: definir criterios y requisitos para cada etapa del ciclo de vida. | A.6.1.2, A.6.1.3; A.6.2.2 a A.6.2.8 | 9 (2 + 7) |
| A.7 | Datos para sistemas de IA | Asegurar que la organización comprende el rol y los impactos de los datos en el desarrollo, provisión y uso de sistemas de IA. | A.7.2 a A.7.6 | 5 |
| A.8 | Información para las partes interesadas de los sistemas de IA | Asegurar que las partes interesadas disponen de la información necesaria para comprender y evaluar riesgos e impactos (positivos y negativos). | A.8.2 a A.8.5 | 4 |
| A.9 | Uso de sistemas de IA | Asegurar que la organización usa los sistemas de IA de forma responsable y según sus políticas. | A.9.2, A.9.3, A.9.4 | 3 |
| A.10 | Relaciones con terceros y clientes | Asegurar que la organización comprende sus responsabilidades, sigue rindiendo cuentas y distribuye los riesgos cuando intervienen terceros en cualquier etapa. | A.10.2, A.10.3, A.10.4 | 3 |
| **Total** | | | | **38** |

Nota sobre la numeración: dentro de cada dominio el subapartado ".1" está reservado al objetivo (por ejemplo, B.2.1 Objetivo), de modo que el primer control de cada dominio es siempre ".2" (A.2.2, A.3.2, etc.). El dominio A.6 es el único con dos subdominios (A.6.1 y A.6.2), cada uno con su propio objetivo, por lo que sus controles tienen cuatro niveles (A.6.1.2, A.6.2.2, ...).

## 4. Contenido detallado

### 4.1 Naturaleza de los controles y relación con la cláusula 6.1.3 y la Declaración de Aplicabilidad

**A.1 Generalidades** establece que los controles de la Tabla A.1 "proporcionan a la organización una referencia" para cumplir sus objetivos y abordar los riesgos del diseño y operación de sistemas de IA. Dos ideas clave: no se requiere utilizar todos los objetivos de control ni todos los controles, y la organización puede diseñar e implementar sus propios controles. El Anexo B facilita orientación para todos los controles de la tabla.

**B.1 Generalidades** añade que la orientación de implementación: (i) apoya la implementación de los controles y el logro del objetivo de control; (ii) **no tiene que documentarse ni justificarse en la declaración de aplicabilidad**; (iii) no siempre es adecuada o suficiente y la organización puede ampliarla, modificarla o definir su propia implementación según sus necesidades de tratamiento del riesgo; (iv) es un punto de partida para desarrollar implementaciones específicas.

La conexión con el cuerpo de la norma se produce a través de la **cláusula 6.1.3 Tratamiento del riesgo de la IA**, que exige (requisito, "debe") definir un proceso de tratamiento del riesgo para:

- a) seleccionar las opciones de tratamiento del riesgo de la IA;
- b) determinar todos los controles necesarios para implementarlas y **compararlos con los del Anexo A** para verificar que no se ha omitido ninguno necesario (NOTA 1: el Anexo A proporciona controles de referencia);
- c) considerar los controles del Anexo A pertinentes;
- d) identificar si hacen falta **controles adicionales** a los del Anexo A;
- e) considerar las orientaciones del **Anexo B** para implementar los controles de b) y c) (NOTA 2: los objetivos de control están implícitos en los controles elegidos; el Anexo A no es exhaustivo; los controles adicionales pueden diseñarse o tomarse de fuentes existentes; la gestión del riesgo de la IA puede integrarse en otros sistemas de gestión);
- f) elaborar una **declaración de aplicabilidad** con los controles necesarios y **justificar la inclusión o exclusión** de controles; la exclusión puede justificarse porque la evaluación del riesgo no los considera necesarios o porque los requisitos externos no los exigen o contemplan excepciones (NOTA 3: la justificación de exclusión puede referirse a objetivos de control en general o a sistemas de IA específicos);
- g) formular un **plan de tratamiento del riesgo de la IA**.

Además, la dirección designada debe aprobar el plan de tratamiento y la aceptación de los riesgos residuales, y los controles necesarios deben alinearse con los objetivos de IA (6.2), estar disponibles como información documentada, comunicarse internamente y estar disponibles para las partes interesadas según corresponda. Las cláusulas 8.1 (planificación y control operacional) y 8.3 (tratamiento del riesgo de la IA) exigen implementar los controles establecidos según 6.1.3 y aplicar el plan de tratamiento.

En síntesis, el flujo lógico es: **evaluación del riesgo (6.1.2) y evaluación del impacto (6.1.4) → opciones de tratamiento → determinación de controles → contraste con Anexo A → controles adicionales si procede → DdA con justificaciones → plan de tratamiento aprobado → implementación (8.1, 8.3) usando Anexo B como guía**. El Anexo A alimenta la DdA; el Anexo B alimenta los procedimientos y evidencias, pero no la DdA.

### 4.2 Recorrido control por control (Anexo A fusionado con Anexo B)

Convenciones de este apartado. Para cada control se indica: (a) el enunciado del control parafraseado (Tabla A.1; en el Anexo B se repite con "debería"); (b) la orientación de implementación del Anexo B; (c) "Otra información" y referencias que cita el Anexo B; (d) evidencia típica que solicitaría un auditor. El punto (d) es una aportación del analista para fines didácticos y no figura en la norma; la norma solo exige información documentada allí donde lo dice expresamente el control o las cláusulas 4-10.

### 4.3 Dominio A.2 / B.2 – Políticas relativas a la IA

Objetivo (B.2.1): proporcionar dirección y apoyo de la dirección para los sistemas de IA de acuerdo con los requisitos de negocio.

#### A.2.2 Política de IA

(a) **Control**: la organización debe documentar una política para el desarrollo o el uso de sistemas de IA.

(b) **Orientación B.2.2**:
- La política de IA debería basarse en: la estrategia de negocio; los valores y cultura de la organización y la cantidad de riesgo que está dispuesta a asumir (apetito de riesgo); el nivel de riesgo que representan los sistemas de IA; los requisitos legales, incluidos contratos; el entorno de riesgo de la organización; y el impacto para las partes interesadas (remite a 6.1.4).
- Además de lo exigido en la cláusula 5.2, la política debería incluir: los principios que guían todas las actividades de IA de la organización y los procesos para gestionar desviaciones y excepciones de la política.
- Debería tratar aspectos específicos por tema cuando sea necesario, o remitir a otras políticas que los cubran; ejemplos: recursos y activos de IA, evaluaciones del impacto de los sistemas de IA (6.1.4) y desarrollo de sistemas de IA.
- Las políticas pertinentes deberían guiar el desarrollo, la compra, la operación y el uso de sistemas de IA.

(c) **Otra información**: no hay apartado específico; el control se vincula con la cláusula 5.2 (Política de IA) y 6.1.4.

(d) **Evidencia típica**: política de IA aprobada y fechada, con principios, alcance (desarrollo/uso), referencia al apetito de riesgo, mecanismo de excepciones, evidencia de comunicación y de disponibilidad para partes interesadas.

#### A.2.3 Alineamiento con otras políticas organizacionales

(a) **Control**: la organización debe determinar cuándo otras políticas pueden aplicarse a sus objetivos respecto a los sistemas de IA, o verse afectadas por ellos.

(b) **Orientación B.2.3**:
- Muchos dominios se entrecruzan con la IA: calidad, seguridad (*security*), protección (*safety*) y privacidad.
- La organización debería realizar un análisis exhaustivo para determinar si las políticas actuales se entrecruzan y en qué aspectos, y o bien actualizarlas o bien incluir disposiciones al respecto en la política de IA.

(c) **Otra información**: la política de IA debería fundamentarse en las políticas que establezca el órgano de gobierno. **ISO/IEC 38507** proporciona orientación a los miembros del órgano de gobierno para posibilitar y gobernar el uso de IA durante todo su ciclo de vida.

(d) **Evidencia típica**: matriz de alineamiento entre la política de IA y las políticas de seguridad de la información, privacidad, calidad, ética, adquisiciones y RRHH; registros de revisión o actualización de esas políticas; actas del órgano de gobierno.

#### A.2.4 Revisión de la política de IA

(a) **Control**: la política de IA debe revisarse a intervalos planificados, o con mayor frecuencia si es necesario, para asegurar su idoneidad, adecuación y eficacia continuas.

(b) **Orientación B.2.4**:
- Un rol aprobado por la dirección debería ser responsable del desarrollo, revisión y evaluación de la política o de sus componentes.
- La revisión debería evaluar oportunidades de mejora de las políticas y del enfoque de gestión de la IA en respuesta a cambios del entorno organizacional, circunstancias de negocio, condiciones legales o entorno técnico.
- La revisión debería tener en cuenta los resultados de las revisiones por la dirección (cláusula 9.3).

(c) **Otra información**: no aplica.

(d) **Evidencia típica**: designación documentada del propietario de la política, calendario de revisión, registros de revisiones con fecha y cambios, vínculo con actas de revisión por la dirección.

### 4.4 Dominio A.3 / B.3 – Organización interna

Objetivo (B.3.1): establecer la rendición de cuentas dentro de la organización para mantener un enfoque responsable en la implementación, operación y gestión de sistemas de IA.

#### A.3.2 Roles y responsabilidades en materia de IA

(a) **Control**: los roles y responsabilidades de IA deben definirse y asignarse según las necesidades de la organización.

(b) **Orientación B.3.2**:
- La definición de roles es crucial para la rendición de cuentas durante todo el ciclo de vida.
- Al asignarlos, considerar las políticas de IA, los objetivos de IA y los riesgos identificados para cubrir todas las áreas pertinentes; se puede priorizar.
- Áreas que pueden requerir roles definidos: gestión del riesgo; evaluaciones del impacto del sistema de IA; gestión de activos y recursos; seguridad (*security*); protección (*safety*); privacidad; desarrollo; rendimiento; supervisión humana; relaciones con proveedores; demostración de la capacidad de cumplir los requisitos legales de forma consistente; gestión de la calidad de los datos en todo el ciclo de vida.
- Las responsabilidades deberían definirse hasta el nivel de detalle necesario para que las personas desempeñen sus funciones.

(c) **Otra información**: no aplica; se relaciona con la cláusula 5.3.

(d) **Evidencia típica**: organigrama o matriz RACI de IA, descripciones de puesto, nombramientos (por ejemplo, responsable del SGIA, comité de ética o gobierno de IA), evidencia de comunicación de roles.

#### A.3.3 Notificación de inquietudes

(a) **Control**: la organización debe definir y poner en marcha un proceso para notificar inquietudes sobre su rol respecto a un sistema de IA durante todo el ciclo de vida.

(b) **Orientación B.3.3** – el mecanismo debería:
- a) ofrecer opciones de confidencialidad, anonimato o ambas;
- b) estar disponible y promoverse entre empleados y personal temporal o contratado;
- c) estar dotado de personal cualificado;
- d) otorgar a ese personal facultades de investigación y resolución;
- e) permitir notificar y escalar a la dirección de forma oportuna;
- f) proteger eficazmente frente a represalias a quienes notifican y a quienes investigan;
- g) generar notificaciones conforme a 4.4 (y a e) si procede) respetando la confidencialidad y el anonimato y la confidencialidad empresarial;
- h) responder en plazos apropiados.
- NOTA: pueden reutilizarse mecanismos de notificación ya existentes.

(c) **Otra información**: la organización debería tener en cuenta **ISO 37002** (sistemas de gestión de denuncias de irregularidades, *whistleblowing*).

(d) **Evidencia típica**: procedimiento de canal de denuncias o inquietudes, evidencia de su difusión, registro de casos (anonimizado), tiempos de respuesta, política antirrepresalias, formación del personal que gestiona el canal.

### 4.5 Dominio A.4 / B.4 – Recursos para los sistemas de IA

Objetivo (B.4.1): asegurar que la organización contabiliza los recursos del sistema de IA (incluidos componentes y activos) para comprender y abordar plenamente riesgos e impactos.

#### A.4.2 Documentación de los recursos

(a) **Control**: identificar y documentar los recursos pertinentes que se requieren en las etapas del ciclo de vida del sistema de IA y en otras actividades de IA relevantes.

(b) **Orientación B.4.2**:
- La documentación de recursos es crucial para comprender riesgos e impactos (positivos y negativos) sobre personas, grupos y sociedad; las evaluaciones del impacto (B.5) pueden basarse en ella (por ejemplo, con diagramas de flujo de datos o de arquitectura).
- Categorías de recursos (no exhaustivas): componentes del sistema de IA; recursos de datos (datos usados en cualquier etapa); recursos de herramientas (algoritmos, modelos, herramientas de IA); recursos de sistema y computación (hardware, almacenamiento); recursos humanos (personas con conocimientos para desarrollo, ventas, entrenamiento, operación, mantenimiento).
- Los recursos pueden ser provistos por la organización, sus clientes o terceros.

(c) **Otra información**: la documentación ayuda a determinar si hay recursos disponibles; si no, la organización debería revisar el diseño de la especificación o los requisitos de despliegue.

(d) **Evidencia típica**: inventario o registro de activos de IA por sistema, diagramas de arquitectura y de flujo de datos, vínculo con el registro de riesgos y con las evaluaciones de impacto.

#### A.4.3 Recursos de datos

(a) **Control**: como parte de la identificación de recursos, documentar información sobre los recursos de datos usados por el sistema de IA.

(b) **Orientación B.4.3** – la documentación debería incluir, sin carácter exhaustivo:
- procedencia de los datos;
- fecha de última actualización o modificación (por ejemplo, etiqueta de fecha en metadatos);
- para aprendizaje automático, categorías por función: entrenamiento, validación, prueba y producción;
- categorías de datos (por ejemplo, según **ISO/IEC 19944-1**);
- proceso de etiquetado;
- uso previsto de los datos;
- calidad de los datos (por ejemplo, según la serie **ISO/IEC 5259**, en elaboración al publicarse la norma);
- políticas aplicables de retención y eliminación;
- sesgos conocidos o potenciales;
- preparación de los datos.

(c) **Otra información**: no hay apartado; las referencias están integradas en la orientación.

(d) **Evidencia típica**: fichas de conjuntos de datos (*datasheets*), catálogo de datos con metadatos, políticas de retención, registros de etiquetado, análisis de sesgo.

#### A.4.4 Recursos de herramientas

(a) **Control**: documentar información sobre los recursos de herramientas utilizados por el sistema de IA.

(b) **Orientación B.4.4** – las herramientas, en particular para aprendizaje automático, pueden incluir: tipos de algoritmos y modelos; herramientas o procesos de acondicionamiento de datos; métodos de optimización; métodos de evaluación; herramientas de aprovisionamiento de recursos; herramientas de ayuda al desarrollo de modelos; software y hardware para diseño, desarrollo y despliegue.

(c) **Otra información**: **ISO/IEC 23053** ofrece orientación detallada sobre tipos, métodos y enfoques de herramientas para aprendizaje automático.

(d) **Evidencia típica**: inventario de frameworks, librerías, modelos (propios y de terceros, incluidos modelos fundacionales), versiones, licencias, pipelines de MLOps, herramientas de evaluación.

#### A.4.5 Recursos del sistema y de computación

(a) **Control**: documentar información sobre los recursos de sistema y computación utilizados por el sistema de IA.

(b) **Orientación B.4.5** – puede incluir:
- requisitos de recursos del sistema (por ejemplo, para asegurar la ejecución en dispositivos con recursos limitados);
- ubicación de los recursos: instalaciones propias (*on-premises*), nube o computación periférica (*edge computing*);
- recursos de procesamiento, incluidos red y almacenamiento;
- impacto del hardware usado para ejecutar las cargas de trabajo (impacto ambiental por uso o fabricación, coste de utilización).
- Pueden requerirse recursos distintos para la mejora continua; desarrollo, despliegue y operación tienen necesidades diferentes.

(c) **Otra información**: NOTA: **ISO/IEC 22989** describe consideraciones sobre los recursos del sistema.

(d) **Evidencia típica**: inventario de infraestructura (clústeres, GPU, servicios cloud, regiones), acuerdos con proveedores cloud, estimaciones de consumo energético o huella de carbono, requisitos de capacidad.

#### A.4.6 Recursos humanos

(a) **Control**: documentar los recursos humanos y las competencias que intervienen en todas las etapas del sistema de IA (desarrollo, despliegue, operación, gestión de cambios, mantenimiento, transferencia y retirada del servicio, además de la verificación e integración).

(b) **Orientación B.4.6**:
- Considerar la necesidad de conocimientos y experiencia diversos e incluir los tipos de roles necesarios; por ejemplo, incluir grupos demográficos específicos relacionados con los conjuntos de datos de entrenamiento si es un componente necesario del diseño.
- Recursos humanos posibles: científicos de datos; roles de supervisión humana; expertos en fiabilidad (seguridad, protección, privacidad); investigadores y especialistas en IA y expertos de dominio.
- Pueden requerirse recursos distintos en cada etapa del ciclo de vida.

(c) **Otra información**: no aplica; se relaciona con la cláusula 7.2 (Competencia).

(d) **Evidencia típica**: matriz de competencias por rol y etapa del ciclo de vida, planes de formación, registros de competencia, contratos con expertos externos.

### 4.6 Dominio A.5 / B.5 – Evaluación de los impactos de los sistemas de IA

Objetivo (B.5.1): evaluar los impactos del sistema de IA sobre las personas, los grupos de personas y la sociedad afectados durante todo su ciclo de vida. Este dominio operacionaliza la cláusula 6.1.4 (Evaluación del impacto del sistema de IA) y la cláusula 8.4.

#### A.5.2 Proceso de evaluación del impacto del sistema de IA

(a) **Control**: establecer un proceso para evaluar las consecuencias potenciales para personas, grupos y sociedad derivadas del sistema de IA durante todo su ciclo de vida.

(b) **Orientación B.5.2**:
- La evaluación se basa en el propósito y uso previstos. Considerar si el sistema afecta a: la situación legal u oportunidades vitales de las personas; su bienestar físico o psicológico; los derechos humanos universales; la sociedad.
- Los procedimientos deberían incluir: a) las **circunstancias** en que se realiza una evaluación (criticidad del propósito y contexto; complejidad de la tecnología y nivel de automatización; sensibilidad de los tipos y fuentes de datos; y cambios significativos en cualquiera de ellos); b) los **elementos** del proceso: identificación (fuentes, eventos, resultados), análisis (consecuencias y probabilidad), evaluación (aceptación y priorización), tratamiento (mitigaciones) y documentación, notificación y comunicación (7.4, 7.5 y B.3.3); c) **quién** realiza la evaluación; d) **cómo se usa** (sustentar el diseño o el uso, B.6 y B.9; activar revisiones y aprobaciones); e) las personas y la sociedad potencialmente impactadas.
- Debería considerar los datos usados, las tecnologías de IA empleadas y la funcionalidad del sistema en conjunto. Los procesos pueden variar según rol, dominio de aplicación y disciplina (seguridad, privacidad, protección).

(c) **Otra información**: en algunas disciplinas (seguridad de la información, protección, gestión ambiental) la consideración del impacto forma parte de la gestión del riesgo; la organización debería determinar si esas evaluaciones integran suficientemente las consideraciones de IA. NOTA: **ISO/IEC 23894** describe cómo realizar análisis de impacto sobre la organización junto con evaluaciones sobre personas, grupos y sociedad dentro de la gestión del riesgo.

(d) **Evidencia típica**: procedimiento de evaluación del impacto de sistemas de IA (AISIA), criterios de activación, plantilla, roles responsables, integración con el proceso de gestión del riesgo y con las evaluaciones de impacto de privacidad (PIA/DPIA).

#### A.5.3 Documentación de las evaluaciones del impacto del sistema de IA

(a) **Control**: documentar los resultados de las evaluaciones del impacto y conservarlos durante un periodo determinado.

(b) **Orientación B.5.3**:
- La documentación ayuda a decidir qué información comunicar a usuarios y otras partes interesadas.
- Las evaluaciones deberían conservarse y actualizarse según sea necesario, alineadas con los elementos de B.5.2; los plazos de conservación pueden seguir el cronograma de la organización o requisitos legales.
- Elementos a documentar: uso previsto y uso indebido razonablemente previsible; impactos positivos y negativos sobre personas, grupos y sociedad; fallos predecibles, sus impactos y mitigaciones; grupos demográficos a los que aplica; complejidad del sistema; papel del humano (supervisión humana, procesos y herramientas); empleo y capacitación del personal.

(c) **Otra información**: no aplica.

(d) **Evidencia típica**: informes de evaluación de impacto completados por sistema, con versión y fecha; política de retención aplicable; registro de actualizaciones tras cambios significativos.

#### A.5.4 Evaluación del impacto del sistema de IA sobre las personas o grupos de personas

(a) **Control**: evaluar y documentar los impactos potenciales sobre personas o grupos de personas durante todo el ciclo de vida.

(b) **Orientación B.5.4**:
- Considerar los principios de gobierno, políticas y objetivos de IA. Las personas que usan el sistema o cuya **IIP** (información de identificación personal; *PII*) procesa pueden tener expectativas de fiabilidad. Considerar necesidades especiales de protección de niños, personas con discapacidad, personas mayores y trabajadores.
- Áreas de impacto (no exhaustivas): equidad; rendición de cuentas; transparencia y explicabilidad; seguridad y privacidad; seguridad y salud; consecuencias financieras; accesibilidad; derechos humanos.

(c) **Otra información**: cuando sea necesario, consultar a expertos (investigadores, expertos en la materia, usuarios) para comprender plenamente los impactos.

(d) **Evidencia típica**: sección de impactos individuales en la AISIA, análisis de equidad por grupo demográfico, consultas a expertos o a usuarios, medidas de accesibilidad.

#### A.5.5 Evaluación de los impactos sociales de los sistemas de IA

(a) **Control**: evaluar y documentar los impactos sociales potenciales de los sistemas de IA durante todo su ciclo de vida.

(b) **Orientación B.5.5** – los impactos sociales pueden ser beneficiosos o perjudiciales; ejemplos:
- sostenibilidad ambiental (recursos naturales, emisiones de gases de efecto invernadero);
- económicos (acceso a servicios financieros, empleo, impuestos, comercio e industria);
- gobierno (procesos legislativos, desinformación con fines políticos, seguridad nacional, justicia penal);
- salud y seguridad (acceso a atención sanitaria, diagnóstico y tratamiento, daños físicos y psicológicos);
- normas, tradiciones, cultura y valores (desinformación que genera sesgos o perjuicios).

(c) **Otra información**:
- La IA es intensiva en computación, con impactos ambientales (energía, agua, terreno, flora y fauna), pero también puede mejorar la sostenibilidad de otros sistemas (edificios, transporte); considerar los impactos dentro de la estrategia de sostenibilidad de la organización.
- Considerar el uso indebido para causar daños sociales y el uso para reparar daños históricos (ejemplo: acceso a préstamos, becas, seguros e inversiones).
- Ejemplos de riesgo: influencia en elecciones y desinformación (ultrafalsos, *deepfakes*); sesgos en el uso gubernamental para justicia penal; escenarios en que un fallo puede causar muerte o lesiones (vehículos autónomos, equipos humano-máquina).
- NOTA: **ISO/IEC TR 24368** ofrece una visión de las inquietudes éticas y sociales de la IA.

(d) **Evidencia típica**: sección de impactos sociales en la AISIA, análisis de huella ambiental, análisis de escenarios de uso indebido, alineamiento con objetivos ESG o de sostenibilidad.

### 4.7 Dominio A.6 / B.6 – Ciclo de vida del sistema de IA

#### Subdominio A.6.1 – Orientación sobre la gestión para el desarrollo de sistemas de IA

Objetivo (B.6.1.1): asegurar que la organización identifica y documenta objetivos e implementa procesos para el diseño y desarrollo responsables de sistemas de IA.

#### A.6.1.2 Objetivos para el desarrollo responsable del sistema de IA

(a) **Control**: identificar y documentar objetivos que guíen el desarrollo responsable, tenerlos en cuenta e integrar medidas para lograrlos en el ciclo de vida de desarrollo.

(b) **Orientación B.6.1.2**:
- Identificar objetivos (véase 6.2) que afecten a los procesos de diseño y desarrollo y considerarlos en dichos procesos. Ejemplo: si la "equidad" es un objetivo, debería incorporarse en la especificación de requisitos, la adquisición y acondicionamiento de datos, el entrenamiento, la verificación y la validación.
- Proporcionar requisitos y directrices para integrar medidas en las distintas etapas (por ejemplo, exigir una herramienta o método de prueba específico para abordar la falta de equidad o el sesgo indeseado).

(c) **Otra información**: las técnicas de IA se usan para reforzar la seguridad (predicción de amenazas, detección y prevención de ataques), tanto en sistemas de IA como convencionales. El **Anexo C** ofrece ejemplos de objetivos organizacionales para la gestión del riesgo útiles para fijar objetivos de desarrollo.

(d) **Evidencia típica**: objetivos de desarrollo responsable documentados y trazados a los objetivos del SGIA (6.2), directrices de ingeniería que los integran, evidencias de pruebas de equidad o robustez exigidas por esas directrices.

#### A.6.1.3 Procesos para el diseño y desarrollo responsables de sistemas de IA

(a) **Control**: definir y documentar los procesos específicos para el diseño y desarrollo responsables del sistema de IA.

(b) **Orientación B.6.1.3** – los procesos deberían considerar, sin carácter exhaustivo:
- las etapas del ciclo de vida (**ISO/IEC 22989** da un modelo genérico, pero la organización puede definir las suyas);
- requisitos de pruebas y medios planificados para ellas;
- requisitos de supervisión humana (procesos y herramientas), especialmente si el sistema puede impactar a personas físicas;
- en qué etapas realizar evaluaciones del impacto;
- expectativas y reglas para datos de entrenamiento (datos permitidos, proveedores aprobados, etiquetado);
- conocimientos, experiencia o formación requeridos a los desarrolladores;
- criterios de lanzamiento;
- aprobaciones y firmas en las distintas etapas;
- control de cambios;
- usabilidad y controlabilidad;
- involucramiento de partes interesadas.
- Los procesos concretos dependen de la funcionalidad y tecnologías de IA previstas.

(c) **Otra información**: no aplica (la referencia a ISO/IEC 22989 está en la orientación).

(d) **Evidencia típica**: procedimiento o metodología de desarrollo de IA (por ejemplo, ciclo de vida de ML documentado), puertas de aprobación (*gates*), registros de control de cambios, plantillas de revisión de diseño.

#### Subdominio A.6.2 – Ciclo de vida del sistema de IA

Objetivo (B.6.2.1): definir los criterios y requisitos para cada etapa del ciclo de vida del sistema de IA.

#### A.6.2.2 Requisitos y especificación del sistema de IA

(a) **Control**: especificar y documentar los requisitos para nuevos sistemas de IA o mejoras sustanciales de los existentes.

(b) **Orientación B.6.2.2**:
- Documentar la justificación y las metas: a) por qué se desarrolla el sistema (caso de negocio, solicitud de cliente, política gubernamental); b) cómo se puede entrenar el modelo y lograr los requisitos de datos.
- Los requisitos deberían abarcar todo el ciclo de vida y revisarse si el sistema no puede operar como se previó o surge nueva información (por ejemplo, inviabilidad financiera).

(c) **Otra información**: **ISO/IEC 5338** describe los procesos del ciclo de vida del sistema de IA; **ISO 9241-210** trata el diseño centrado en las personas para sistemas interactivos.

(d) **Evidencia típica**: documento de requisitos o especificación del sistema de IA, caso de negocio aprobado, requisitos de datos, registro de revisiones de requisitos.

#### A.6.2.3 Documentación del diseño y desarrollo del sistema de IA

(a) **Control**: documentar el diseño y desarrollo del sistema basándose en los objetivos organizacionales, los requisitos documentados y los criterios de especificación.

(b) **Orientación B.6.2.3** – decisiones de diseño a documentar (no exhaustivas):
- enfoque de aprendizaje automático (supervisado frente a no supervisado);
- algoritmo de aprendizaje y tipo de modelo;
- cómo se entrenará el modelo y calidad de los datos (véase B.7);
- evaluación y refinamiento de modelos;
- componentes de hardware y software;
- amenazas de seguridad consideradas durante todo el ciclo de vida, incluidas las específicas de IA: **envenenamiento de datos, robo de modelos, ingeniería inversa de modelos**;
- interfaz y presentación de salidas;
- forma de interacción de los humanos con el sistema;
- interoperabilidad y portabilidad.
- Puede haber múltiples iteraciones diseño-desarrollo; debería mantenerse la documentación de cada etapa y la documentación final de arquitectura.

(c) **Otra información**: **ISO 9241-210** para diseño centrado en las personas.

(d) **Evidencia típica**: documento de diseño y arquitectura, fichas de modelo (*model cards*), registro de decisiones de diseño, modelo de amenazas específico de IA.

#### A.6.2.4 Verificación y validación del sistema de IA

(a) **Control**: definir y documentar medidas de verificación y validación (V&V) del sistema de IA y especificar los criterios para su uso.

(b) **Orientación B.6.2.4**:
- Medidas de V&V: metodologías y herramientas de prueba; selección de datos de prueba y su representatividad del ámbito de uso previsto; requisitos asociados a los criterios de lanzamiento.
- Criterios de evaluación a definir: un plan para evaluar componentes y sistema completo frente a riesgos de impacto sobre personas, grupos y sociedad; el plan puede basarse en requisitos de fiabilidad y protección física (incluidas tasas de error aceptables), en los objetivos de desarrollo y uso responsables (B.6.1.2 y B.9.3), en factores operacionales (calidad de datos, uso previsto, con rangos aceptables) y en usos que exijan factores más rigurosos o tasas de error más bajas.
- Métodos, orientaciones o métricas para evaluar si las partes interesadas que toman o reciben decisiones basadas en el sistema pueden **interpretar adecuadamente** sus resultados; la frecuencia de evaluación puede basarse en la evaluación del impacto.
- Factores aceptables que justifiquen no alcanzar un rendimiento mínimo (por ejemplo, baja resolución de imagen en visión por computador, ruido de fondo en reconocimiento de voz) y mecanismos para gestionar los malos resultados derivados.
- El sistema debería evaluarse contra los criterios documentados; si no los cumple, especialmente respecto a los objetivos responsables, reconsiderar o gestionar las deficiencias del uso previsto, los requisitos de rendimiento y el tratamiento de impactos.

(c) **Otra información**: NOTA: **ISO/IEC TR 24029-1** trata la robustez de las redes neuronales.

(d) **Evidencia típica**: plan de pruebas y de V&V, informes de resultados frente a criterios y umbrales, conjuntos de datos de prueba y su justificación de representatividad, pruebas de sesgo y robustez, registro de decisiones ante incumplimiento de criterios.

#### A.6.2.5 Despliegue del sistema de IA

(a) **Control**: documentar un plan de despliegue y asegurar que se cumplen los requisitos apropiados antes del despliegue.

(b) **Orientación B.6.2.5**:
- Los sistemas pueden desarrollarse en un entorno y desplegarse en otro (por ejemplo, desarrollo *on-premises* y despliegue en nube); el plan debería considerar esas diferencias y si los componentes se despliegan por separado (software y modelo pueden desplegarse de forma independiente).
- Debería existir un conjunto de requisitos previos al lanzamiento ("**criterios de lanzamiento**"): medidas de V&V superadas, métricas de rendimiento cumplidas, pruebas de usuario completadas, aprobaciones y firmas de la dirección.
- El plan debería considerar las perspectivas de las partes interesadas y los impactos para ellas.

(c) **Otra información**: no aplica.

(d) **Evidencia típica**: plan de despliegue, lista de verificación de criterios de lanzamiento firmada, aprobaciones de la dirección, registro de despliegues y de reversión (*rollback*).

#### A.6.2.6 Operación y seguimiento del sistema de IA

(a) **Control**: definir y documentar lo necesario para mantener el sistema de IA en operación; el contenido mínimo recomendado abarca el seguimiento del sistema y de su rendimiento, las reparaciones, las actualizaciones y el soporte. (El enunciado combina "debe" para la obligación y "debería" para el contenido mínimo, igual que el original inglés.)

(b) **Orientación B.6.2.6**:
- **Seguimiento del sistema y del rendimiento**: errores y fallos generales; comportamiento esperado con datos de producción; criterios técnicos (tasas de éxito en resolución de problemas o finalización de tareas, índices de confianza); cumplimiento de compromisos con partes interesadas, requisitos del cliente y requisitos legales.
- **Aprendizaje continuo**: si el sistema sigue entrenándose con datos de producción, hacer seguimiento para asegurar que sigue cumpliendo sus objetivos de diseño.
- **Desviación (deriva) de conceptos o de datos** (*concept/data drift*): aun sin aprendizaje continuo, el rendimiento puede cambiar; el seguimiento puede detectar la necesidad de reentrenar (más información en **ISO/IEC 23053**).
- **Reparaciones**: procesos de respuesta y reparación de errores y fallos.
- **Actualizaciones**: necesarias por evolución, problemas críticos o problemas externos (expectativas del cliente, requisitos legales); procesos que cubran componentes afectados, calendario e información a los usuarios; también cambios operacionales, usos previstos nuevos o modificados y cambios de funcionalidad, con comunicación a usuarios.
- **Soporte**: interno, externo o ambos; cómo contactan los usuarios, cómo se notifican problemas e incidentes, acuerdos de nivel de servicio y métricas.
- Si el sistema se usa para fines distintos o de formas no anticipadas, considerar la idoneidad de tales usos.
- Identificar amenazas de seguridad de la información específicas de IA: envenenamiento de datos, robo de modelos, ingeniería inversa de modelos.

(c) **Otra información** (extensa):
- Considerar el rendimiento operacional que afecta a las partes interesadas al fijar criterios.
- Los criterios de rendimiento dependen de la tarea (clasificación, regresión, ordenación, agrupamiento, reducción de dimensionalidad) y pueden incluir tasas de error y duración del procesamiento; para cada criterio identificar métricas e interdependencias y valores aceptables (recomendaciones de expertos, expectativas de partes interesadas respecto a prácticas no basadas en IA).
- Ejemplo: elegir el valor **F1** como métrica según el impacto de falsos positivos y negativos, conforme a **ISO/IEC TS 4213**, y fijar un valor objetivo.
- Usar el rendimiento de los procesos no basados en IA como contexto; asegurar que los medios y datos de evaluación mejoran la exhaustividad y fiabilidad; las metodologías deberían reflejar fielmente los atributos de operación y uso, y pueden requerir introducir de forma controlada datos o procesos erróneos.
- El modelo de calidad de **ISO/IEC 25059** puede usarse para definir criterios de rendimiento.

(d) **Evidencia típica**: procedimientos de operación y monitorización, cuadros de mando de métricas con umbrales, alertas de deriva, registros de incidentes y reparaciones, calendario y notas de actualización, acuerdos de soporte.

#### A.6.2.7 Documentación técnica del sistema de IA

(a) **Control**: determinar la documentación técnica necesaria para cada categoría de partes interesadas (usuarios, socios, autoridades de supervisión) y proporcionársela en la forma apropiada.

(b) **Orientación B.6.2.7**:
- Contenido general: descripción del sistema y propósito previsto; instrucciones de uso; asunciones técnicas de despliegue y operación (entorno de ejecución, capacidades de software y hardware, asunciones sobre datos); limitaciones técnicas (tasas de error aceptables, precisión, fiabilidad, robustez); capacidades y funciones de supervisión que permiten a usuarios u operadores influir en el sistema.
- Elementos por etapa del ciclo de vida (según **ISO/IEC 22989**): especificación de diseño y arquitectura; elecciones de diseño y medidas de calidad; información sobre los datos usados; asunciones y medidas sobre calidad de datos (distribuciones estadísticas asumidas); actividades de gestión (por ejemplo, gestión del riesgo); registros de V&V; cambios realizados en operación; documentación de la evaluación del impacto (B.5).
- Información técnica sobre operación responsable: plan de gestión de fallos (plan de reversión, desactivación de características, proceso de actualización, notificación a clientes y usuarios); procesos de supervisión de la salud del sistema (**observabilidad**) y de tratamiento de fallos; procedimientos operativos estándar (eventos a supervisar, priorización y revisión de registros, investigación y prevención de fallos); roles responsables de la operación y de rendir cuentas del uso; documentación de actualizaciones y cambios de uso previsto.
- Procedimientos para cambios operacionales con comunicación a usuarios y evaluación interna del tipo de cambio.
- La documentación debería estar actualizada, ser precisa y aprobarse por la dirección pertinente. Los controles de la Tabla A.1 deberían tenerse en cuenta cuando se facilitan como parte de la documentación de usuario.

(c) **Otra información**: no hay apartado separado.

(d) **Evidencia típica**: dossier técnico por sistema de IA con control de versiones y aprobación, manuales de usuario y de operación, plan de gestión de fallos, matriz de qué documentación recibe cada tipo de parte interesada.

#### A.6.2.8 Grabación de los registros de eventos del sistema de IA

(a) **Control**: determinar en qué etapas del ciclo de vida debería habilitarse la grabación de registros de eventos (*event logs*), como mínimo cuando el sistema de IA esté en uso.

(b) **Orientación B.6.2.8**:
- El registro debería recopilar automáticamente eventos de la operación para: la trazabilidad de la funcionalidad (asegurar operación según lo previsto) y la detección de rendimiento fuera de las condiciones previstas que pueda causar resultados no deseados o impactos en partes interesadas.
- Los registros pueden incluir fecha y hora de cada uso, datos de producción sobre los que opera el sistema, salidas fuera del rango previsto, etc.
- Conservación: durante el tiempo necesario para el uso previsto y conforme a las políticas de retención de la organización; pueden aplicar requisitos legales de retención.

(c) **Otra información**: algunos sistemas, como los de identificación biométrica, pueden tener requisitos adicionales de registro según la jurisdicción.

(d) **Evidencia típica**: configuración de *logging* activa en producción, muestras de registros, política de retención, controles de acceso e integridad de los registros, evidencia de revisión periódica.

### 4.8 Dominio A.7 / B.7 – Datos para sistemas de IA

Objetivo (B.7.1): asegurar que la organización comprende el rol y los impactos de los datos en los sistemas de IA, tanto en desarrollo como en provisión o uso, durante todo el ciclo de vida.

#### A.7.2 Datos para el desarrollo y la mejora del sistema de IA

(a) **Control**: definir, documentar e implementar procesos de gestión de datos para el desarrollo de sistemas de IA.

(b) **Orientación B.7.2** – la gestión de datos puede abarcar: implicaciones de privacidad y seguridad (datos sensibles); amenazas a la protección y seguridad derivadas de sistemas dependientes de datos; transparencia y explicabilidad (origen de los datos y capacidad de explicar cómo se usan para determinar la salida, si el sistema lo requiere); representatividad de los datos de entrenamiento frente al ámbito operacional de uso; precisión e integridad de los datos.

(c) **Otra información**: NOTA: **ISO/IEC 22989** detalla los conceptos de ciclo de vida y gestión de datos.

(d) **Evidencia típica**: procedimiento de gobierno de datos para IA, análisis de representatividad, controles de acceso a datos de entrenamiento, evaluación de privacidad de los datos.

#### A.7.3 Adquisición de los datos

(a) **Control**: determinar y documentar los detalles de la adquisición y selección de los datos usados en los sistemas de IA.

(b) **Orientación B.7.3** – detalles a documentar: categorías de datos necesarias; cantidad requerida; fuentes (internas, de pago, compartidas, abiertas, sintéticas); características de las fuentes (estáticas, dinámicas, recopiladas, generadas por ordenador); datos demográficos y características de los interesados (sesgos potenciales o conocidos, errores no aleatorios); manejo previo (usos anteriores, conformidad con privacidad y seguridad); derechos sobre los datos (IIP, derechos de autor); metadatos asociados (etiquetado, mejora de datos); origen.

(c) **Otra información**: pueden usarse las categorías de datos y la estructura de uso de datos de **ISO/IEC 19944-1**.

(d) **Evidencia típica**: registro de fuentes de datos con licencias y bases legales, contratos de adquisición de datos, análisis demográfico de los conjuntos, fichas de metadatos.

#### A.7.4 Calidad de los datos para los sistemas de IA

(a) **Control**: definir y documentar requisitos de calidad de los datos y asegurar que los datos usados para desarrollar y operar el sistema los cumplen.

(b) **Orientación B.7.4**:
- La calidad de los datos afecta significativamente a la validez de las salidas. **ISO/IEC 25024** define la calidad de los datos como el grado en que sus características inherentes satisfacen necesidades explícitas e implícitas en condiciones específicas.
- En aprendizaje supervisado o semisupervisado, la calidad de los datos de entrenamiento, validación, prueba y producción debería definirse, medirse y mejorarse; los datos deben ser adecuados para el propósito.
- Considerar el impacto de los sesgos en el rendimiento y la equidad, y ajustar modelo y datos hasta niveles aceptables para el caso de uso.

(c) **Otra información**: serie **ISO/IEC 5259** (calidad de datos para analítica y aprendizaje automático); **ISO/IEC TR 24027** (tipos de sesgo en datos y en la toma de decisiones asistida por IA).

(d) **Evidencia típica**: requisitos de calidad de datos documentados (completitud, exactitud, actualidad, representatividad), informes de medición de calidad, análisis de sesgo y acciones de corrección.

#### A.7.5 Procedencia de los datos

(a) **Control**: definir y documentar un proceso para registrar la procedencia (*provenance*) de los datos a lo largo de los ciclos de vida de los datos y del sistema de IA.

(b) **Orientación B.7.5**:
- Según **ISO 8000-2**, un registro de procedencia puede incluir la creación, actualización, transcripción, abstracción, validación y transferencia de control de los datos; también la compartición sin transferencia de control y las transformaciones.
- Según fuentes, contenido y contexto, considerar si se necesitan medidas para **verificar** la procedencia.

(c) **Otra información**: no hay apartado separado.

(d) **Evidencia típica**: registros de linaje de datos (*data lineage*), herramientas de catálogo con trazabilidad, evidencias de verificación de fuentes externas.

#### A.7.6 Preparación de los datos

(a) **Control**: definir y documentar los criterios para seleccionar las preparaciones de datos y los métodos de preparación a utilizar.

(b) **Orientación B.7.6**:
- Los datos suelen requerir preparación para ser utilizables; los algoritmos a veces no toleran entradas ausentes o incorrectas, distribuciones no normales o rangos muy amplios; una mala preparación puede causar errores.
- Métodos habituales: estudio estadístico (distribución, media, mediana, desviación típica, rango, estratificación, muestreo) y metadatos estadísticos (especificación DDI, Data Documentation Initiative, referencia [28]); limpieza; imputación; normalización; escalado; etiquetado de variables objetivo; codificación (variables cualitativas a números).
- Para cada tarea, documentar los criterios de selección y los métodos y transformaciones concretos aplicados.

(c) **Otra información**: NOTA: serie **ISO/IEC 5259** e **ISO/IEC 23053** para preparación de datos en aprendizaje automático.

(d) **Evidencia típica**: documentación de pipelines de preprocesamiento, criterios de selección de métodos, código versionado de transformaciones, informes exploratorios de datos.

### 4.9 Dominio A.8 / B.8 – Información para las partes interesadas de los sistemas de IA

Objetivo (B.8.1): asegurar que las partes interesadas pertinentes disponen de la información necesaria para comprender y evaluar los riesgos y sus impactos, positivos y negativos.

#### A.8.2 Documentación e información del sistema para los usuarios

(a) **Control**: determinar y proporcionar la información necesaria a los usuarios del sistema de IA.

(b) **Orientación B.8.2**:
- La información puede incluir detalles técnicos, instrucciones y notificaciones de que el usuario interactúa con una IA, tanto sobre el sistema como sobre sus salidas (por ejemplo, avisar de que una imagen fue creada por IA).
- Los usuarios deberían poder comprender cómo funciona el sistema, su propósito y usos previstos y su potencial de daño o beneficio; parte de la documentación puede dirigirse a usuarios técnicos (administradores); la información debería ser **accesible** (fácil de encontrar y con funciones de accesibilidad).
- Información que puede proporcionarse: propósito; que se interactúa con IA; cómo interactuar; cómo y cuándo anular el sistema; requisitos técnicos, limitaciones y vida útil prevista; necesidades de supervisión humana; precisión y rendimiento; información de la evaluación del impacto (beneficios y perjuicios, por contexto o grupo demográfico, B.5.2 y B.5.4); cambios en las afirmaciones sobre beneficios; actualizaciones, cambios y mantenimiento con su frecuencia; contacto; material de formación.
- Documentar los **criterios** para decidir si informar y qué informar (uso previsto y uso indebido previsible, conocimientos del usuario, impacto específico).
- Canales: instrucciones documentadas, alertas y notificaciones integradas, página web, etc.; comprobar que los usuarios tienen acceso y que la información es completa, precisa y actualizada.

(c) **Otra información**: no aplica.

(d) **Evidencia típica**: documentación de usuario, avisos de interacción con IA en la interfaz, etiquetado de contenidos generados, criterios documentados de información, pruebas de accesibilidad, registros de actualización.

#### A.8.3 Notificación externa

(a) **Control**: proporcionar medios para que las partes interesadas notifiquen impactos adversos del sistema de IA.

(b) **Orientación B.8.3**: además de la supervisión interna del sistema para detectar problemas y fallos, la organización debería ofrecer medios a usuarios y otras partes externas para notificar efectos adversos (por ejemplo, falta de equidad).

(c) **Otra información**: no aplica. Complementa a A.3.3 (canal interno) con un canal externo.

(d) **Evidencia típica**: formulario o canal público de notificación, procedimiento de triaje y respuesta, registro de notificaciones recibidas y acciones derivadas.

#### A.8.4 Comunicación de incidentes

(a) **Control**: determinar y documentar un plan para comunicar incidentes a los usuarios del sistema de IA.

(b) **Orientación B.8.4**:
- Los incidentes pueden ser específicos de IA o de seguridad de la información o privacidad (por ejemplo, violación de datos). La organización debería conocer sus obligaciones de notificación según el contexto; un incidente en un componente de IA de un producto que afecte a la protección de personas puede tener requisitos distintos.
- Requisitos legales, contratos y reguladores pueden fijar: tipos de incidentes a comunicar; plazo; si se notifica a autoridades y a cuáles; detalles a comunicar.
- Puede integrarse en la gestión general de incidentes, pero atendiendo a requisitos específicos (por ejemplo, una violación de IIP en los datos de entrenamiento).

(c) **Otra información**: **ISO/IEC 27001** e **ISO/IEC 27701** detallan la gestión de incidentes de seguridad y de privacidad respectivamente.

(d) **Evidencia típica**: plan de comunicación de incidentes de IA integrado en el procedimiento de gestión de incidentes, plantillas de notificación, matriz de obligaciones regulatorias y contractuales, registros de incidentes y comunicaciones.

#### A.8.5 Información para las partes interesadas

(a) **Control**: determinar y documentar las obligaciones de comunicación de información sobre el sistema de IA a las partes interesadas.

(b) **Orientación B.8.5**:
- Una jurisdicción puede exigir compartir información con autoridades o reguladores; puede comunicarse a clientes o autoridades en plazos apropiados.
- Información que puede enviarse: documentación técnica (incluidos conjuntos de datos de entrenamiento, validación y prueba, justificación de algoritmos, registros de V&V); riesgos del sistema; resultados de evaluaciones de impacto; registros (*logs*) del sistema.
- Comprender las obligaciones y compartir lo apropiado con las autoridades; se presupone conocimiento de los requisitos sobre información a fuerzas y cuerpos de seguridad.

(c) **Otra información**: no aplica.

(d) **Evidencia típica**: registro de obligaciones legales, regulatorias y contractuales de información (vinculado a 4.2), evidencias de comunicaciones a reguladores o clientes, procedimiento de respuesta a solicitudes de autoridades.

### 4.10 Dominio A.9 / B.9 – Uso de sistemas de IA

Objetivo (B.9.1): asegurar que la organización usa los sistemas de IA de forma responsable y conforme a sus políticas. Este dominio aplica sobre todo a la organización en rol de **usuario** de IA (propia o de terceros).

#### A.9.2 Procesos para el uso responsable de los sistemas de IA

(a) **Control**: definir y documentar los procesos para el uso responsable de los sistemas de IA.

(b) **Orientación B.9.2**:
- La organización puede tener varias razones para decidir usar un sistema de IA (propio o de tercero) y debería ser clara sobre ellas y desarrollar políticas al respecto; ejemplos: aprobaciones requeridas; coste (incluido seguimiento y mantenimiento continuos); requisitos aprobados de contratación; requisitos legales aplicables.
- Pueden incorporarse políticas ya existentes para el uso de otros sistemas y activos.

(c) **Otra información**: no aplica.

(d) **Evidencia típica**: procedimiento de aprobación e incorporación de sistemas de IA (incluidas herramientas de IA generativa de terceros), criterios de adquisición, registro de sistemas de IA aprobados.

#### A.9.3 Objetivos para el uso responsable del sistema de IA

(a) **Control**: identificar y documentar objetivos que guíen el uso responsable de los sistemas de IA.

(b) **Orientación B.9.3**:
- Objetivos posibles según contexto: equidad; rendición de cuentas; transparencia; explicabilidad; fiabilidad; protección; robustez y redundancia; privacidad y seguridad; accesibilidad.
- Implementar mecanismos para lograrlos, incluida la determinación de si una solución de tercero cumple los objetivos o si una solución interna es aplicable al uso previsto.
- Determinar en qué etapas incorporar objetivos significativos de **supervisión humana**: revisores humanos que verifiquen salidas con autoridad para anular decisiones; supervisión humana cuando la exija la documentación del despliegue; seguimiento del rendimiento y precisión de salidas; notificación de inquietudes sobre salidas e impactos; notificación de cambios en el rendimiento con datos de producción; valoración de si la **toma de decisiones automatizada** es apropiada.
- La necesidad de supervisión humana puede fundamentarse en las evaluaciones de impacto (B.5). El personal de supervisión debería estar informado y formado en las instrucciones del sistema y en sus tareas; la supervisión humana puede mejorar la supervisión automática.

(c) **Otra información**: el **Anexo C** ofrece ejemplos de objetivos organizacionales para la gestión del riesgo útiles para fijar objetivos de uso.

(d) **Evidencia típica**: objetivos de uso responsable documentados, diseño de supervisión humana por sistema (puntos de control, autoridad de anulación), registros de formación de revisores, registros de anulaciones e inquietudes notificadas.

#### A.9.4 Uso previsto del sistema de IA

(a) **Control**: asegurar que el sistema de IA se usa de acuerdo con sus usos previstos y su documentación acompañante.

(b) **Orientación B.9.4**:
- Desplegar según las instrucciones y documentación del sistema (B.8.2); el despliegue puede requerir recursos de soporte específicos y la supervisión humana requerida (B.9.3); los datos sobre los que opera pueden tener que ajustarse a la documentación para asegurar un rendimiento preciso.
- Hacer seguimiento de la operación (B.6.2.6). Si el despliegue correcto suscita inquietudes de impacto o legales, comunicarlas al personal pertinente y al tercero proveedor.
- Mantener registros de eventos u otra documentación de despliegue y operación que demuestren el uso según lo previsto o ayuden a comunicar inquietudes; el periodo de conservación depende del uso previsto, las políticas de la organización y los requisitos legales.

(c) **Otra información**: no aplica.

(d) **Evidencia típica**: declaración de uso previsto por sistema, evidencias de conformidad del despliegue con la documentación del proveedor, registros de uso, comunicaciones de inquietudes al proveedor.

### 4.11 Dominio A.10 / B.10 – Relaciones con terceros y clientes

Objetivo (B.10.1): asegurar que la organización comprende sus responsabilidades y sigue rindiendo cuentas, y que los riesgos se distribuyen apropiadamente cuando intervienen terceros en cualquier etapa del ciclo de vida.

#### A.10.2 Asignación de responsabilidades

(a) **Control**: asegurar que las responsabilidades del ciclo de vida del sistema de IA se asignan entre la organización, sus socios, proveedores, clientes y terceros.

(b) **Orientación B.10.2**:
- Las responsabilidades pueden repartirse entre quienes proporcionan datos, quienes proporcionan algoritmos y modelos, quienes desarrollan o usan el sistema y quienes rinden cuentas ante las partes interesadas. Documentar todas las partes y sus roles, y determinar sus responsabilidades.
- Si la organización suministra un sistema de IA a un tercero, debería adoptar un enfoque responsable de desarrollo (B.6) y poder proporcionar la documentación necesaria (B.6.2.7 y B.8.2).
- Si se trata IIP, las responsabilidades se reparten entre **responsables** y **encargados** del tratamiento de IIP (**ISO/IEC 29100**); considerar controles de **ISO/IEC 27701**; la organización puede ser responsable (o corresponsable), encargado o ambos según sus actividades y rol.

(c) **Otra información**: no hay apartado separado; las referencias están en la orientación.

(d) **Evidencia típica**: mapa de la cadena de suministro de IA por sistema con roles y responsabilidades, contratos y acuerdos de tratamiento de datos, matriz de responsabilidades compartidas con proveedores cloud o de modelos.

#### A.10.3 Proveedores

(a) **Control**: establecer un proceso para asegurar que el uso de servicios, productos o materiales suministrados por proveedores es coherente con el enfoque de la organización para el desarrollo y uso responsables de IA.

(b) **Orientación B.10.3**:
- Los proveedores pueden suministrar desde conjuntos de datos, modelos, algoritmos o librerías hasta sistemas de IA completos usados como tales o integrados en otro producto (por ejemplo, un vehículo).
- Considerar tipos de proveedores, qué suministran y el nivel de riesgo al determinar la selección, los requisitos exigidos y el seguimiento y evaluación continuos.
- Documentar cómo se integran el sistema de IA y sus componentes con los sistemas desarrollados o usados por la organización.
- Si el sistema o componente de un proveedor no rinde como se previó o puede tener impactos no alineados con el enfoque responsable, exigir acciones correctivas (posiblemente en colaboración).
- Asegurar que el proveedor entrega documentación adecuada (B.6.2.7 y B.8.2).

(c) **Otra información**: no aplica.

(d) **Evidencia típica**: procedimiento de evaluación y selección de proveedores de IA, cuestionarios de debida diligencia, cláusulas contractuales sobre IA responsable y documentación, registros de seguimiento del desempeño y de acciones correctivas exigidas.

#### A.10.4 Clientes

(a) **Control**: asegurar que el enfoque responsable de desarrollo y uso de IA tiene en cuenta las expectativas y necesidades de los clientes.

(b) **Orientación B.10.4**:
- Cuando la organización actúa como proveedor, debería entender las expectativas y necesidades del cliente, que pueden plasmarse como requisitos de producto o servicio en diseño o ingeniería, requisitos contractuales o acuerdos generales de uso; distintos tipos de relación implican distintas necesidades.
- Entender la naturaleza compleja de la relación proveedor-cliente y cuándo la responsabilidad recae en cada uno.
- La organización puede identificar riesgos del uso de sus productos de IA por el cliente y tratarlos proporcionándole información apropiada para que el cliente gestione los suyos; por ejemplo, comunicar los **límites del ámbito de uso** válido del sistema (véanse B.6.2.7 y B.8.2).

(c) **Otra información**: no aplica.

(d) **Evidencia típica**: requisitos de cliente capturados en la especificación, contratos y términos de uso con limitaciones del sistema, comunicaciones al cliente sobre ámbito de uso y riesgos, gestión de reclamaciones.

### 4.12 Mapa rápido de los 38 controles

| Código | Título | Dominio | Palabras clave |
|---|---|---|---|
| A.2.2 | Política de IA | A.2 Políticas | política documentada, principios, excepciones, estrategia, apetito de riesgo, 5.2 |
| A.2.3 | Alineamiento con otras políticas organizacionales | A.2 Políticas | calidad, seguridad, protección, privacidad, ISO/IEC 38507, órgano de gobierno |
| A.2.4 | Revisión de la política de IA | A.2 Políticas | intervalos planificados, propietario de la política, revisión por la dirección |
| A.3.2 | Roles y responsabilidades en materia de IA | A.3 Organización interna | rendición de cuentas, RACI, áreas: riesgo, impacto, datos, supervisión humana |
| A.3.3 | Notificación de inquietudes | A.3 Organización interna | canal confidencial y anónimo, antirrepresalias, escalado, ISO 37002 |
| A.4.2 | Documentación de los recursos | A.4 Recursos | inventario de activos de IA, diagramas de flujo de datos y arquitectura |
| A.4.3 | Recursos de datos | A.4 Recursos | procedencia, categorías, etiquetado, calidad, retención, sesgos, ISO/IEC 19944-1, 5259 |
| A.4.4 | Recursos de herramientas | A.4 Recursos | algoritmos, modelos, métodos de optimización y evaluación, ISO/IEC 23053 |
| A.4.5 | Recursos del sistema y de computación | A.4 Recursos | on-premises, nube, edge, red, almacenamiento, impacto ambiental, ISO/IEC 22989 |
| A.4.6 | Recursos humanos | A.4 Recursos | competencias, científicos de datos, supervisores humanos, expertos de dominio |
| A.5.2 | Proceso de evaluación del impacto del sistema de IA | A.5 Evaluación de impactos | circunstancias de activación, identificación-análisis-evaluación-tratamiento, ISO/IEC 23894, 6.1.4 |
| A.5.3 | Documentación de las evaluaciones del impacto | A.5 Evaluación de impactos | conservación, uso indebido previsible, fallos predecibles, grupos demográficos |
| A.5.4 | Evaluación del impacto sobre personas o grupos | A.5 Evaluación de impactos | equidad, transparencia, privacidad, salud, derechos humanos, grupos vulnerables, IIP |
| A.5.5 | Evaluación de los impactos sociales | A.5 Evaluación de impactos | sostenibilidad ambiental, economía, gobierno, salud, cultura, deepfakes, ISO/IEC TR 24368 |
| A.6.1.2 | Objetivos para el desarrollo responsable | A.6.1 Gestión del desarrollo | objetivos 6.2, equidad integrada en el ciclo, Anexo C |
| A.6.1.3 | Procesos para el diseño y desarrollo responsables | A.6.1 Gestión del desarrollo | etapas del ciclo de vida, pruebas, supervisión humana, criterios de lanzamiento, control de cambios, ISO/IEC 22989 |
| A.6.2.2 | Requisitos y especificación del sistema de IA | A.6.2 Ciclo de vida | justificación, caso de negocio, requisitos de datos, ISO/IEC 5338, ISO 9241-210 |
| A.6.2.3 | Documentación del diseño y desarrollo | A.6.2 Ciclo de vida | enfoque de ML, algoritmo, amenazas (envenenamiento, robo de modelo), arquitectura |
| A.6.2.4 | Verificación y validación del sistema de IA | A.6.2 Ciclo de vida | pruebas, datos de prueba representativos, criterios de evaluación, tasas de error, ISO/IEC TR 24029-1 |
| A.6.2.5 | Despliegue del sistema de IA | A.6.2 Ciclo de vida | plan de despliegue, criterios de lanzamiento, aprobaciones, entornos |
| A.6.2.6 | Operación y seguimiento del sistema de IA | A.6.2 Ciclo de vida | monitorización, deriva de datos y conceptos, reparaciones, actualizaciones, soporte, F1, ISO/IEC TS 4213, 25059, 23053 |
| A.6.2.7 | Documentación técnica del sistema de IA | A.6.2 Ciclo de vida | dossier técnico, limitaciones, plan de gestión de fallos, observabilidad, SOP, aprobación |
| A.6.2.8 | Grabación de los registros de eventos | A.6.2 Ciclo de vida | logs automáticos en uso, trazabilidad, retención, biometría |
| A.7.2 | Datos para el desarrollo y la mejora | A.7 Datos | gestión de datos, privacidad, representatividad, explicabilidad, ISO/IEC 22989 |
| A.7.3 | Adquisición de los datos | A.7 Datos | fuentes, cantidad, derechos, IIP, derechos de autor, metadatos, ISO/IEC 19944-1 |
| A.7.4 | Calidad de los datos | A.7 Datos | requisitos de calidad, ISO/IEC 25024, sesgos, ISO/IEC 5259, TR 24027 |
| A.7.5 | Procedencia de los datos | A.7 Datos | linaje, creación, transformación, transferencia de control, verificación, ISO 8000-2 |
| A.7.6 | Preparación de los datos | A.7 Datos | limpieza, imputación, normalización, escalado, codificación, DDI, ISO/IEC 5259, 23053 |
| A.8.2 | Documentación e información del sistema para usuarios | A.8 Información a partes interesadas | aviso de IA, propósito, cómo anular, limitaciones, accesibilidad, criterios de información |
| A.8.3 | Notificación externa | A.8 Información a partes interesadas | canal externo de impactos adversos, falta de equidad |
| A.8.4 | Comunicación de incidentes | A.8 Información a partes interesadas | plan de comunicación, plazos, autoridades, ISO/IEC 27001, 27701 |
| A.8.5 | Información para las partes interesadas | A.8 Información a partes interesadas | obligaciones regulatorias, documentación técnica, logs, autoridades |
| A.9.2 | Procesos para el uso responsable | A.9 Uso | aprobaciones, coste, contratación, requisitos legales |
| A.9.3 | Objetivos para el uso responsable | A.9 Uso | equidad, transparencia, explicabilidad, supervisión humana, anulación, decisiones automatizadas, Anexo C |
| A.9.4 | Uso previsto del sistema de IA | A.9 Uso | instrucciones del proveedor, datos conformes, registros de uso, comunicación de inquietudes |
| A.10.2 | Asignación de responsabilidades | A.10 Terceros y clientes | cadena de suministro, roles, responsable/encargado de IIP, ISO/IEC 29100, 27701 |
| A.10.3 | Proveedores | A.10 Terceros y clientes | selección, requisitos, seguimiento, acciones correctivas, documentación del proveedor |
| A.10.4 | Clientes | A.10 Terceros y clientes | expectativas del cliente, requisitos contractuales, límites del ámbito de uso |

## 5. Conceptos y términos clave

| Término (ES) | Término (EN) | Definición breve | Dónde aparece |
|---|---|---|---|
| Control de referencia | Reference control | Control del Anexo A que sirve de referencia para tratar riesgos y cumplir objetivos; no es obligatorio usarlo y puede sustituirse o complementarse. | A.1, 6.1.3 NOTA 1 y 2 |
| Objetivo de control | Control objective | Enunciado del propósito de un dominio de controles; está implícito en los controles seleccionados. | Tabla A.1, B.x.1, 6.1.3 NOTA 2 |
| Declaración de aplicabilidad (DdA) | Statement of Applicability (SoA) | Documento que lista los controles necesarios y justifica la inclusión o exclusión de cada uno; no incluye la orientación del Anexo B. | 6.1.3 f), B.1, definición 3.26 |
| Tratamiento del riesgo de la IA | AI risk treatment | Proceso para seleccionar opciones de tratamiento, determinar controles, contrastarlos con el Anexo A y formular el plan de tratamiento. | 6.1.3, A.1, B.1 |
| Orientación sobre la implementación | Implementation guidance | Recomendaciones del Anexo B para implementar cada control; modificables o sustituibles. | B.1 y cada B.x.y |
| Política de IA | AI policy | Política documentada que dirige el desarrollo o uso de IA, basada en estrategia, valores, riesgo y requisitos legales. | A.2.2, B.2.2, 5.2 |
| Notificación de inquietudes | Reporting of concerns | Proceso interno, confidencial o anónimo, para notificar inquietudes sobre el rol de la organización respecto a un sistema de IA. | A.3.3, B.3.3 |
| Recursos del sistema de IA | AI system resources | Componentes, datos, herramientas, sistema y computación y recursos humanos necesarios en el ciclo de vida. | A.4, B.4.2 |
| Evaluación del impacto del sistema de IA | AI system impact assessment | Evaluación de consecuencias potenciales para personas, grupos de personas y sociedad durante el ciclo de vida. | A.5, B.5, 6.1.4, 8.4 |
| Información de identificación personal (IIP) | Personally identifiable information (PII) | Información que permite identificar a una persona; su tratamiento reparte responsabilidades entre responsables y encargados. | B.5.4, B.7.3, B.8.4, B.10.2 |
| Ciclo de vida del sistema de IA | AI system life cycle | Conjunto de etapas desde requisitos hasta retirada; modelo genérico en ISO/IEC 22989, procesos en ISO/IEC 5338. | A.6, B.6.1.3, B.6.2.7 |
| Criterios de lanzamiento | Release criteria | Conjunto de requisitos a cumplir antes del despliegue (V&V superada, métricas, pruebas de usuario, aprobaciones). | B.6.1.3, B.6.2.4, B.6.2.5 |
| Verificación y validación | Verification and validation (V&V) | Medidas y criterios para comprobar que el sistema cumple sus requisitos y es adecuado para el uso previsto. | A.6.2.4, B.6.2.4 |
| Deriva de datos o de conceptos | Data drift / concept drift | Cambio en la distribución de datos o en la relación que el modelo aprendió, que degrada el rendimiento y puede requerir reentrenamiento. | B.6.2.6 |
| Aprendizaje continuo | Continuous learning | Modalidad en que el modelo sigue entrenándose con datos de producción tras el despliegue. | B.6.2.6 |
| Envenenamiento de datos, robo de modelos, ingeniería inversa de modelos | Data poisoning, model stealing, model inversion | Amenazas de seguridad de la información específicas de los sistemas de IA. | B.6.2.3, B.6.2.6 |
| Observabilidad | Observability | Capacidad de supervisar que el sistema opera según lo previsto y dentro de sus márgenes normales. | B.6.2.7 |
| Registro de eventos | Event log | Registro automático de eventos de operación (fecha y hora, datos de producción, salidas fuera de rango) para trazabilidad y detección. | A.6.2.8, B.6.2.8, B.9.4 |
| Procedencia de los datos | Data provenance | Registro del origen e historia de los datos (creación, actualización, transformación, transferencia de control). | A.7.5, B.7.5, ISO 8000-2 |
| Calidad de los datos | Data quality | Grado en que las características de los datos satisfacen las necesidades en condiciones específicas (ISO/IEC 25024). | A.7.4, B.7.4 |
| Preparación de los datos | Data preparation | Transformaciones (limpieza, imputación, normalización, escalado, codificación) que hacen los datos utilizables. | A.7.6, B.7.6 |
| Supervisión humana | Human oversight | Intervención de revisores humanos con autoridad para verificar y anular decisiones del sistema de IA. | B.3.2, B.6.1.3, B.8.2, B.9.3, B.9.4 |
| Uso previsto | Intended use | Finalidad y condiciones de uso definidas en la documentación del sistema; el uso indebido razonablemente previsible también se evalúa. | A.9.4, B.5.3, B.8.2 |
| Responsable y encargado del tratamiento de IIP | PII controller / PII processor | Roles que determinan fines y medios del tratamiento, o tratan IIP por cuenta del responsable (ISO/IEC 29100). | B.10.2 |
| Ultrafalsos | Deepfakes | Contenidos sintéticos generados por IA que pueden usarse para desinformación. | B.5.5 |

## 6. Relación con otros documentos del corpus

- **Cláusulas 4-10 de la misma norma** (resumen 01 del corpus): el Anexo A se activa a través de 6.1.3 (tratamiento del riesgo y DdA), 8.1 y 8.3 (implementación de controles). Dominios concretos operacionalizan cláusulas del cuerpo: A.2 desarrolla 5.2 (Política de IA); A.3.2 desarrolla 5.3 (Roles, responsabilidades y autoridades); A.4.6 se apoya en 7.2 (Competencia); A.5 desarrolla 6.1.4 y 8.4 (Evaluación del impacto); A.8 se relaciona con 7.4 (Comunicación) y 4.2 (Partes interesadas); A.2.4 se conecta con 9.3 (Revisión por la dirección).
- **Anexos C y D de la misma norma** (resumen 03): B.6.1.2 y B.9.3 remiten explícitamente al **Anexo C** (objetivos organizacionales y fuentes de riesgo) como fuente de ejemplos de objetivos para el desarrollo y el uso responsables. El Anexo D (uso del SGIA en distintos dominios y sectores) contextualiza la integración con otros sistemas de gestión (27001, 27701, 9001) mencionada en 6.1.3 NOTA 2 y en B.5.2.
- **ISO/IEC 23894:2023 (gestión del riesgo de la IA)**: B.5.2 NOTA la cita como referencia para integrar la evaluación de impacto en la gestión del riesgo. La evaluación del riesgo de 23894 es la entrada del proceso 6.1.3 que selecciona controles del Anexo A.
- **Guía para la Implementación ISO/IEC 42001 (Impulsa360 Academy)**: su hoja de ruta de implementación debería mapear los 38 controles del Anexo A a entregables; este resumen aporta la lista completa y las evidencias típicas por control.
- **Plantilla "Declaración de Aplicabilidad 42001 – SOA v2"**: es la materialización directa de 6.1.3 f). Cada fila de la hoja de la DdA debería corresponder a uno de los 38 controles A.x.y aquí descritos, con columnas de aplicabilidad, justificación de inclusión o exclusión y referencia a la implementación. La orientación B.x.y puede usarse para redactar la columna de implementación, pero no debe figurar como objeto de justificación.
- **Plantilla "Análisis de Riesgo SGIA v.03"**: los riesgos identificados deben enlazarse con los controles del Anexo A seleccionados como tratamiento; por ejemplo, un riesgo de sesgo en datos de entrenamiento se trata con A.7.3, A.7.4 y A.6.2.4; un riesgo de uso indebido con A.8.2 y A.9.4; un riesgo de proveedor con A.10.2 y A.10.3.
- **Plantilla "Diagnóstico ISO 42001 – Análisis de Brechas"**: el diagnóstico inicial debería incluir los 9 dominios y 38 controles para evaluar el grado de madurez; la tabla del apartado 4.12 sirve de lista de verificación.
- **Plantillas "FODA vs Riesgo" y "Stakeholders"**: el análisis de partes interesadas alimenta A.8 (información a partes interesadas), A.5 (personas y grupos afectados) y A.10 (proveedores y clientes).
- **ISO 19011 (auditoría de sistemas de gestión)**: las evidencias típicas del apartado 4.2 se corresponden con los métodos de recopilación de evidencia de 19011; en una auditoría de certificación 42001, la fase 2 verifica la implementación de los controles declarados aplicables en la DdA.
- **Catálogo PECB / Impulsa360**: los cursos Lead Implementer y Lead Auditor ISO/IEC 42001 dedican módulos específicos a los Anexos A y B; el presente resumen cubre ese temario.

## 7. Aplicación práctica y puntos de examen

### 7.1 Cómo se usa en una implementación real

1. Realizar la evaluación del riesgo de la IA (6.1.2) y la evaluación del impacto (6.1.4) por sistema de IA.
2. Elegir opciones de tratamiento y determinar controles necesarios; contrastarlos con la Tabla A.1 para no omitir ninguno (6.1.3 b).
3. Añadir controles propios o de otras fuentes si el Anexo A no basta (6.1.3 d), por ejemplo controles de ISO/IEC 27001 o 27701.
4. Redactar la DdA con los 38 controles del Anexo A más los adicionales, marcando aplicable / no aplicable y justificando cada decisión (6.1.3 f).
5. Formular y aprobar el plan de tratamiento y la aceptación de riesgos residuales (6.1.3 g).
6. Implementar cada control usando el Anexo B como guía adaptable (8.1, 8.3), generando la información documentada que el control exige.
7. Auditar internamente (9.2) y revisar por la dirección (9.3) la eficacia de los controles; revisar la política de IA (A.2.4).

### 7.2 Puntos "debes saber"

1. El Anexo A tiene **9 dominios (A.2 a A.10) y 38 controles**; el primer control de cada dominio se numera ".2" porque ".1" es el objetivo.
2. A.6 es el único dominio con dos subdominios (A.6.1 con 2 controles y A.6.2 con 7), y es el dominio más extenso (9 controles).
3. Los controles del Anexo A son **de referencia**: no es obligatorio aplicarlos todos, pueden excluirse con justificación y pueden añadirse otros (A.1, 6.1.3 NOTA 2 y 3).
4. La **DdA** debe contener los controles necesarios y justificar inclusiones y exclusiones; una exclusión puede justificarse porque el riesgo no lo requiere o porque los requisitos externos no lo exigen.
5. El **Anexo B no se documenta ni se justifica en la DdA** (B.1); es orientación adaptable, "punto de partida".
6. Ambos anexos se denominan **normativos**, pero la obligatoriedad de un control solo nace de su selección en la DdA; el Anexo B es recomendación ("debería").
7. A.3.3 (canal interno de inquietudes, con referencia a ISO 37002) y A.8.3 (canal externo para partes interesadas) son controles distintos y complementarios.
8. La evaluación del impacto del Anexo A.5 se centra en **personas, grupos de personas y sociedad**, no en la organización; el impacto organizacional se trata en la gestión del riesgo (ISO/IEC 23894).
9. Las tres amenazas de seguridad específicas de IA citadas por la norma son **envenenamiento de datos, robo de modelos e ingeniería inversa de modelos** (B.6.2.3 y B.6.2.6).
10. A.6.2.8 exige registros de eventos **como mínimo cuando el sistema está en uso**; la retención sigue las políticas de la organización y los requisitos legales.
11. Las cuatro categorías de datos por función en aprendizaje automático son **entrenamiento, validación, prueba y producción** (B.4.3, B.7.4).
12. B.6.2.6 distingue entre sistemas con **aprendizaje continuo** y sistemas afectados por **deriva de datos o de conceptos**; ambos requieren monitorización y posible reentrenamiento.
13. La supervisión humana aparece transversalmente (B.3.2, B.6.1.3, B.8.2, B.9.3, B.9.4); B.9.3 exige que los revisores tengan **autoridad para anular** decisiones y que se valore si la decisión automatizada es apropiada.
14. En A.10.2, cuando hay IIP, la organización puede ser **responsable, corresponsable, encargado o ambos** (ISO/IEC 29100, 27701).
15. El **Anexo C** se cita en B.6.1.2 y B.9.3 como fuente de objetivos; la **ISO/IEC 22989** es la referencia para etapas del ciclo de vida y recursos; la **ISO/IEC 5338** para procesos del ciclo de vida.

### 7.3 Preguntas típicas de examen o auditoría

- ¿Cuántos controles tiene el Anexo A y cómo se agrupan? (38 controles en 9 dominios.)
- ¿Puede una organización certificarse excluyendo controles del Anexo A? (Sí, con justificación documentada en la DdA.)
- ¿Debe justificarse en la DdA la no aplicación de una orientación del Anexo B? (No.)
- ¿Qué cláusula exige comparar los controles determinados con el Anexo A? (6.1.3 b.)
- ¿Qué control exige un canal para que terceros notifiquen impactos adversos? (A.8.3.)
- ¿Qué control trata la deriva de datos y el reentrenamiento? (A.6.2.6.)
- Como auditor, ¿qué evidencia pediría para A.6.2.5? (Plan de despliegue y criterios de lanzamiento aprobados.)
- ¿Qué norma referencia B.3.3 para el canal de denuncias? (ISO 37002.)

### 7.4 Errores frecuentes

- Tratar el Anexo B como requisitos auditables y exigir su cumplimiento literal.
- Copiar los 38 controles como "aplicables" sin trazabilidad a riesgos ni justificación de los excluidos.
- Confundir la evaluación del impacto (A.5, sobre personas y sociedad) con la evaluación del riesgo (6.1.2, sobre la organización).
- Olvidar que los dominios A.9 y A.10 aplican también a organizaciones que solo **usan** IA de terceros (por ejemplo, herramientas de IA generativa contratadas).
- No conservar las evaluaciones de impacto ni los registros de eventos durante el periodo definido (A.5.3, A.6.2.8).
- Redactar la política de IA sin mecanismo de excepciones ni alineamiento con las políticas de seguridad y privacidad (A.2.2, A.2.3).

## 8. Limitaciones del análisis

- **Fuente**: texto extraído por OCR del PDF UNE-ISO/IEC 42001:2025 (traducción oficial española de ISO/IEC 42001:2023). La extracción es de buena calidad; las tablas del Anexo A se recuperaron con columnas ligeramente desalineadas pero legibles en su totalidad.
- **Errores tipográficos del texto extraído**: la nota al pie de la serie ISO/IEC 5259 aparece como "ISO/IEC 52592)" (el "2)" es el superíndice de la nota 2, que indica que la serie estaba en elaboración: ISO/IEC DIS 5259-1 a -4 y CD 5259-5 en 2023). Aparecen algunas veces "AI" en lugar de "IA" y "sistemas e IA" por "sistemas de IA"; se han corregido en el resumen. La referencia "[28] DDI" corresponde a la especificación DDI-Lifecycle 3.3 de la DDI Alliance según la bibliografía.
- **Lenguaje verbal**: en el Anexo B todos los enunciados de control usan "debería", excepto B.7.6 que conserva "debe"; se interpreta como inconsistencia de traducción, ya que en la versión inglesa todos los controles del Anexo B se expresan con "should". En A.6.2.6 y A.6.2.8 la mezcla "debe / debería" dentro del mismo enunciado es fiel al original inglés ("shall ... should").
- **Fuente secundaria (traducción automática BSI, iso42001_es.txt)**: se consultó para A.6.2.6 y B.7.6, pero usa "debe/deberá" indistintamente para *shall* y *should*, por lo que no permite resolver dudas de lenguaje verbal; no se ha usado como base del resumen.
- **Evidencias de auditoría**: el apartado (d) de cada control es una aportación didáctica del analista; la norma no prescribe evidencias concretas más allá de la información documentada que el propio control menciona.
- **Nombres de archivos del corpus**: los nombres de los resúmenes relacionados (01 y 03 de la misma norma, y los de plantillas y guías) se indican de forma orientativa; en el momento de redactar este archivo la carpeta `resumenes/` no contenía aún otros archivos.
- **Cobertura**: se han cubierto A.1, la Tabla A.1 completa (38 controles), B.1 y todos los apartados B.2 a B.10.4, incluidas sus notas y apartados de "Otra información". No se cubren las cláusulas 4-10 ni los Anexos C y D, asignados a otros resúmenes.

## 9. Referencias

Normas y documentos citados en los Anexos A y B (con su número en la bibliografía de la norma):

- ISO 8000-2, *Data quality. Part 2: Vocabulary* [1] – procedencia de datos (B.7.5).
- ISO 9241-210, *Ergonomics of human-system interaction. Part 210: Human-centred design for interactive systems* [3] – diseño centrado en las personas (B.6.2.2, B.6.2.3).
- ISO/IEC TS 4213, *Assessment of machine learning classification performance* [8] – métrica F1 (B.6.2.6).
- ISO/IEC 5259 (todas las partes), *Data quality for analytics and machine learning (ML)* [9] – calidad y preparación de datos (B.4.3, B.7.4, B.7.6); en elaboración en 2023.
- ISO/IEC 5338, *AI system life cycle processes* [10] – procesos del ciclo de vida (B.6.2.2).
- ISO/IEC 19944-1, *Cloud computing and distributed platforms. Data flow, data categories and data use. Part 1* [12] – categorías de datos (B.4.3, B.7.3).
- ISO/IEC 22989, *Artificial intelligence concepts and terminology* – ciclo de vida, recursos y gestión de datos (B.4.5, B.6.1.3, B.6.2.7, B.7.2); es norma de consulta obligatoria de la cláusula 2.
- ISO/IEC 23053, *Framework for AI systems using ML* [13] – herramientas, deriva y preparación de datos (B.4.4, B.6.2.6, B.7.6).
- ISO/IEC 23894, *Guidance on risk management* [14] – evaluación de impacto en la gestión del riesgo (B.5.2).
- ISO/IEC TR 24027, *Bias in AI systems and AI aided decision making* [15] – sesgo en datos (B.7.4).
- ISO/IEC TR 24029-1, *Assessment of the robustness of neural networks. Part 1: Overview* [16] – robustez (B.6.2.4).
- ISO/IEC TR 24368, *Overview of ethical and societal concerns* [17] – impactos sociales (B.5.5).
- ISO/IEC 25024, *SQuaRE. Measurement of data quality* [18] – definición de calidad de datos (B.7.4).
- ISO/IEC 25059, *SQuaRE. Quality model for AI systems* [19] – criterios de rendimiento (B.6.2.6).
- ISO/IEC 27001, *Information security management systems. Requirements* [22] – gestión de incidentes de seguridad (B.8.4).
- ISO/IEC 27701, *Extension to ISO/IEC 27001 and 27002 for privacy information management* [21] – incidentes de privacidad y controles de IIP (B.8.4, B.10.2).
- ISO/IEC 29100, *Privacy framework* [23] – responsables y encargados de IIP (B.10.2).
- ISO 37002, *Whistleblowing management systems. Guidelines* [25] – canal de inquietudes (B.3.3).
- ISO/IEC 38507, *Governance implications of the use of AI by organizations* [27] – órgano de gobierno y política de IA (B.2.3).
- DDI-Lifecycle 3.3, Data Documentation Initiative Alliance [28] – metadatos estadísticos (B.7.6).
- Anexo C de ISO/IEC 42001 – objetivos organizacionales y fuentes de riesgo (B.6.1.2, B.9.3).
- Cláusulas de ISO/IEC 42001 referenciadas desde los anexos: 4.4, 5.2, 6.1.3, 6.1.4, 6.2, 7.4, 7.5.
