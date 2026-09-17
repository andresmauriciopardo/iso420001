---
titulo: "Alcance del producto: MVP, Fase 2 y Fase 3"
fecha: "2026-09-17"
estado: "borrador para revisión"
autor: "Agente Analista de Requisitos (Claude) para Andrés Mauricio Pardo"
nivel_fuente_de_verdad: 2
fuentes:
  - docs/product/PROMPT_REVIEW_AND_ADJUSTMENTS.md (A1 corte vertical del MVP, A2 hoja de ruta única, A9, A13)
  - docs/product/MASTER_ISO42001_PLATFORM_REQUIREMENTS.md (§46 E2E, §64 fases, §69 criterios de aceptación)
  - docs/product/MASTER_ORCHESTRATION_BMAD_OPENSPEC.md (§17 fases macro)
  - docs/product/REQUIREMENTS_ANALYSIS.md (IDs PR-*)
  - resumenes/01_ISO-IEC-42001-2023_Requisitos_Clausulas-4-10.md (tabla de información documentada obligatoria)
---

# Alcance del producto: MVP, Fase 2 y Fase 3

## 1. Principio de corte

El ajuste A1 resuelve la contradicción entre "no reduzcas el alcance" (P2 §73) y "no planifiques para siempre" (P1 §36) con una regla simple:

> **Arquitectura de dominio completa desde el inicio; implementación por cortes verticales.**

Esto significa que en el MVP existen los 21 bounded contexts del `DOMAIN_MAP.md`, el modelo de datos contiene todas las entidades del prompt maestro §5 (aunque algunas tablas queden vacías y sin UI) y el bus de eventos conoce todo el catálogo de §65, pero **solo se construye la UI, la API y los flujos** de las capacidades que una organización real necesita para operar un SGIA mínimo y llegar a una auditoría interna con evidencia.

El MVP no es un prototipo: cada acción recorre `UI → API → Domain Service → Database → Audit Log` (PR-NFR-017) y ningún botón es falso.

## 2. Hoja de ruta única (ajuste A2)

| Fase de este documento | Fases P1 §17 que absorbe | Fases P2 §64 que absorbe | Resultado observable |
|---|---|---|---|
| **MVP** | 0 Entorno, 1 Fundación, 2 Motor de normas, 3 Organización/contexto/alcance, 4 Portafolio de IA (núcleo), 5 Riesgo e impacto (núcleo), 6 SoA y controles, 7 Evidencia, 11 Auditoría/CAPA, 12 Informes (esenciales), 13 Revisión por la dirección, 14 Preparación para certificación (checklist) | 0, 1, 2, 3, 4, 5, 7 (evidencia y documentos sin generación), 9, 10 (parcial), 11 | Una organización crea su SGIA, inventaría IA, evalúa riesgo e impacto, aprueba la SoA, recoge evidencia, audita, corrige y revisa; obtiene los 4 informes esenciales. |
| **Fase 2** | 4 (grafo de activos, descubrimiento), 5 (tendencia, apetito), 8 Ciclo de vida y operaciones, 9 Políticas y documentos (generación), 10 Integraciones, 12 Informes (resto), 15 Hardening (residencia, antimalware) | 6, 7 (generación), 8, 10 (resto) | Conectores reales, ciclo de vida con puertas, incidentes, IA asistida, 25 informes, formación, calidad de datos, importación. |
| **Fase 3** | Extensiones | — | Agentes de IA, constructor de informes, búsqueda semántica, paquetes sectoriales, madurez, conectores futuros, ABAC. |

## 3. MVP: corte vertical

### 3.1 Capacidades incluidas (PR-CAP)

| Bloque | IDs incluidos | Notas de alcance dentro del MVP |
|---|---|---|
| Motor de normas | PR-CAP-001, 002, 003, 004*, 005*, 006, 007, 008* | Semilla con ISO/IEC 42001 completa (cláusulas 4-10 y 38 controles); 23894 y 19011 solo estructura de alto nivel con `SOURCE_DETAIL_REQUIRED`. Página de requisito con las relaciones ya disponibles (controles, riesgos, evidencia, hallazgos, CAPA). |
| SGIA | PR-CAP-011, 012, 013, 014, 015, 017, 018 | Política de IA como documento gestionado (sin generador LLM). Objetivos como registro simple con métrica manual. RACI básica. |
| Inventario de IA | PR-CAP-020, 021*, 026 | Sistema de IA, modelo, versión, proveedor, dataset y despliegue como entidades; `AIUseCase`, `AIProject`, `AIDataSource`, `AIIntegration` existen en el esquema pero sin UI dedicada. Tenant demo. |
| Riesgo | PR-CAP-030, 031*, 032, 034, 035, 036*, 037, 038 | Escalas 3x3/4x4/5x5 probabilidad × impacto; dimensiones adicionales en Fase 2. Disparador de reevaluación por cambio de modelo/versión y manual. |
| Impacto | PR-CAP-040, 041, 042*, 044 | Plantilla semilla de dimensiones; versionado; vínculo a riesgo. |
| SoA y controles | PR-CAP-050, 051, 052, 053, 054, 055*, 056, 058, 060 | Catálogo de 38 controles, aplicabilidad, activación por eventos, aprobación y versionado de la SoA, espacio de control con propietario, tareas, evidencia, hallazgos. |
| Evidencia | PR-CAP-061, 062, 063, 065, 066, 068*, 069 | Biblioteca, solicitudes, revisión, hash, vínculos, expiración; sin borrado físico. |
| Documentos | PR-CAP-070, 072* | Información documentada con versiones; exportación PDF y Markdown. |
| Tareas | PR-CAP-075 | Tareas desde control, riesgo, hallazgo, CAPA, solicitud de evidencia y revisión por la dirección. |
| Auditoría interna | PR-CAP-085*, 086, 087, 088, 089, 090*, 091 | Programa anual simple, auditoría con ciclo completo, evidencia por criterio, hallazgos, conclusión, seguimiento. |
| CAPA | PR-CAP-095, 096, 097 | No conformidad, causa raíz, acción, verificación de eficacia. |
| Revisión por la dirección | PR-CAP-100, 101 | Entradas agregadas automáticamente, acta formal. |
| KPI y puntuación | PR-CAP-105*, 106, 107 | Puntuaciones transparentes; dashboard ejecutivo; KPI de cumplimiento y riesgo. |
| Informes | PR-CAP-110*, 111 | Motor declarativo + 4 informes esenciales (SoA, cumplimiento por cláusula, registro de riesgos, preparación para auditoría), exportación PDF/CSV/JSON. |
| Preparación para certificación | PR-CAP-120 | Checklist de 13 preguntas y estados de preparación. |
| Integraciones | PR-CAP-125, 131 | SDK de conectores + adaptador mock etiquetado; sin proveedores reales. |
| Búsqueda y trazabilidad | PR-CAP-160, 165*, 166* | Búsqueda léxica; navegación de trazabilidad por enlaces; explicaciones desde semilla. |
| UI | PR-CAP-175* | Navegación de §37 mostrando solo módulos implementados. |

### 3.2 Transversales, no funcionales, reglas y dependencias del MVP

| Categoría | IDs incluidos |
|---|---|
| PR-XC | 001*, 002, 003, 004*, 005*, 006, 007, 008*, 009*, 010, 011*, 012, 013, 014, 015*, 016, 017, 019, 020 |
| PR-NFR | 001, 003, 004, 005 (diseño), 006, 007, 008, 009, 010, 012, 013, 014, 015, 016, 017, 018, 020, 021, 022 |
| PR-DR | 001, 002, 003, 004, 005, 006, 007*, 009, 010, 011, 015, 017, 018, 019, 020, 021, 022, 023, 024, 027, 028, 029, 030 |
| PR-EXT | 005, 006, 007*, 008, 009, 011*, 012 |

Total MVP: **141 requisitos** (73 CAP, 19 XC, 19 NFR, 23 DR, 7 EXT). El asterisco indica que el núcleo entra en MVP y el resto del enunciado se completa en Fase 2.

### 3.3 Secuencia propuesta de cambios OpenSpec del MVP [ASSUMPTION]

La secuencia es una propuesta del analista para la fase de épicas BMAD; no es vinculante hasta el checkpoint de planificación.

1. `foundation-project-bootstrap` — monorepo, stack ADR-001, CI, i18n base, design system.
2. `identity-tenancy-rbac` — PR-XC-001..005, PR-EXT-007.
3. `audit-log-hash-chain` — PR-XC-006, PR-DR-017.
4. `domain-events-outbox-jobs` — PR-XC-007, PR-XC-014, PR-XC-019.
5. `standards-engine-seed` — PR-CAP-001..008, PR-NFR-015, PR-NFR-020.
6. `aims-context-parties-scope` — PR-CAP-011..014, PR-DR-029.
7. `aims-policy-objectives-roles` — PR-CAP-015, 017, 018, 070.
8. `ai-inventory-core` — PR-CAP-020, 021, PR-DR-007.
9. `risk-engine-core` — PR-CAP-030..038, PR-DR-005, PR-DR-006.
10. `impact-assessment-core` — PR-CAP-040..044, PR-DR-028.
11. `soa-control-activation` — PR-CAP-050..060, PR-DR-001..004.
12. `evidence-library` — PR-CAP-061..069, PR-DR-011, PR-DR-015.
13. `tasks-approvals-notifications` — PR-CAP-075, PR-XC-008, PR-XC-009.
14. `internal-audit-core` — PR-CAP-085..091, PR-DR-022, PR-DR-023.
15. `capa-core` — PR-CAP-095..097, PR-DR-024.
16. `management-review-core` — PR-CAP-100, 101.
17. `scoring-kpi-dashboard` — PR-CAP-105..107, PR-DR-009, PR-DR-010.
18. `reporting-essentials` — PR-CAP-110, 111, PR-XC-011.
19. `certification-readiness-workspace` — PR-CAP-120, PR-DR-027.
20. `connector-sdk-mock` — PR-CAP-125, 131, PR-XC-015, PR-DR-021, PR-NFR-016.
21. `search-traceability-navigation` — PR-CAP-160, 165, 166, 175, PR-XC-010.
22. `demo-tenant-seed` — PR-CAP-026, PR-DR-020.
23. `mvp-hardening` — PR-NFR-001, 003, 007, 008, 014, 018; pruebas E2E del flujo de salida.

## 4. Criterios de salida del MVP

### 4.1 Flujo E2E mínimo

El MVP se considera completo cuando **una organización real** (no el tenant demo) puede ejecutar el flujo siguiente en la aplicación desplegada, con un usuario por rol, dejando registros persistentes, trazables y auditables en cada paso. Cada paso indica la condición de aceptación verificable y los requisitos que ejercita.

| Paso | Acción | Condición de aceptación | Requisitos ejercitados |
|---|---|---|---|
| 1 | Crear organización | Tenant creado con `data_region`, administrador invitado y activado; RLS impide ver datos de otro tenant (prueba automática). | PR-XC-002, 003, 004, 005 |
| 2 | Definir contexto | Al menos una cuestión interna, una externa, un rol de IA de la organización y una revisión registrada. | PR-CAP-011, PR-DR-029 |
| 3 | Definir partes interesadas | Al menos tres partes con necesidades, expectativas, influencia y fecha de revisión. | PR-CAP-012 |
| 4 | Definir alcance del SGIA | Alcance compuesto de al menos una unidad de negocio y un sistema de IA, con una exclusión justificada, aprobado y versionado (v1). | PR-CAP-013, PR-DR-017 |
| 5 | Inventariar sistemas de IA | Un sistema con modelo, versión, proveedor, dataset, uso previsto, mal uso previsible, supervisión humana y propietarios. | PR-CAP-020, 021 |
| 6 | Evaluar riesgos | Metodología aprobada; al menos dos riesgos con fuente del Anexo C, inherente y residual calculados con la escala del tenant; un plan de tratamiento aprobado; una aceptación con fecha de expiración. | PR-CAP-030..038, PR-DR-005, 006 |
| 7 | Evaluar impacto | Una evaluación con la plantilla semilla, al menos un hallazgo con mitigación, aprobada y enlazada al riesgo. | PR-CAP-040..044, PR-DR-028 |
| 8 | Completar la SoA | Los 38 controles con decisión de aplicabilidad; al menos un `Not Applicable` con justificación; SoA aprobada (v1). | PR-CAP-050..054, 060, PR-DR-001, 003, 004 |
| 9 | Activar controles | Los controles aplicables tienen propietario y generan tareas y solicitudes de evidencia por evento; los no aplicables no generan nada y siguen visibles. | PR-CAP-053, 055, 056, PR-DR-002 |
| 10 | Recoger evidencia | Al menos una evidencia por control activo probado, con hash, aceptada por un revisor distinto del que la subió, y una rechazada con motivo. | PR-CAP-061..066, PR-DR-011 |
| 11 | Auditoría interna | Programa anual; una auditoría con plan, criterios (requisitos y controles), evidencia por criterio, hallazgos de al menos dos tipos, conclusión e informe. | PR-CAP-085..089, PR-DR-022, 023 |
| 12 | Registrar hallazgo | Una no conformidad derivada de la auditoría, enlazada a requisito, control y evidencia. | PR-CAP-088, 095 |
| 13 | Gestionar CAPA | Corrección, causa raíz (5 porqués), acción correctiva con propietario y plazo, verificación de eficacia y cierre; el cierre sin verificación es rechazado. | PR-CAP-095..097, PR-DR-024 |
| 14 | Revisión por la dirección | Entradas agregadas automáticamente (riesgos, auditoría, CAPA, KPI), decisiones y acciones registradas, acta exportada a PDF. | PR-CAP-100, 101 |
| 15 | Informe de preparación | Informe de preparación para auditoría y checklist de certificación con puntuaciones explicadas (fórmula, numerador, denominador, exclusiones); ninguna pantalla dice "certificado". | PR-CAP-106, 111, 120, PR-DR-009, 010, 027 |

Este flujo es la prueba E2E principal (Playwright) del MVP y sustituye los pasos "Connect Azure/OpenAI/Anthropic → Discover → Validate" de P2 §69, que vuelven en Fase 2 (conflicto C10 del análisis).

### 4.2 Criterios técnicos de salida

1. Pruebas de dominio en verde: 38 controles con distribución A.2:3, A.3:2, A.4:5, A.5:4, A.6:9, A.7:5, A.8:4, A.9:3, A.10:3; 26 términos; 11 objetivos y 7 fuentes del Anexo C (PR-NFR-015).
2. Pruebas de seguridad en verde: acceso cross-tenant, escalada de privilegios, acceso a evidencia sin permiso, exposición de secretos, autorización a nivel de objeto (PR-NFR-014).
3. Cadena de hashes del registro de auditoría verificable por tenant; toda decisión de cumplimiento del flujo E2E aparece en el registro con actor, motivo y valor anterior (PR-XC-006, PR-DR-017).
4. UI completa en ES y EN; ninguna cadena canónica sin traducción (PR-XC-012).
5. Auditoría automática de accesibilidad WCAG 2.2 AA sin errores críticos en las pantallas del flujo E2E (PR-NFR-007).
6. Ningún botón o entrada de menú sin implementación detrás (PR-NFR-017, PR-CAP-175).
7. Semilla sin texto literal de normas; cada registro cita `resumenes/NN §sección` (PR-NFR-020).
8. SDK de conectores con adaptador mock etiquetado y pruebas de contrato; el estado del conector muestra `mock` (PR-CAP-131, PR-DR-021).
9. `docs/compliance/TRACEABILITY_MATRIX.md` actualizada con una fila por requisito MVP implementado (ver `TRACEABILITY_MODEL.md`).
10. Checkpoint de aceptación humana (P1 §40, A13) firmado antes de declarar el MVP cerrado.

## 5. Fase 2

### 5.1 Objetivo

Convertir el SGIA operable en un SGIA **conectado y asistido**: conectores reales, ciclo de vida con puertas, incidentes, cambios, excepciones, IA asistida con gobernanza propia, resto de informes, formación, calidad de datos e importación.

### 5.2 Requisitos

| Categoría | IDs |
|---|---|
| PR-CAP | 009, 016, 019, 022, 024, 027, 033, 039, 043, 045, 046, 047, 048, 057, 059, 064, 067, 071, 080, 081, 098, 108, 112, 114, 115, 116, 126, 127, 128, 129, 130, 135, 140, 150, 155, 170, 171, 172 |
| PR-XC | 018 |
| PR-NFR | 002, 011, 019 |
| PR-DR | 008, 012, 013, 014, 016, 026 |
| PR-EXT | 001, 002, 003, 004, 010, 015 |

Total Fase 2: **54 requisitos**. Además se completa la parte diferida de los requisitos `MVP*` (dimensiones adicionales de riesgo, roles por unidad de negocio y por sistema, rotación de credenciales, revisiones de acceso, SAML, exportación DOCX/XLSX, visualización gráfica de trazabilidad, explicaciones generadas por IA, importación).

### 5.3 Criterio de salida de Fase 2

El flujo E2E del MVP se extiende con: conectar un proveedor real (al menos uno de Azure AI Foundry, OpenAI o Anthropic validado contra la API real con credenciales de prueba), descubrir un candidato y validarlo, hacer avanzar un sistema por una puerta de despliegue bloqueada y desbloquearla con evidencia, registrar un incidente que dispare reevaluación de riesgo, generar una política con LLM y aprobarla tras revisión humana, y emitir el Certification Readiness Pack. Las funciones LLM aparecen en el inventario del tenant plataforma (PR-DR-026).

## 6. Fase 3

| Categoría | IDs |
|---|---|
| PR-CAP | 010, 023, 113, 117, 132, 145, 161 |
| PR-DR | 025 |
| PR-EXT | 013, 014 |

Total Fase 3: **10 requisitos** más ABAC (parte diferida de PR-XC-004). Fase 3 es opcional por bloques: cada elemento puede adelantarse si un cliente lo exige, sin afectar al núcleo.

## 7. Supuestos

Todos los supuestos requieren confirmación humana en el checkpoint de planificación.

1. [ASSUMPTION] El primer cliente objetivo es una organización mediana (50-500 empleados) que **usa** IA de terceros más que la desarrolla; por eso los controles A.9 y A.10 tienen la misma prioridad que A.6 en la semilla, y el ciclo de vida completo puede esperar a Fase 2.
2. [ASSUMPTION] El equipo de desarrollo es pequeño (1-3 personas más agentes), lo que justifica el monolito modular y pg-boss (A8).
3. [ASSUMPTION] La política de IA y los objetivos entran en MVP como gestión documental sin generación por LLM (conflictos C2 y C3).
4. [ASSUMPTION] El tenant demo se construye al final del MVP (cambio 22) y no sustituye a la organización real en los criterios de salida.
5. [ASSUMPTION] La autenticación local (email + contraseña con MFA TOTP) es suficiente para el MVP junto con OIDC-ready; SAML se difiere.
6. [ASSUMPTION] Los valores iniciales de RPO ≤ 1 h y RTO ≤ 8 h (PR-NFR-009) son objetivos de diseño, no compromisos contractuales.
7. [ASSUMPTION] La cobertura del 23 % de ISO/IEC 23894 en el corpus es suficiente para la semilla del MVP porque el Anexo C de 42001 cubre la taxonomía de fuentes de riesgo.
8. [ASSUMPTION] "Informes esenciales" de A1 se interpreta como los informes #2, #3, #5 y #13 de P2 §27.

## 8. Fuera de alcance explícito

### 8.1 Fuera del MVP (entra en fases posteriores)

- Conectores reales a Azure AI Foundry, OpenAI y Anthropic; descubrimiento automático.
- Cualquier función que llame a un LLM (generación de políticas, sugerencias, análisis de evidencia, resúmenes).
- Puertas de ciclo de vida, incidentes, gestión de cambios formal, excepciones formales.
- 21 de los 25 informes, constructor dinámico, programación de informes, dashboards por dominio.
- Formación y competencia, calidad de datos, importación masiva, `legal hold`, residencia de datos efectiva, antimalware.
- Agentes de IA, madurez, paquetes sectoriales, búsqueda semántica, ABAC.

### 8.2 Fuera del producto (en ninguna fase)

- **Declarar certificación**: la plataforma nunca emite ni sugiere un certificado ISO/IEC 42001; la certificación la realizan organismos acreditados (PR-DR-009).
- **Reproducir texto normativo**: la plataforma no muestra ni almacena texto literal de ISO, IEC o UNE (PR-NFR-020).
- **Sustituir el juicio humano**: ninguna función de IA aprueba, acepta ni cierra nada (PR-DR-008).
- **Ejecutar o alojar modelos de IA de los clientes**: la plataforma gobierna, no ejecuta cargas de inferencia.
- **Auditoría de tercera parte**: la plataforma apoya auditorías internas y la preparación; no actúa como organismo de certificación (ISO/IEC 42006 fuera del corpus, PR-EXT-013).
- **Asesoría legal**: el módulo regulatorio mapea obligaciones cargadas por la organización; no interpreta leyes.
- **Consumo en tiempo de ejecución de `resumenes/`**: el código nunca lee los resúmenes; solo la semilla versionada revisada por humanos (A4).
