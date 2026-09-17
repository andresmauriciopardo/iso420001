# INFORME QA – Corpus de 13 resúmenes ISO/IEC 42001

Fecha: 2026-09-17. Revisor: Claude (agente Revisor de Calidad). Carpeta revisada: `/Users/mauriciopardo/Documents/ISO42001/resumenes/`.

## 1. Tabla por archivo

| Archivo | Frontmatter OK | 9 secciones OK | Palabras (wc -w, tras QA) | Defectos hallados | Correcciones aplicadas |
|---|---|---|---|---|---|
| 01_ISO-IEC-42001-2023_Requisitos_Clausulas-4-10.md | Sí (tras corrección) | Sí | 10 317 | `documentos_relacionados_en_corpus` con descripciones genéricas; 9 tramos literales de 25-39 palabras de UNE-ISO/IEC 42001 (5.2, 6.1.1, 6.1.2, 6.2) | Lista sustituida por 13 nombres reales + 00_INDICE.md; parafraseados 5.2 (política, items a-d), criterios de riesgo 6.1.1, encabezado y items a-b de 6.1.2, características a-g de los objetivos 6.2 y planificación (qué/recursos/quién/cuándo/cómo) |
| 02_ISO-IEC-42001-2023_Anexo-A_Controles_y_Anexo-B_Guia-Implementacion.md | Sí (tras corrección) | Sí | 13 539 | Lista de relacionados con nombres inventados ("01_ISO-IEC-42001-2023_Clausulas-4-10.md", "03_..._Anexo-C_Objetivos-y-fuentes...") y descripciones; **safety/security etiquetados al revés** en B.2.3 (línea 112) y B.3.2 (línea 143) respecto a la convención UNE; controles A.4.6 y A.6.2.6 transcritos literalmente (35-36 palabras) | Lista corregida (10 nombres + índice); "seguridad (*security*), protección (*safety*)" en ambas líneas; A.4.6 y A.6.2.6 parafraseados |
| 03_ISO-IEC-42001-2023_Anexos-C-D_Objetivos-Fuentes-de-Riesgo-y-Sectores.md | Sí (tras corrección) | Sí | 7 800 | Relacionados genéricos; frase de D.2 (enfoque holístico) literal de 30 palabras | Lista corregida (10 + índice); frase parafraseada |
| 04_ISO-IEC-23894-2023_Gestion-de-Riesgos-de-IA.md | Sí (tras corrección) | Sí | 4 965 | Relacionados genéricos; en Tabla 2 (factores humanos) traducía *safety* como "seguridad" y *security* como "seguridad de la información", contra la convención del corpus | Lista corregida (12 + índice); "protección (*safety*), seguridad (*security*)" |
| 05_ISO-19011-2026_Directrices-Auditoria-Sistemas-de-Gestion.md | Sí (tras corrección) | Sí | 12 283 | Solo relacionados genéricos (archivo declarado estable; contenido no tocado). "27 términos (3.1-3.27)" verificado contra iso19011.txt: correcto | Lista corregida (11 + índice) |
| 06_Guia-Implementacion-Inicial-ISO42001_Impulsa360.md | Sí (tras corrección) | Sí | 5 637 | Faltaba 00_INDICE.md y 13; **"38 en 9 objetivos de control"** (Anexo A tiene 9 dominios pero 10 objetivos: A.6 tiene A.6.1 y A.6.2) | Lista completada (13 + índice); texto corregido a "38 controles en 9 dominios, A.2 a A.10, con 10 objetivos de control" |
| 07_Catalogo-Certificaciones-PECB_Impulsa360.md | Sí (tras corrección) | Sí | 4 689 | Faltaba 00_INDICE.md y 13; `fecha_publicacion_fuente` es una frase larga en vez de "no consta" (tolerado) | Lista completada (7 + índice) |
| 08_Plantilla_Diagnostico-Analisis-de-Brechas-ISO42001.md | Sí (tras corrección) | Sí | 4 600 | Relacionados mezclados (1 real, 6 genéricos) | Lista corregida (9 + índice) |
| 09_Plantilla_Declaracion-de-Aplicabilidad-SOA-ISO42001.md | Sí (tras corrección) | Sí | 4 400 | Relacionados mezclados; "37 controles Si + A.2.2 No" verificado: suma 38, coherente | Lista corregida (9 + índice) |
| 10_Plantilla_Analisis-de-Riesgo-SGIA.md | Sí (tras corrección) | Sí | 4 073 | Lista de relacionados demasiado corta (solo 11 y 12, sin 01/02/04/08/09) y sin comillas | Lista ampliada (10 + índice), entrecomillada |
| 11_Plantilla_Actividad2_FODA-vs-Riesgo_Contexto-Organizacion.md | Sí (tras corrección) | Sí | 3 418 | Lista corta (solo 10 y 12) | Lista ampliada (7 + índice) |
| 12_Plantilla_Actividad3_Stakeholders_Partes-Interesadas.md | Sí (tras corrección) | Sí | 2 983 | Lista corta (solo 10 y 11) | Lista ampliada (6 + índice) |
| 13_Glosario-Bilingue-ES-EN_ISO42001_y_Nota-Version-Inglesa.md | Sí (tras corrección) | Sí | 9 315 | Relacionados expresados como "resumen de <PDF>", no como .md | Lista corregida (11 + índice) |

Total corpus: 88 019 palabras. Comprobaciones automáticas realizadas en los 13: los 12 campos del frontmatter presentes y YAML parseable (PyYAML); los 9 H2 exactos y en orden; 0 bloques de código sin cerrar; 0 encabezados duplicados; todas las tablas con fila separadora y número de columnas constante; ningún blockquote de más de 3 líneas; `archivo_fuente` coincide con un nombre real del material (08-12 anteponen la subcarpeta "Plantillas para ejercicios y proyectos/", lo cual es una ruta real y se conservó).

## 2. Contradicciones entre archivos y resolución

| Hecho | Estado | Resolución |
|---|---|---|
| 38 controles en Anexo A, distribución A.2:3, A.3:2, A.4:5, A.5:4, A.6:9 (A.6.1:2 + A.6.2:7), A.7:5, A.8:4, A.9:3, A.10:3 | Coherente en 02, 08, 09, 13 (02 lista los 38 con títulos UNE; 13 suma explícitamente 38) | Sin cambios |
| 9 dominios vs. objetivos de control | **Contradicción**: 06 decía "9 objetivos de control"; 13 dice 10 | Fuente UNE: 10 líneas "Objetivo:" en Tabla A.1 (A.6 tiene dos). Corregido 06 |
| 26 términos en cláusula 3 (3.1-3.26) | Coherente en 01 y 13 (13 desmiente expresamente la cifra de 31). Las menciones a "3.28" son la referencia a ISO/IEC 27000:2018, correctas | Sin cambios |
| Títulos A.7.2 / A.7.3 | 02 usa el literal UNE ("Datos para el desarrollo y la mejora del sistema de IA", "Adquisición de los datos"); 09 usa "Datos para el desarrollo y mejora del sistema de IA" / "Adquisición de datos". Equivalentes; 08 y 09 documentan correctamente la errata de las plantillas (copian A.8.2/A.8.3) | Sin cambios |
| 11 objetivos y 7 fuentes de riesgo (Anexo C) | Coherente en 03 y 04; 01, 02 y 06 no dan cifras | Sin cambios |
| Traducción UNE: safety = protección, security = seguridad | 03 y 13 lo explican bien. **02 lo invertía** en dos líneas y **04 usaba "seguridad" para safety** | Verificado en une_iso42001_es.txt (B.2.3: "la calidad, la seguridad, la protección y la privacidad"; B.3.2 lista "la seguridad; la protección"). Corregidos 02 y 04 |
| ISO/IEC 42001 publicada 2023-12 | Coherente (01, 02, 06, 13); 01-03 fechan además la adopción UNE en 2025-04 | Sin cambios |
| ISO 19011:2026 cuarta edición | Solo 05 lo afirma; 01, 06, 07 citan "19011:2026" sin edición. Ningún archivo cita 2018 como vigente | Sin cambios |
| ISO/IEC 23894 solo parcial | 04 lo documenta con detalle (12 páginas de vista previa; disponibles cláusulas 1-4, 5.1-5.3 y 5.4.1 truncada). 03 remite a 23894 como fuente del Anexo C sin afirmar disponibilidad completa | Sin cambios |

## 3. Transcripciones literales

Se midieron secuencias de 14+ palabras idénticas (normalizadas) entre cada resumen y su texto fuente. Antes de QA: 01 tenía 51 tramos (máximo 39 palabras), 02 tenía 32 (máx. 36), 03 tenía 35 (máx. 30). Tras las paráfrasis: 01 → 46 tramos (máx. 33, que corresponde a una frase de "información documentada exigida" seguida de un encabezado, es decir, no es un párrafo continuo), 02 → 31 (máx. 21), 03 → 34 (máx. 22), 13 → 8 (máx. 24). Los tramos restantes son enunciados de requisitos de una sola frase, títulos de cláusulas y entradas bibliográficas, dentro de lo permitido por la especificación ("citas breves de una frase"). 04 y 05 no tienen tramos literales de 14+ palabras. 07 comparte listas de "público objetivo" con el catálogo comercial (no es norma ISO; sin restricción).

## 4. Observaciones no corregidas (menores)

- 07 `fecha_publicacion_fuente` y 08-11 incluyen explicaciones entre paréntesis en vez de "YYYY" o "no consta" puro; son cadenas YAML válidas y aportan contexto.
- 08-12 anteponen "Plantillas para ejercicios y proyectos/" en `archivo_fuente`; útil para localizar el archivo, no se modificó.
- 00_INDICE.md aún no existe; lo creará el orquestador. Todos los demás nombres de la lista fueron validados contra la carpeta.

## 5. Valoración global como base de conocimiento para una IA

**Puntuación: 8,5 / 10.**

Fortalezas: cobertura completa de la estructura de cada fuente con numeración original; distinción sistemática debe/debería; tablas de términos bilingües; secciones 6 y 7 con vínculos concretos entre cláusulas, controles y hojas de plantilla; las erratas de las plantillas (A.7.2/A.7.3, A.6.2.8) y la disponibilidad parcial de 23894 están documentadas, lo que evita alucinaciones aguas abajo; frontmatter uniforme y ahora enlazado con nombres reales, apto para RAG con metadatos.

Debilidades: 01 y 02 siguen muy pegados al texto UNE en frases de requisito (riesgo residual de licencia, aunque dentro del umbral de una frase); la cobertura de 23894 es solo ~23 % del cuerpo técnico, por lo que las preguntas sobre el proceso 6.x de esa norma no podrán responderse desde el corpus; 04 y 05 traducen del inglés sin versión oficial española, lo que puede generar variantes terminológicas frente a 01-03; falta el índice 00 que consolide el mapa del corpus.
