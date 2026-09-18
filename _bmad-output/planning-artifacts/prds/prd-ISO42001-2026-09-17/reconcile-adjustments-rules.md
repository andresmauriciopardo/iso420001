---
title: "Reconciliación de insumos: ajustes A1-A13, reglas de CLAUDE.md, P1 §13/§17/§37-38 y P2 §46/§70 frente al PRD y la hoja de ruta"
status: draft
created: 2026-09-17
project: ISO42001
step: "BMAD PRD Finalize — paso 2 (reconciliación de insumos)"
inputs:
  - docs/product/PROMPT_REVIEW_AND_ADJUSTMENTS.md (A1-A13)
  - CLAUDE.md (reglas de dominio, idioma, aprobación humana, prohibiciones)
  - docs/product/MASTER_ORCHESTRATION_BMAD_OPENSPEC.md §13, §17, §37-§38
  - docs/product/MASTER_ISO42001_PLATFORM_REQUIREMENTS.md §46 (E2E), §70 (principio rector)
targets:
  - _bmad-output/planning-artifacts/prds/prd-ISO42001-2026-09-17/prd.md
  - _bmad-output/planning-artifacts/prds/prd-ISO42001-2026-09-17/addendum.md
  - docs/product/ROADMAP.md
---

# Reconciliación de insumos frente al PRD y la hoja de ruta

Estados usados: **covered** (el efecto es visible donde el insumo lo exige), **weakened** (presente pero incompleto, degradado o con inconsistencia interna), **missing** (no aparece ni se justifica su omisión), **contradicted** (el PRD o la hoja de ruta dicen lo contrario).

## Resumen de recuento

| Insumo | covered | weakened | missing | contradicted | total |
|---|---|---|---|---|---|
| 1. PROMPT_REVIEW_AND_ADJUSTMENTS.md (A1-A13) | 12 | 1 | 0 | 0 | 13 |
| 2. CLAUDE.md (6 reglas + idioma + 11 aprobaciones + 6 prohibiciones) | 21 | 1 | 2 | 0 | 24 |
| 3. P1 §13 (24 áreas del PRD) | 24 | 0 | 0 | 0 | 24 |
| 4. P1 §17 / §37 / §38 (fases, orden, primeros cambios) | 6 | 2 | 0 | 0 | 8 |
| 5. P2 §70 (principio rector) | 4 | 1 | 0 | 0 | 5 |
| 5. P2 §46 (18 pasos E2E + categorías de prueba) | 15 | 3 | 1 | 0 | 19 |
| **Total** | **82** | **8** | **3** | **0** | **93** |

---

## 1. PROMPT_REVIEW_AND_ADJUSTMENTS.md — ajustes A1-A13

| Ítem | Dónde en el insumo | Estado | Dónde en el PRD / ROADMAP | Corrección sugerida |
|---|---|---|---|---|
| A1 — Arquitectura de dominio completa + MVP vertical; diferir conectores (SDK + mock primero), constructor de informes, búsqueda semántica, paquetes sectoriales, madurez, agentes, 25 informes | §3 A1; "Dónde": PRODUCT_SCOPE.md, PRD (fuera de alcance del MVP), roadmap | covered | PRD §1 (Alcance), §11.1 (tabla MVP, 21 contextos), §11.2 (F2: FR-117 a FR-119, FR-107), §11.3 (F3: FR-023, FR-038, FR-104, FR-111, FR-124), §8.7, FR-114/FR-115 (SDK + mock en MVP), FR-106 (los 4 informes esenciales que A1 nombra). ROADMAP §1, §3 (24 cambios), §4, §5 | Ninguna. Nota: el PRD amplía la lista MVP de A1 con política y objetivos (D-1), desviación justificada y marcada como aprobación humana pendiente (PRD §17, ROADMAP §6). |
| A2 — Una única hoja de ruta; fases de P1 como referencia, P2 §64 mapeado | §3 A2 | covered | PRD §11 (remite a ROADMAP.md), ROADMAP §1 y §2 (tabla P1 §17 0-15 ↔ P2 §64 0-12 ↔ tren ↔ sección PRD) | Ninguna. |
| A3 — Un solo árbol `docs/`; en raíz solo `README.md` y `CLAUDE.md` | §3 A3 | covered | PRD NFR-022 ("árbol único `docs/` (A3)"), §15.2; ROADMAP cambio 24 (documentación viva), regla 4 (`docs/compliance/TRACEABILITY_MATRIX.md`) | Opcional: NFR-022 es "Should"; subir a Must si se quiere que el layout sea criterio de salida (§16.2 ya exige la matriz, no el resto de documentos). |
| A4 — Monorepo `resumenes/` (solo lectura) + `apps/`/`packages/`; semilla desde `packages/standards-seed/`; sin importación en tiempo de ejecución; material licenciado fuera de Git | §3 A4 | covered | PRD §6.2 (descripción), §8.2 ("Ningún módulo … lee `resumenes/` en tiempo de ejecución (A4)"), §8.3, §12 (no-objetivo), NFR-020; ROADMAP cambio 1 (monorepo) y cambio 6 (`packages/standards-seed/`) | Ninguna. |
| A5 — Documentos en español; identificadores/enums/API/commits en inglés; UI bilingüe desde la fundación (i18n NFR fase 1); glosario `resumenes/13` fuente de terminología | §3 A5; "Dónde": CLAUDE.md, config OpenSpec/BMAD, NFR i18n del PRD, GLOSSARY.md | covered | PRD redactado en español con identificadores en inglés; §5 (trampas de traducción vinculantes), §6 convenciones ("enumeraciones canónicas en inglés"), FR-132, NFR-018 (MVP), §8.5; addendum §C. ROADMAP cambio 1 (i18n base ES/EN en la fundación) | Ninguna. |
| A6 — Semilla solo con identificador, título, paráfrasis, tipo (4 valores), evidencia esperada, referencias; nunca texto literal; cada registro cita `resumenes/NN §sección` | §3 A6 | covered | PRD FR-017 (`record_type` 4 valores), FR-018, NFR-020, §8.2 (campos mínimos + `standard_version`), §8.3, §16.2 criterio 7; addendum §D.5 | Ninguna. |
| A7 — `StandardVersion` con 42001:2023 + Amd 1:2024 (cambio climático 4.1/4.2), UNE 42001:2025, ISO 19011:2026, 23894:2023 parcial (cl. 1-4, 5.1-5.3), 42006 como dependencia pendiente | §3 A7 | covered | PRD FR-015, §8.1 (relación `identical_adoption`, cláusulas disponibles de 23894, 42006 sin contenido), FR-025 (frases Amd 1), PR-EXT-013, R8, Q8; ROADMAP §7 | Ninguna. |
| A8 — ADR-001 stack, ADR-002 RLS + `tenant_id`, ADR-003 monolito modular + outbox, ADR-004 pg-boss, ADR-005 cadena de hashes | §3 A8 | covered | PRD §0 (remite a brief addendum §E), FR-002, FR-007, §8.6 (outbox, ADR-003/ADR-005), §12 (sin esquema por tenant ni microservicios), §13.4 (pg-boss); addendum §D.6. ROADMAP cambio 1 (stack ADR-001), cambio 5 (pg-boss, outbox) | Ninguna. ADR-004 solo aparece como supuesto §13.4 y en ROADMAP cambio 5; aceptable porque los ADR viven en el brief. |
| A9 — Vacíos añadidos: plataforma como sistema de IA (A.6/A.8/A.9); WCAG 2.2 AA; residencia, copias/DR, `legal hold` con RPO/RTO fijados; coste y límites LLM + abstracción de proveedor; pruebas de dominio con fixtures (38 controles, 26 términos); pruebas de contrato con mock, nunca "validado" sin credenciales | §3 A9 | covered | FR-130 + PR-DR-026 (F2, coherente con "sin LLM en MVP"); NFR-009 (MVP); NFR-015, NFR-008 (RPO ≤ 1 h, RTO ≤ 8 h, `[ASSUMPTION]`, D-7/Q6), FR-008 (`legal hold` F2); FR-129 (F2); NFR-024 y FR-018 (MVP); FR-115 + PR-DR-021 (MVP). ROADMAP F2-1, F2-3, F2-23, F2-27 | Ninguna de fondo. NFR-008 es "Should" y provisional: ratificar en checkpoint (D-7) para que "el PRD los fija" quede cerrado. |
| A10 — Trazabilidad: anteponer el nodo `Documento fuente (resumenes/NN, sección)` | §3 A10 | covered | FR-016 ("nodo [0] … A10"), FR-125 (primer nodo), §8.1 (`SeedRecordSource`), FR-018, NFR-020, §16.2 criterio 7; ROADMAP cambio 6 criterio de salida | Ninguna. |
| A11 — Verificación de herramientas (BMAD v6.12.0, OpenSpec 1.13.1, config en español, `_bmad-output/`) | §3 A11 | covered | Fuera del ámbito del PRD (configuración). Consistencia verificada: PRD frontmatter (modo headless bmad-prd, salida en `_bmad-output/planning-artifacts/`), §0 (consumo por `bmad-ux`, `bmad-architecture`, `bmad-create-epics-and-stories`), ROADMAP regla 5 (`/opsx:propose`) | Ninguna. |
| A12 — Dos puntos de aprobación humana añadidos: antes de **publicar** (PR, push a rama compartida, despliegue) y antes de **dependencias copyleft o de terceros no auditados** | §3 A12 | weakened | Copyleft: NFR-003 ("sin aprobación humana (A12)"), PR-EXT-011. **Publicar**: no aparece ni en PRD §17 ni en ROADMAP §6 ("Puntos de aprobación humana en la hoja de ruta"), que sí lista "antes de cada migración irreversible" | Añadir fila a ROADMAP §6: "Antes de publicar (PR, push a rama compartida, despliegue de cada cambio) — `CLAUDE.md`, A12". Opcional: nota en PRD §16.3 (definición de hecho) de que el cierre de un cambio pasa por PR aprobado por humano. |
| A13 — Ejecutar el método con agentes en paralelo; detenerse en el checkpoint I; `foundation-project-bootstrap` solo tras aceptación humana | §3 A13 | covered | PRD §13 (intro), §15.2, §16.2 criterio 10, R9; ROADMAP regla 3, cambio 1 ("Depende de: Checkpoint de planificación aceptado"), hitos §3, §6 fila 1 | Ninguna. |

---

## 2. CLAUDE.md — reglas no negociables, idioma, aprobación humana, prohibiciones

### 2.1 Reglas del dominio de cumplimiento (6)

| Ítem | Dónde en el insumo | Estado | Dónde en el PRD / ROADMAP | Corrección sugerida |
|---|---|---|---|---|
| R1 — Nunca inventar requisitos, identificadores, texto normativo, obligaciones regulatorias ni capacidades de API; `SOURCE_DETAIL_REQUIRED` | "Reglas del dominio", viñeta 1 | covered | PR-DR-019 (§9), FR-018, FR-128 (validación de referencias contra el catálogo), FR-117 ("No se asume la disponibilidad de ningún dato hasta verificarlo"), FR-022 ("La plataforma no interpreta leyes"), §5 (`SOURCE_DETAIL_REQUIRED`), §3 principio 6, R2/R6 | Ninguna. |
| R2 — Distinguir siempre `normative_requirement`, `implementation_guidance`, `recommended_practice`, `organizational_decision`, `product_requirement`, `technical_design_decision` | viñeta 2 | weakened | Tipos normativos: FR-017, PR-DR-018, §5 (`record_type`). Tipos de decisión: solo en addendum §C ("Clasificación de decisión"). El PRD no clasifica con esos tipos sus decisiones §17 (D-1…D-9) ni sus supuestos §13/§19; usa `[ASSUMPTION]` y "interpretación normativa" (D-2) | Añadir columna "Tipo" a la tabla PRD §17 (p. ej. D-1, D-6, D-8, D-9 = `organizational_decision`; D-3, D-4, D-5, D-7 = `technical_design_decision`; D-2 = interpretación normativa → `organizational_decision` con aprobación obligatoria) y una frase en §0 que declare que todo FR/NFR es `product_requirement`. |
| R3 — Hechos verificados: 38 controles (A.2:3, A.3:2, A.4:5, A.5:4, A.6:9, A.7:5, A.8:4, A.9:3, A.10:3); cláusula 3 = 26 términos; Anexo C = 11 objetivos y 7 fuentes; safety = protección, security = seguridad | viñeta 3 | covered | FR-018, FR-059 (prueba bloqueante), NFR-024, §16.2 criterio 1, FR-043 (C.3.1-C.3.7, C.2.1-C.2.11), FR-051 (safety ≠ security), §5 trampas; ROADMAP cambio 6 criterio de salida | Ninguna. |
| R4 — La SoA gobierna los flujos: un control no aplicable no genera tareas, evidencia recurrente ni métricas activas; permanece visible y auditable | viñeta 4 | covered | PR-DR-001 a PR-DR-004, FR-060 (38 siempre visibles), FR-061, FR-064, FR-067, FR-100 (exclusiones), FR-102, FR-103; UJ-1; §16.1 paso 9; ROADMAP cambio 12 | Ninguna. |
| R5 — Riesgo tratado ≠ plan; evidencia subida ≠ aceptada; la IA sugiere, una persona aprueba; nunca "certificado ISO" | viñeta 5 | covered | PR-DR-005/006 + FR-044/045/046; PR-DR-011 + FR-072/074; PR-DR-008 + FR-128 (F2) y, en MVP, FR-020/FR-039 (sugerencia, no cambio automático); PR-DR-009 + FR-100 (prueba de cadenas), FR-113, §12, SM-C4 | Ninguna. |
| R6 — Toda puntuación expone fórmula, numerador, denominador, exclusiones y registros fuente | viñeta 6 | covered | PR-DR-010, FR-100 (`ScoreSnapshot`), FR-102, FR-113, FR-028, FR-099 ("no hay cifras sin fuente"), SM-4; ROADMAP cambio 18 | Ninguna. |

### 2.2 Idioma

| Ítem | Dónde en el insumo | Estado | Dónde en el PRD / ROADMAP | Corrección sugerida |
|---|---|---|---|---|
| Documentos en español; código, identificadores, enumeraciones, API y commits en inglés; UI bilingüe ES/EN desde la fundación; terminología de `resumenes/13` | sección "Idioma" | covered | PRD y ROADMAP en español; entidades, eventos, permisos, enums y nombres de cambio en inglés; §5, §6 convenciones, §8.5, FR-132 (términos prohibidos en ES), NFR-018 (MVP, 100 % de claves), SM-6; ROADMAP cambio 1 (i18n base) | Ninguna. |

### 2.3 Aprobación humana obligatoria antes de… (11 puntos)

| Ítem | Dónde en el insumo | Estado | Dónde en el PRD / ROADMAP | Corrección sugerida |
|---|---|---|---|---|
| H1 — Aceptar supuestos de producto mayores | "Aprobación humana obligatoria" | covered | PRD §13 ("Todos requieren confirmación humana en el checkpoint"), §19 (índice A1-A37), §17; ROADMAP §6 fila 1 | Ninguna. |
| H2 — Bloquear la arquitectura | ídem | covered | PRD §15.2, §17 (D-3, D-4 "debe fijarse antes de…"); ROADMAP §6 fila 1 (checkpoint incluye arquitectura), filas "antes del cambio 6 / 9 y 16" | Ninguna. |
| H3 — Migraciones irreversibles | ídem | covered | NFR-019 ("las migraciones irreversibles exigen aprobación humana"); ROADMAP §6 "Antes de cada migración irreversible" | Ninguna. |
| H4 — Cambios de arquitectura de seguridad | ídem | missing | No aparece como punto de aprobación en PRD §17 ni en ROADMAP §6. NFR-001/NFR-002/NFR-005 fijan controles pero no el punto de aprobación para cambiarlos | Añadir a ROADMAP §6: "Antes de cualquier cambio de arquitectura de seguridad (modelo de tenencia, RLS, gestor de secretos, autenticación, cadena de hashes) — `CLAUDE.md`". Opcional: frase en PRD §7.1 (convenciones). |
| H5 — Credenciales de producción | ídem | covered | FR-115 ("aprobación humana previa"), FR-116; ROADMAP §4 criterio de salida, §6 "Antes de F2-5…F2-7: credenciales reales del proveedor" | Ninguna. |
| H6 — Integraciones contra cuentas reales | ídem | covered | FR-115, PR-DR-021, NFR-024; ROADMAP §4, §6 | Ninguna. |
| H7 — Cambiar interpretación normativa | ídem | covered | PRD §8.2 ("Cambiar una paráfrasis o un identificador … exige aprobación humana"), §17 D-2 ("punto de aprobación humana obligatorio"), §17 intro; ROADMAP §6 "Antes del cambio 6" | Ninguna. |
| H8 — Declarar un requisito implementado | ídem | covered | PRD §16.3 (definición de hecho; "Implementado sin evidencia es `unverified`"); ROADMAP §6 "Antes de declarar cualquier requisito implementado" | Ninguna. |
| H9 — Borrar o alterar evidencia | ídem | covered | FR-008 (sin borrado físico en MVP; borrado solo mediante flujo aprobado en F2; `legal hold`), FR-007 (sin update/delete), FR-073 (supersede), PR-DR-015, UJ-3 caso límite | Opcional: fila explícita en ROADMAP §6 para F2-23 (`retention-legal-hold`). |
| H10 — Publicar (PR, push a rama compartida, despliegue) | ídem | missing | Ausente en PRD y ROADMAP §6 (ver A12) | Igual que A12: fila en ROADMAP §6. |
| H11 — Añadir dependencias con licencia copyleft | ídem | covered | NFR-003, PR-EXT-011; ROADMAP cambio 1 (CI con escaneo de dependencias) | Ninguna. |

### 2.4 Prohibido (6)

| Ítem | Dónde en el insumo | Estado | Dónde en el PRD / ROADMAP | Corrección sugerida |
|---|---|---|---|---|
| P1 — Codificar antes del checkpoint de planificación aceptado | "Prohibido" | covered | PRD §15.2, §16.2 criterio 10; ROADMAP regla 3, cambio 1 dependencia, §6 fila 1 | Ninguna. |
| P2 — Un cambio "build-entire-platform" | ídem | covered | ROADMAP regla 1 (24 cambios en MVP), §7 ("Ceremonia"); addendum §E ("Un solo cambio OpenSpec mvp-core: prohibido") | Ninguna. |
| P3 — UI con botones falsos | ídem | covered | NFR-011, FR-131 ("sin botones falsos"; entradas no implementadas no se muestran), §16.2 criterio 6; ROADMAP cambio 22 criterio de salida | Ninguna. |
| P4 — Afirmar que un conector funciona sin validarlo contra el proveedor real | ídem | covered | PR-DR-021, FR-115 (estados `mock`/`configured`/`validated`), NFR-024, FR-120; ROADMAP F2-5…F2-7 criterio de salida | Ninguna. |
| P5 — Borrar registros de cumplimiento en silencio | ídem | covered | PR-DR-015, FR-008, NFR-016, FR-007, FR-061 (cancelación con motivo) | Ninguna. |
| P6 — Pegar el prompt maestro completo en el contexto de OpenSpec o de cada tarea | ídem | covered | Proceso, no producto. El PRD cita P2 "por secciones" (frontmatter) y P1 solo §13/§17/§37-38; ROADMAP hace lo mismo | Opcional: recordar en ROADMAP §1 (reglas) que cada `/opsx:propose` carga solo el PRD y la arquitectura necesaria (`CLAUDE.md` "Cómo trabajar un cambio"). |

---

## 3. P1 §13 — las 24 áreas del PRD (verificación de la tabla de PRD §0)

| Ítem | Dónde en el insumo | Estado | Dónde en el PRD / ROADMAP (verificado) | Corrección sugerida |
|---|---|---|---|---|
| 1 Executive summary | §13 ítem 1 | covered | §1: tres párrafos sustantivos (norma, tesis, alcance MVP/F2/F3) + cifras del documento | Ninguna. |
| 2 Problem statement | ítem 2 | covered | §2.1-2.4: dolor (tres fallos citando `resumenes/01 §7`), cómo se afronta hoy, quién sufre, por qué ahora | Ninguna. |
| 3 Product vision | ítem 3 | covered | §3: cita P2 §70, tesis, tabla del ciclo (8 etapas ↔ cláusulas ↔ FR), 7 principios, visión a tres años | Ninguna. |
| 4 Personas | ítem 4 | covered | Inline en UJ-1…UJ-7 (§4.3): 7 personas con nombre, rol, contexto y actitud (Lucía, Rodrigo, Carmen, Javier, Marta, Sofía, Diego); §2.3 y §4.2 (no usuarios). Sin sección independiente, justificado en §0 y addendum §E | Opcional para `bmad-ux`: tabla resumen persona → rol → UJ → FR clave (una fila por persona) al inicio de §4.3. |
| 5 User journeys | ítem 5 | covered | §4.3: 7 trayectorias con estado de entrada, camino, clímax, resolución y caso límite; mapa UJ → FR | Ninguna. |
| 6 Functional requirements | ítem 6 | covered | §6: FR-001 a FR-132 en 22 capacidades, cada FR con PR-* cubiertos, fase, UJ y consecuencias verificables | Ninguna. |
| 7 Non-functional requirements | ítem 7 | covered | §7: NFR-001 a NFR-024 en 10 familias con umbrales etiquetados | Ninguna. |
| 8 Security requirements | ítem 8 | covered | §7.1 (NFR-001 a NFR-005), §6.1 FR-002 a FR-007, FR-116, NFR-002; addendum §B (matriz rol × permiso) | Ninguna. |
| 9 AI governance requirements | ítem 9 | covered | §6.21 (FR-127 a FR-130), §9 PR-DR-008 y PR-DR-026, §3 principios 4 y 7. Además, el dominio completo §6.3-§6.15 es gobernanza de IA | Opcional: aclarar en §0 que "gobernanza de IA" en P1 §13 se lee como gobernanza de la IA del propio producto (§6.21) y que la gobernanza del SGIA cliente es todo §6. |
| 10 Integration requirements | ítem 10 | covered | §6.19 (FR-114 a FR-122), §15.1 PR-EXT-001 a 003, 014 | Ninguna. |
| 11 Reporting requirements | ítem 11 | covered | §6.17 (FR-105 a FR-112), §6.16 (FR-100 a FR-104) | Ninguna. |
| 12 Audit requirements | ítem 12 | covered | §6.13 (FR-087 a FR-093, auditoría del SGIA) y FR-007 + §7.7 NFR-017 (auditabilidad de la aplicación) | Ninguna. |
| 13 Evidence requirements | ítem 13 | covered | §6.9 (FR-071 a FR-078) | Ninguna. |
| 14 Risk requirements | ítem 14 | covered | §6.5 (FR-041 a FR-049) y §6.6 (impacto, FR-050 a FR-054) | Ninguna. |
| 15 Lifecycle requirements | ítem 15 | covered | §6.7 (FR-055 a FR-058, todos F2 con modelo y campo de etapa en MVP), FR-052 | Ninguna; la deferral está justificada por A1 (§6.7 descripción, ROADMAP §2 fila 8). |
| 16 Administration requirements | ítem 16 | covered | §6.1 (FR-001 a FR-014) | Ninguna. |
| 17 UX requirements | ítem 17 | covered | §6.22 (FR-131, FR-132) y §7.4 (NFR-009 a NFR-012) | Ninguna. |
| 18 Data requirements | ítem 18 | covered | §8.1-8.8 (modelo versionado, semilla, derechos de autor, catálogo/estado, campos estándar, eventos, propiedad, datos personales); addendum §C | Ninguna. |
| 19 Success metrics | ítem 19 | covered | §10: SM-1 a SM-8 con FR validados y SM-C1 a SM-C8 contramétricas | Ninguna. |
| 20 Out-of-scope items | ítem 20 | covered | §11.2, §11.3 (diferidos por fase), §12 (no-objetivos permanentes) | Ninguna. |
| 21 Assumptions | ítem 21 | covered | §13 (12 supuestos de producto) y §19 (índice A1-A37) | Ninguna. |
| 22 Risks | ítem 22 | covered | §14: R1-R13 con impacto y mitigación referenciada a FR/NFR | Ninguna. |
| 23 Dependencies | ítem 23 | covered | §15.1 (PR-EXT-001 a 015 con fase y FR) y §15.2 | Ninguna. |
| 24 Acceptance criteria | ítem 24 | covered | §16.1 (E2E 15 pasos), §16.2 (10 criterios técnicos), §16.3 (definición de hecho) | Ver §5 de este documento (pasos de P2 §46 no cubiertos). |

---

## 4. P1 §17, §37, §38 — fases, orden inicial y primeros cambios frente a ROADMAP.md

| Ítem | Dónde en el insumo | Estado | Dónde en el PRD / ROADMAP | Corrección sugerida |
|---|---|---|---|---|
| 16 fases macro (Phase 0 Development Environment … Phase 15 Security/Performance/Hardening) | §17 | covered | ROADMAP §2: las 16 fases P1 aparecen como referencia, cada una mapeada a la fase P2 §64 que absorbe, al tren (MVP/F2/F3) y a la sección del PRD; columna "Fase P1" en todos los cambios de §3, §4, §5 | Ninguna. |
| "Do not implement all phases in one change" | §17 | covered | ROADMAP regla 1 y §3 (24 cambios), §7 | Ninguna. |
| Orden §37 ítems 1-16 (Foundation → Tenant → Auth → RBAC → Audit Trail → Standards → Requirements → Context → Scope → Inventory → Risk → Impact → SoA → Controls → Evidence → Workflow) | §37 | covered | ROADMAP §3 cambios 1-14 siguen ese orden (Auth+RBAC fusionados en `identity-and-rbac`; Standards+Requirements en `standards-and-requirements-engine`; SoA+Controls en `soa-control-activation`), con la inserción de `domain-events-outbox-jobs` (#5, infraestructura ADR-003) | Ninguna. |
| Desvío: Policies (§37 #18) adelantado al puesto 8 (tras contexto/alcance) | §37 #18; "Architecture may justify changing this order" | covered | PRD §17 D-9 y ROADMAP §3 (intro) justifican el desvío por agrupación P2 §64 fase 3 (AIMS Core) | Ninguna. |
| Desvío: Lifecycle (§37 #17) e Incidents (§37 #19) fuera del MVP | §37 #17, #19 | covered | Justificado por A1 en PRD §6.7 y §6.12 (descripciones), §11.2; ROADMAP §2 fila 8, F2-11, F2-13 | Ninguna. |
| Desvío: Certification Readiness (§37 #25) implementado **antes** de Integrations (§37 #24) | §37 #24-#25 | weakened | ROADMAP §3: cambio 20 `certification-readiness-workspace` precede a 21 `connector-sdk-mock`. Sin justificación explícita en ROADMAP ni en PRD §17 (D-8/D-9 no lo cubren) | Añadir una frase en ROADMAP §3 intro: "el SDK de conectores (21) se sitúa tras la preparación (20) porque no participa del flujo E2E de 15 pasos y solo depende del cambio 13". O invertir 20 y 21. |
| Cinco primeros cambios §38: `foundation-project-bootstrap`, `organization-tenancy`, `identity-and-rbac`, `immutable-audit-trail`, `standards-and-requirements-engine` | §38 | weakened | Los cinco nombres existen (ROADMAP cambios 1, 2, 3, 4 y **6**). Pero ROADMAP regla 2 y PRD §17 D-8 afirman que son "los cinco primeros cambios", mientras la tabla §3 inserta `domain-events-outbox-jobs` como #5. Inconsistencia interna (P1 §38 dice "at minimum", por lo que la inserción es admisible, pero el texto del PRD/ROADMAP promete otra cosa) | Opción a: reformular ROADMAP regla 2 y PRD D-8 ("los cinco cambios nominados por P1 §38 se conservan con su nombre; se inserta `domain-events-outbox-jobs` (ADR-003/ADR-004) tras el registro de auditoría"). Opción b: fusionar el outbox en `immutable-audit-trail` o en `foundation-project-bootstrap` para que los cinco sean literalmente los primeros. |
| "Do not start with Azure integration. The platform needs its domain foundation first." | §38 | covered | ROADMAP regla 2 ("Ninguna integración antes de la fundación de dominio"), cambio 21 "Depende de: 13 (nunca antes del 6)", conectores reales en F2-5…F2-7; PRD §11.2, addendum §E | Ninguna. |

---

## 5. P2 §70 (principio rector) y §46 (E2E de 18 pasos) frente al PRD §3 y §16

### 5.1 P2 §70 — principio rector

| Ítem | Dónde en el insumo | Estado | Dónde en el PRD / ROADMAP | Corrección sugerida |
|---|---|---|---|---|
| "Software que opera el SGIA y produce continuamente la evidencia de que funciona", no "software que dice si marqué la casilla" | §70 | covered | PRD §3 (cita literal como visión, "principio rector P2 §70"), §1, tesis y 7 principios; §4.2 (no usuarios: checklist rápida) | Ninguna. |
| La SoA gobierna los flujos | §70 (SoA → ACTIVE CONTROLS → PROCESSES) + CLAUDE.md | covered | PR-DR-001 a 004, FR-061, FR-064, FR-103; UJ-1 | Ninguna. |
| Evidencia con procedencia | §70 (EVIDENCE) | covered | FR-071 (origen, método, recogido por, fecha, hash, versión, revisor), FR-073, FR-074, FR-076 (procedencia automatizada; modelo en MVP, F2 operativo), §5 "Procedencia de la evidencia", SM-3 | Ninguna. |
| Nunca "certificado" | §70 / CLAUDE.md | covered | PR-DR-009, FR-100 (prueba de cadenas), FR-113, FR-104, §12, SM-C4; §16.1 paso 15 | Ninguna. |
| Cadena de datos ORGANIZATION → CONTEXT → SCOPE → AI PORTFOLIO → RISKS + IMPACTS → OBJECTIVES → REQUIREMENTS → SoA → ACTIVE CONTROLS → PROCESSES → EVIDENCE → MONITORING → AUDIT → FINDINGS → CORRECTIVE ACTION → MANAGEMENT REVIEW → CONTINUAL IMPROVEMENT, "reflejada en BD, API, UI, workflows e informes" | §70 | weakened | El ciclo de §3 cubre todas las etapas y los FR existen para cada nodo (FR-001, FR-025, FR-027, FR-034, FR-041…, FR-031, FR-016, FR-060, FR-064, FR-079, FR-071, FR-100, FR-088, FR-094, FR-096, FR-098, FR-097). Pero la cadena de trazabilidad navegable de FR-125 es `Requirement → Control → Risk → AI System → Asset/Data/Model → Evidence → Audit → Finding → Corrective Action → Management Review`: omite ORGANIZATION/CONTEXT/SCOPE, OBJECTIVES, PROCESSES, MONITORING y CONTINUAL IMPROVEMENT; FR-019 mapea requisito ↔ control ↔ riesgo ↔ política/procedimiento ↔ tarea ↔ KPI ↔ pregunta pero no `AIObjective`. No hay criterio de aceptación que exija la cadena completa en BD/API/UI/informes | Ampliar FR-125 con los nodos de contexto/alcance (`Scope` → `AISystem`), `AIObjective` (→ requisitos/controles/KPI), `Procedure` (control → procedimiento → evidencia) e `ImprovementOpportunity` (F2); añadir a FR-019 el mapeo `AIObjective ↔ Requirement/Control`; añadir en §16.2 un criterio "la cadena de P2 §70 es navegable en UI y API para la organización real (prueba E2E de recorrido)". |

### 5.2 P2 §46 — pasos E2E (18) frente a PRD §16.1 (15) y categorías de prueba

| Ítem | Dónde en el insumo | Estado | Dónde en el PRD / ROADMAP | Corrección sugerida |
|---|---|---|---|---|
| E1 Create organization | §46 E2E #1 | covered | §16.1 paso 1 (tenant con `data_region`, RLS probado); FR-001 a FR-005; ROADMAP cambio 2 | Ninguna. |
| E2 Create users | §46 #2 | weakened | Fusionado en §16.1 paso 1 ("administrador invitado y activado"); la intro de §16.1 dice "con un usuario por rol" pero la condición verificable del paso 1 solo exige el administrador | Añadir al paso 1 la condición "≥ 1 usuario invitado y activado por cada rol usado en los pasos 2-15 (`USER_INVITED`/`USER_ACTIVATED`)". |
| E3 Configure roles | §46 #3 | weakened | Implícito en "un usuario por rol" (§16.1 intro) y en FR-004; no hay condición verificable de asignación de roles/permisos en la tabla | Añadir al paso 1: "roles asignados según la matriz por defecto (D-6); una prueba de autorización a nivel de objeto en verde". |
| E4 Define context | §46 #4 | covered | §16.1 paso 2; FR-025 | Ninguna. |
| E5 Define scope | §46 #5 | covered | §16.1 paso 4 (más paso 3 partes interesadas, añadido); FR-027 | Ninguna. |
| E6 Create AI system | §46 #6 | covered | §16.1 paso 5; FR-034 a FR-036 | Ninguna. |
| E7 Perform risk assessment | §46 #7 | covered | §16.1 paso 6 (metodología, ≥ 2 riesgos, plan aprobado, aceptación con expiración); FR-041 a FR-046 | Ninguna. |
| E8 Perform impact assessment | §46 #8 | covered | §16.1 paso 7; FR-050 a FR-053 | Ninguna. |
| E9 Configure SoA | §46 #9 | covered | §16.1 paso 8; FR-059 a FR-063, FR-067 | Ninguna. |
| E10 Activate controls | §46 #10 | covered | §16.1 paso 9; FR-064, FR-065 | Ninguna. |
| E11 Create evidence | §46 #11 | covered | §16.1 paso 10 (hash, revisor distinto, una rechazada); FR-071 a FR-075 | Ninguna. |
| E12 Generate policy | §46 #12 | missing | No hay paso de política en §16.1. El PRD decide (D-1) que la política de IA entra en el MVP como información documentada sin LLM (FR-029) precisamente porque "sin ellos el paso 15 del E2E no puede marcar 'política aprobada'", y la lista de preparación de FR-113 exige "política aprobada"; sin embargo ni la política ni los objetivos (FR-031) aparecen como paso ni como condición del E2E. §16.1 solo justifica la omisión de los pasos de conectores de P2 §69, no la de "Generate policy" | Insertar un paso (tras el 4): "Aprobar la política de IA y ≥ 1 objetivo de la IA — `Policy` en `Approved`/`Published` con versión y `POLICY_APPROVED`; `AIObjective` con responsable, plazo y método; ambos visibles en la lista de preparación (FR-029, FR-031, FR-079)". Añadir en la intro de §16.1: "'Generate policy' de P2 §46 se sustituye en MVP por gestión documental de la política (D-1); la generación por LLM (FR-081) vuelve en Fase 2". Actualizar "15 pasos" → "16 pasos" en §1, §10 SM-1, ROADMAP cambio 8 y 24. |
| E13 Run audit | §46 #13 | covered | §16.1 paso 11; FR-087 a FR-091 | Ninguna. |
| E14 Create finding | §46 #14 | covered | §16.1 paso 12; FR-090, FR-094 | Ninguna. |
| E15 Create CAPA | §46 #15 | covered | §16.1 paso 13; FR-094, FR-095 | Ninguna. |
| E16 Close CAPA | §46 #16 | covered | §16.1 paso 13 (verificación de eficacia y cierre; cierre sin verificación rechazado); FR-096 | Ninguna. |
| E17 Generate management review | §46 #17 | covered | §16.1 paso 14; FR-098, FR-099, FR-080 | Ninguna. |
| E18 Generate reports | §46 #18 | weakened | §16.1 paso 15 solo exige el *Audit Readiness Report* y la lista de comprobación; FR-106 promete cuatro informes esenciales en MVP y UJ-1 exporta el *Statement of Applicability Report*, pero el E2E no los verifica | Ampliar paso 15: "los cuatro informes esenciales (FR-106) generados en ES y EN con versión de SoA, versión de norma y fecha de corte; ninguno contiene 'certificado'". |
| Categorías de prueba §46: unitarias (scoring, cálculo de riesgo, aplicabilidad, permisos, transiciones, cálculos de informes), integración (BD, API, conectores, ingestión de evidencia, workflow), seguridad (cross-tenant, escalada, evidencia no autorizada, exposición de secretos, BOLA) | §46 Unit / Integration / Security tests | covered | NFR-023 (reproduce las 6 unitarias y las 5 de integración), NFR-005 (las 5 de seguridad), §16.2 criterios 1-2, FR-002, FR-004; ROADMAP cambios 3, 4, 24 | Ninguna. |

---

## 6. Brechas priorizadas (para el paso 3 del Finalize)

1. **E12 "Generate policy" ausente del E2E (§16.1)** — contradice la propia justificación de D-1 y deja incompleta la lista de preparación del paso 15. Insertar paso de política + objetivo y justificar la sustitución del paso de P2 §46.
2. **Inconsistencia "cinco primeros cambios" (ROADMAP regla 2, PRD D-8) frente a la inserción de `domain-events-outbox-jobs` como #5** — reformular el texto o fusionar el cambio.
3. **Puntos de aprobación humana de `CLAUDE.md`/A12 ausentes en ROADMAP §6**: "publicar (PR, push, despliegue)" y "cambios de arquitectura de seguridad".
4. **Cadena de datos de P2 §70 solo parcialmente navegable (FR-125)**: faltan CONTEXT/SCOPE, OBJECTIVES, PROCESSES, MONITORING, CONTINUAL IMPROVEMENT y un criterio de aceptación que exija la cadena completa en BD/API/UI/informes.
5. **Paso 15 del E2E verifica un solo informe** cuando FR-106 promete cuatro; y los pasos E2/E3 (usuarios, roles) no tienen condición verificable propia.
6. Menor: clasificar las decisiones §17 con los tipos de `CLAUDE.md` (`organizational_decision` / `technical_design_decision`); justificar en ROADMAP el orden 20 (preparación) antes de 21 (SDK de conectores).
