---
titulo: "Modelo de trazabilidad: del documento fuente a la evidencia generada"
fecha: "2026-09-17"
estado: "borrador para revisión"
autor: "Agente Analista de Requisitos (Claude) para Andrés Mauricio Pardo"
nivel_fuente_de_verdad: 2
fuentes:
  - docs/product/PROMPT_REVIEW_AND_ADJUSTMENTS.md (A10 nodo "documento fuente", A6 semilla sin texto literal, §5 jerarquía de fuentes de verdad)
  - docs/product/MASTER_ORCHESTRATION_BMAD_OPENSPEC.md (§16 modelo de trazabilidad, §6 jerarquía, §25 archivo OpenSpec)
  - docs/product/MASTER_ISO42001_PLATFORM_REQUIREMENTS.md (§32 grafo de trazabilidad, §63 auditabilidad, §73 navegación bidireccional)
  - CLAUDE.md (paso 5 "Cómo trabajar un cambio": actualizar TRACEABILITY_MATRIX.md al archivar)
  - docs/product/REQUIREMENTS_ANALYSIS.md, DOMAIN_MAP.md, GLOSSARY.md
  - resumenes/01 (cláusula 6.1.3), resumenes/02 (control A.6.2.2), resumenes/13 (glosario)
---

# Modelo de trazabilidad

## 1. Propósito

El producto es una plataforma de gobernanza; su propio desarrollo debe ser auditable con el mismo rigor que exige a sus clientes (P1 §16, P2 §63). Este modelo define la cadena de trazabilidad completa, el esquema del archivo `docs/compliance/TRACEABILITY_MATRIX.md` y el procedimiento para mantenerlo actualizado en cada archivo de cambio OpenSpec.

El ajuste A10 antepone a la cadena de P1 §16 el nodo **Documento fuente**, de modo que cada requisito sembrado pueda remontarse al material de estudio (`resumenes/`) sin distribuir el texto de la norma.

## 2. La cadena de trazabilidad

```text
[0] Documento fuente         resumenes/NN_…, sección (paráfrasis de la norma; nivel 0/1)
        ↓
[1] Requisito ISO            cláusula, subcláusula o control (identificador oficial, p. ej. 6.1.3 f) o A.6.2.2)
        ↓
[2] Requisito de producto    PR-CAP-nnn / PR-XC-nnn / PR-NFR-nnn / PR-DR-nnn / PR-EXT-nnn
        ↓
[3] Capacidad / contexto     bounded context de DOMAIN_MAP.md y capacidad OpenSpec (specs/<capacidad>/)
        ↓
[4] Épica BMAD               EPIC-nnn en _bmad-output/planning-artifacts/
        ↓
[5] Historia BMAD            STORY-nnn.n
        ↓
[6] Cambio OpenSpec          openspec/changes/<kebab-name>/ (archivado en openspec/changes/archive/)
        ↓
[7] Implementación           módulo, archivo(s) y símbolo(s) en apps/ o packages/
        ↓
[8] Prueba automatizada      archivo de prueba y nombre del caso (unit / integration / e2e / security / domain)
        ↓
[9] Evidencia generada       artefacto observable que demuestra el comportamiento (informe de pruebas en CI, captura E2E, migración, registro de semilla)
```

Las relaciones son **muchos a muchos** en casi todos los saltos: un control puede generar varios PR; un PR puede cubrirse en varios cambios; una prueba puede demostrar varios PR. La matriz registra una fila por **par (Requisito ISO, Requisito de producto)** y agrupa el resto de nodos en columnas con listas separadas por `;`.

### 2.1 Reglas de la cadena

1. **Ningún nodo se salta.** Un PR sin requisito ISO se marca `—` en [1] y `producto` en [0] (procede solo de P2 o de A). Un requisito ISO sin PR es una **brecha** y aparece en la sección de brechas de la matriz.
2. **[0] es obligatorio para todo [1].** Si un requisito ISO no puede citarse desde `resumenes/`, no entra en la semilla ni en la matriz; se registra `SOURCE_DETAIL_REQUIRED` en la columna [0].
3. **[7] y [8] solo se rellenan al archivar un cambio OpenSpec** con pruebas en verde. Hasta entonces la fila existe con estado `planned` o `in_progress`.
4. **[9] nunca es una afirmación.** Es una referencia a un artefacto verificable (URL de ejecución de CI, ruta de captura, hash de migración). "Implementado" sin [9] es `unverified`.
5. **Bidireccionalidad**: desde cualquier nodo debe poderse llegar al resto. La matriz Markdown es la vista canónica; en Fase 2 se genera además un JSON (`docs/compliance/traceability.json`) para que la propia plataforma lo muestre en su tenant de plataforma (A9).

## 3. Convenciones de identificador

| Nodo | Formato | Ejemplo | Fuente de verdad |
|---|---|---|---|
| Documento fuente | `resumenes/NN §sección` (número de archivo de dos dígitos y sección tal como aparece en el encabezado) | `resumenes/02 §4.7 A.6.2.2`; `resumenes/01 §4 6.1.3` | Encabezados de `resumenes/*.md` |
| Requisito ISO | Norma abreviada + identificador oficial; letras de lista tal como en la norma | `42001:6.1.3 f)`; `42001:A.6.2.2`; `19011:6.4.8`; `23894:5.2` | `resumenes/` |
| Tipo de registro | `normative_requirement`, `implementation_guidance`, `recommended_practice`, `evidence_example` | `normative_requirement` | P2 §1, A6 |
| Requisito de producto | `PR-<CAT>-nnn` | `PR-CAP-053` | `REQUIREMENTS_ANALYSIS.md` |
| Contexto | Nombre del bounded context tal como en `DOMAIN_MAP.md` | `Controls & SoA` | `DOMAIN_MAP.md` |
| Capacidad OpenSpec | kebab-case, nombre de la carpeta en `openspec/specs/` | `soa-control-activation` | `openspec/specs/` |
| Épica BMAD | `EPIC-nnn` (tres dígitos, secuencial) | `EPIC-006` | `_bmad-output/planning-artifacts/` |
| Historia BMAD | `STORY-nnn.n` (épica.historia) | `STORY-006.3` | idem |
| Cambio OpenSpec | kebab-case, nombre de la carpeta del cambio | `soa-control-activation` | `openspec/changes/` |
| Implementación | ruta relativa desde la raíz + `#símbolo` opcional | `packages/domain/soa/src/activate-control.ts#activateControl` | repositorio |
| Prueba | ruta + `::nombre del caso` | `packages/domain/soa/test/activation.spec.ts::"inactive control creates no tasks"` | repositorio |
| Evidencia | tipo + referencia | `ci:run#4821`; `e2e:screenshots/soa/activate.png`; `seed:standards-seed@1.0.0#A.6.2.2` | CI, repositorio |
| Estado de la fila | `planned`, `in_progress`, `implemented`, `verified`, `unverified`, `gap`, `deferred(F2)`, `deferred(F3)`, `retired` | `verified` | este documento |

## 4. Esquema de `docs/compliance/TRACEABILITY_MATRIX.md`

### 4.1 Estructura del archivo

```markdown
---
titulo: "Matriz de trazabilidad"
ultima_actualizacion: "AAAA-MM-DD"
ultimo_cambio_archivado: "<kebab-name>"
version_semilla: "standards-seed@x.y.z"
---

# Matriz de trazabilidad

## 1. Resumen
(tabla: requisitos ISO totales / con PR / implementados / verificados / brechas; por cláusula y por dominio del Anexo A)

## 2. Matriz principal
(una fila por par Requisito ISO × PR; columnas de §4.2)

## 3. Requisitos de producto sin requisito ISO
(PR-XC, PR-NFR, PR-EXT y PR-CAP puramente de producto; mismas columnas, [0] = "producto", [1] = "—")

## 4. Brechas
(requisitos ISO de resumenes/ sin PR o sin cambio; con justificación o fase)

## 5. Historial de actualizaciones
(fecha, cambio archivado, filas añadidas/modificadas, autor)
```

### 4.2 Columnas de la matriz principal

| # | Columna | Contenido | Obligatoria desde |
|---|---|---|---|
| 1 | `source_doc` | `resumenes/NN §sección` | siempre |
| 2 | `iso_requirement` | identificador oficial (`42001:6.1.3 f)`) | siempre |
| 3 | `record_type` | tipo de registro normativo | siempre |
| 4 | `iso_title` | título parafraseado corto (nunca texto literal) | siempre |
| 5 | `product_requirement` | `PR-*` | siempre |
| 6 | `context` | bounded context | siempre |
| 7 | `capability` | capacidad OpenSpec | planificación |
| 8 | `epic` | `EPIC-nnn` | planificación BMAD |
| 9 | `story` | `STORY-nnn.n` (lista) | planificación BMAD |
| 10 | `change` | cambio(s) OpenSpec (lista) | propuesta OpenSpec |
| 11 | `implementation` | rutas y símbolos (lista) | archivo del cambio |
| 12 | `tests` | rutas y casos (lista) | archivo del cambio |
| 13 | `evidence` | referencias verificables (lista) | archivo del cambio |
| 14 | `phase` | `MVP`, `F2`, `F3` | siempre |
| 15 | `status` | estado de la fila | siempre |
| 16 | `notes` | supuestos, `SOURCE_DETAIL_REQUIRED`, conflictos | opcional |

La matriz se mantiene en Markdown por legibilidad en revisión; para evitar filas ilegibles, las listas usan `;` y las rutas largas pueden abreviarse con el prefijo del paquete si el pie de tabla lo declara.

### 4.3 Reglas de integridad (comprobadas por script en CI, Fase MVP tardía) [ASSUMPTION]

1. Todo `PR-*` de `REQUIREMENTS_ANALYSIS.md` aparece al menos una vez en la matriz (§2 o §3).
2. Todo control A.n.n de la semilla (38) y toda subcláusula 4.1-10.2 aparecen al menos una vez en §2 o en §4 (brechas).
3. Una fila `verified` tiene las columnas 11, 12 y 13 no vacías.
4. Una fila `implemented` sin columna 13 pasa automáticamente a `unverified`.
5. Ningún `change` referenciado está en `openspec/changes/` activo si la fila es `verified` (debe estar archivado).
6. `version_semilla` del frontmatter coincide con la versión de `packages/standards-seed/`.

## 5. Procedimiento de actualización al archivar un cambio OpenSpec

Este procedimiento se ejecuta en el paso 5 de "Cómo trabajar un cambio" de `CLAUDE.md` (`/opsx:archive` → actualizar matriz). Es parte de la definición de hecho del cambio; un cambio no se considera archivado si la matriz no se actualizó.

1. **Antes de archivar**, leer `openspec/changes/<name>/proposal.md` y `tasks.md` para extraer: PR-* cubiertos (deben estar listados en la propuesta), capacidades tocadas, archivos implementados y pruebas añadidas.
2. **Localizar filas** de la matriz cuya columna `product_requirement` esté en la lista de PR del cambio y cuya `change` incluya `<name>` o esté vacía en estado `planned`.
3. **Rellenar** columnas 11 (`implementation`), 12 (`tests`) y 13 (`evidence`) con las referencias reales del cambio. La evidencia mínima es el identificador de la ejecución de CI en verde que archiva el cambio y, para cambios con UI, la captura o el vídeo de la prueba E2E.
4. **Cambiar el estado** a `verified` si hay pruebas y evidencia; a `implemented` si falta evidencia (y anotar por qué); mantener `in_progress` si el PR se cubre solo parcialmente y anotar qué parte queda (usar el asterisco `MVP*` del análisis como guía).
5. **Añadir filas nuevas** si el cambio descubrió requisitos ISO o PR no contemplados; en ese caso, actualizar antes `REQUIREMENTS_ANALYSIS.md` (nivel 2) porque la matriz no es fuente de requisitos.
6. **Registrar brechas**: si el cambio decidió no cubrir algo que la propuesta anunciaba, moverlo a §4 con justificación y fase.
7. **Actualizar el frontmatter** (`ultima_actualizacion`, `ultimo_cambio_archivado`, `version_semilla` si cambió) y añadir una línea al §5 Historial.
8. **Ejecutar el script de integridad** (`pnpm traceability:check`, cuando exista) y corregir antes de archivar.
9. **Commit** con mensaje `docs(compliance): update traceability matrix for <name>` en el mismo PR que archiva el cambio.

Cuando la semilla de normas cambia de versión (nueva enmienda o corrección de paráfrasis), se ejecuta una revisión completa de la columna `source_doc` para las filas afectadas, y las filas cuyo requisito ISO haya cambiado de identificador se marcan `retired` y se crean nuevas.

## 6. Filas de ejemplo completas

Las filas siguientes ilustran el estado que tendrán al archivarse el cambio `soa-control-activation` del MVP. Las rutas de implementación, pruebas y evidencia son **ilustrativas** [ASSUMPTION]: el repositorio aún no contiene código; los nombres de épica e historia se asignarán en la planificación BMAD.

### 6.1 Cláusula 6.1.3 f) — declaración de aplicabilidad

| Columna | Valor |
|---|---|
| source_doc | `resumenes/01 §4 6.1.3 Tratamiento del riesgo de la IA`; `resumenes/01 §4 Tabla: Información documentada obligatoria (fila 6.1.3 f)`; `resumenes/13 §4.2 3.26` |
| iso_requirement | `42001:6.1.3 f)` |
| record_type | `normative_requirement` |
| iso_title | Elaborar una declaración de aplicabilidad con los controles necesarios y la justificación de inclusiones y exclusiones |
| product_requirement | `PR-CAP-051` (SoA de primera clase); filas hermanas: `PR-CAP-052`, `PR-CAP-054`, `PR-CAP-060`, `PR-DR-001`, `PR-DR-003`, `PR-DR-004` |
| context | `Controls & SoA` |
| capability | `soa-control-activation` |
| epic | `EPIC-006 Declaración de aplicabilidad y activación de controles` |
| story | `STORY-006.1 Decidir aplicabilidad de los 38 controles`; `STORY-006.2 Justificación obligatoria de exclusiones`; `STORY-006.4 Aprobar y versionar la SoA` |
| change | `soa-control-activation` |
| implementation | `packages/domain/soa/src/statement-of-applicability.ts#StatementOfApplicability`; `packages/domain/soa/src/soa-item.ts#decideApplicability`; `packages/domain/soa/src/approve-soa.ts#approveSoA`; `apps/web/app/[locale]/iso42001/soa/page.tsx`; `packages/db/prisma/migrations/2026xxxx_soa/migration.sql` |
| tests | `packages/domain/soa/test/soa-item.spec.ts::"Not Applicable requires justification"`; `packages/domain/soa/test/approve-soa.spec.ts::"approval requires owner and mapped risks for every Applicable item"`; `packages/domain/soa/test/soa-report.spec.ts::"completeness report lists all 38 controls including Not Applicable"`; `apps/web/e2e/soa.spec.ts::"complete and approve SoA v1"`; `packages/standards-seed/test/annex-a.spec.ts::"38 controls with domain distribution"` |
| evidence | `ci:github-actions/run#<id>` (todas las suites en verde); `e2e:artifacts/soa/approve-v1.png`; `auditlog:SOA_APPROVED` (entrada de la cadena de hashes del tenant de prueba); `report:soa-report-sample.pdf` |
| phase | `MVP` |
| status | `planned` (pasará a `verified` al archivar) |
| notes | La justificación de exclusión aplica a controles del Anexo A, no a requisitos de las cláusulas 4-10 (conflicto C5 del análisis). La SoA no documenta la orientación del Anexo B (B.1, `resumenes/02 §3.1`). |

### 6.2 Control A.6.2.2 — requisitos y especificación del sistema de IA

| Columna | Valor |
|---|---|
| source_doc | `resumenes/02 §4.7 A.6.2.2 Requisitos y especificación del sistema de IA`; `resumenes/02 §4.12 Mapa rápido (fila A.6.2.2)`; `resumenes/13 §4.3 (título bilingüe)` |
| iso_requirement | `42001:A.6.2.2` (control, Tabla A.1); orientación asociada `42001:B.6.2.2` como `implementation_guidance` |
| record_type | `normative_requirement` (el control) + `implementation_guidance` (B.6.2.2) |
| iso_title | Especificar y documentar los requisitos de las nuevas versiones o mejoras del sistema de IA, con justificación y caso de negocio |
| product_requirement | `PR-CAP-050` (catálogo Anexo A); `PR-CAP-020` (registro del sistema de IA con propósito de negocio, uso previsto y requisitos); `PR-CAP-053` (activación del control); en Fase 2 `PR-CAP-046` (puerta de ciclo de vida que exige la especificación) |
| context | `Standards & Compliance` (catálogo); `Controls & SoA` (aplicabilidad y activación); `AI Portfolio` (dónde se documenta la especificación) |
| capability | `standards-engine-seed`; `soa-control-activation`; `ai-inventory-core` |
| epic | `EPIC-002 Motor de normas y semilla`; `EPIC-006 Declaración de aplicabilidad`; `EPIC-004 Inventario de IA` |
| story | `STORY-002.3 Sembrar los 38 controles con guía B como implementation_guidance`; `STORY-006.3 Activar control y generar tareas y solicitudes de evidencia`; `STORY-004.2 Registrar propósito, uso previsto y requisitos del sistema` |
| change | `standards-engine-seed`; `soa-control-activation`; `ai-inventory-core` |
| implementation | `packages/standards-seed/data/iso42001/annex-a/A.6.2.2.json` (con `source_doc` y `SOURCE_DETAIL_REQUIRED` donde aplique); `packages/domain/soa/src/activate-control.ts#activateControl`; `packages/domain/ai-portfolio/src/ai-system.ts#AISystem` |
| tests | `packages/standards-seed/test/annex-a.spec.ts::"A.6.2.2 exists in domain A.6 subdomain A.6.2 with record_type normative_requirement"`; `packages/standards-seed/test/annex-a.spec.ts::"B.6.2.2 guidance is implementation_guidance and not SoA-justifiable"`; `packages/domain/soa/test/activation.spec.ts::"activating A.6.2.2 creates evidence request and implementation task"`; `packages/domain/soa/test/activation.spec.ts::"Not Applicable A.6.2.2 creates no tasks and stays visible"` |
| evidence | `seed:standards-seed@1.0.0#A.6.2.2`; `ci:github-actions/run#<id>`; `auditlog:CONTROL_ACTIVATED{control=A.6.2.2}`; `e2e:artifacts/soa/activate-A.6.2.2.png` |
| phase | `MVP` (catálogo y activación); `F2` (puerta de ciclo de vida) |
| status | `planned` |
| notes | El control se enuncia con *shall* en la Tabla A.1 y la orientación B.6.2.2 con *should* (`resumenes/13 §4.5`); la UI debe distinguirlos (PR-DR-018). Las referencias externas de la orientación (ISO/IEC 5338, ISO 9241-210) se registran como `references`, no como requisitos. |

## 7. Relación con la trazabilidad del producto (P2 §32)

La cadena de este documento traza el **desarrollo** de la plataforma. La plataforma, a su vez, ofrece a sus clientes la cadena **operativa** `Requirement → Control → Risk → AI System → Evidence → Audit → Finding → Corrective Action → Management Review` (P2 §32, §73; PR-CAP-165). Ambas comparten los nodos [0] y [1]: la semilla que alimenta el catálogo del producto cita `resumenes/NN §sección` en cada registro (`SeedRecordSource`, `DOMAIN_MAP.md` §3.2), por lo que un auditor que vea un control en la UI puede saber de qué documento fuente y de qué cambio de software procede su definición. En Fase 2 el tenant de plataforma expone esta matriz como evidencia de sus propios controles A.6.2.3 (documentación del diseño) y A.6.2.7 (documentación técnica) (A9).
