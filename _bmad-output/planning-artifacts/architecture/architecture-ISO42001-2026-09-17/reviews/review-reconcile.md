---
titulo: "Revisión independiente: reconciliación de insumos contra la espina de arquitectura"
fecha: 2026-09-17
revisor: "lente de reconciliación (headless, independiente)"
objeto: _bmad-output/planning-artifacts/architecture/architecture-ISO42001-2026-09-17/ARCHITECTURE-SPINE.md
derivados_revisados:
  - docs/architecture/{SYSTEM,DOMAIN,DATA,API,SECURITY,EVENT,INTEGRATION,REPORTING,AI}_ARCHITECTURE.md
  - docs/decisions/ADR-001..ADR-016
  - docs/compliance/TRACEABILITY_MATRIX.md
insumos_contrastados:
  - CLAUDE.md (completo)
  - docs/product/PROMPT_REVIEW_AND_ADJUSTMENTS.md (A1-A13)
  - prd.md §7, §8, §9, §16.2, §17, §18
  - docs/product/DOMAIN_MAP.md §4, §7
  - docs/product/ROADMAP.md §3 (24 cambios)
  - docs/product/TRACEABILITY_MODEL.md §3-§5
  - docs/product/MASTER_ISO42001_PLATFORM_REQUIREMENTS.md §37, §43
metodo: "lectura completa de la espina; por insumo, extracción de exigencias y grep sobre espina + derivados; estado = cubierto | parcial | ausente | contradicción"
---

# Revisión de reconciliación de insumos

## 0. Veredicto

La espina aterriza fielmente las decisiones estructurales de nivel 1-2 (RLS, outbox, cadena de hashes, catálogo/estado, SoA por evento, `Approval`, `ScoreSnapshot`, SDK de conectores, IA solo `*.suggest`) y los derivados rellenan casi todo el detalle; **pero el mapa capacidad → arquitectura no es construible en el orden de la hoja de ruta** (cinco inversiones de dependencia, dos de ellas duras: el outbox/pg-boss que los cambios 2-4 necesitan llega en el 5, y la tabla `findings` de CAPA que los cambios 11 y 15 necesitan llega en el 16), y quedan **sin aterrizar en la espina** varios requisitos "silenciosos" de nivel 1: accesibilidad WCAG 2.2 AA, prohibición de botones falsos / feature flags de módulo, puntos de aprobación humana A12 (publicar, copyleft), D-2 (restricción de `Not Applicable` en requisitos), la actualización obligatoria de la matriz de trazabilidad al archivar y el mapeo cruzado 27001. Hay además tres contradicciones a corregir: antimalware en MVP (PRD lo fija en F2), `/findings` asignado a Audit cuando `Finding` es de CAPA, y dos formas de ruta para `ReportRun`.

Leyenda de estado: **cubierto** = aterriza en la espina (AD-n, convención, seed, Deferred) o en un derivado con la fidelidad exigida; **parcial** = aterriza solo en un derivado, o de forma incompleta, cuando la espina debería fijarlo porque dos cambios OpenSpec podrían decidirlo distinto; **ausente** = no aparece en espina ni derivados; **contradicción** = la espina o un derivado dice lo contrario que el insumo (o dos derivados se contradicen).

---

## 1. `CLAUDE.md` (nivel 1: reglas del dominio, idioma, ingeniería, aprobaciones, prohibiciones)

| # | Exigencia | Dónde aterriza | Estado | Corrección propuesta |
|---|---|---|---|---|
| C1 | `resumenes/` es nivel 0/1, solo lectura, **nunca se importa en runtime** | Espina, árbol semilla (`resumenes/ # nunca importado en runtime`); AD-7 carga solo desde `standards-seed`; DATA §11 | parcial | Añadir a AD-3 una regla de `dependency-cruiser` que prohíba todo import desde `resumenes/` y desde `Material de estudio*` (hoy la prohibición es un comentario, no una prueba de arquitectura). |
| C2 | Jerarquía de fuentes de verdad: el inferior nunca sobreescribe al superior en silencio | Espina AD-15 `[ASSUMPTION prefijo v1]`; API §15 A-1 y SYSTEM §11 lo elevan a aprobación humana | cubierto | Ninguna. Buen ejemplo de conflicto identificado y no resuelto en silencio. |
| C3 | Nunca inventar requisitos/identificadores/texto/capacidades de API; `SOURCE_DETAIL_REQUIRED` | AD-7, AD-17, AD-18; convención de errores; DATA §2.1 `source_detail_required`; DOMAIN §4 PR-DR-019 | cubierto | — |
| C4 | Distinguir siempre los seis tipos (`normative_requirement`, `implementation_guidance`, `recommended_practice`, `organizational_decision`, `product_requirement`, `technical_design_decision`) | Los cuatro primeros como `record_type` de la semilla (DATA §2.2); API §15 tipa sus decisiones pendientes; la espina no tipa las suyas | parcial | Añadir a cada `[ASSUMPTION]` y a la tabla Deferred de la espina la columna/etiqueta de tipo (`technical_design_decision`, `organizational_decision`, `product_requirement`) como hacen API §15 y SECURITY §14, para que el aprobador humano sepa qué está aprobando. |
| C5 | Hechos verificados: 38 controles con distribución, 26 términos, 11 objetivos, 7 fuentes; UNE safety=protección, security=seguridad | AD-23 (pruebas 38/26/11/7); DATA §2.1; convención i18n cita `resumenes/13`; DOMAIN §3.11 `safety ≠ security` | cubierto | — |
| C6 | La SoA gobierna los flujos | AD-9; EVENT §11(a); DOMAIN §4 PR-DR-001..004 | cubierto | — |
| C7 | Riesgo tratado ≠ plan; evidencia subida ≠ aceptada; la IA sugiere, una persona aprueba; nunca "certificado" | AD-10, AD-12, AD-17; DOMAIN §3.10, §3.14, §4 | cubierto | — |
| C8 | Toda puntuación expone fórmula, numerador, denominador, exclusiones y fuentes | AD-16 `ScoreSnapshot`; REPORTING §6 | cubierto | — |
| C9 | Idioma: docs en español; código/identificadores/enums/API/commits en inglés; UI bilingüe desde la fundación; glosario `resumenes/13` | Encabezado de la espina, AD-12, convención i18n, capability 1 | cubierto | — |
| C10 | Stack ADR-001 | Sección Stack + ADR-001 | cubierto | El frontmatter de la espina dice `ADR-001..ADR-015` pero existen 16 ADR (ADR-016 SDK de conectores): corregir a `ADR-001..ADR-016`. |
| C11 | `tenant_id` + RLS (ADR-002); monolito modular + outbox (ADR-003) | AD-1, AD-2, AD-4, AD-5 | cubierto | — |
| C12 | Campos comunes de toda entidad importante | AD-11; DATA §3.1 | cubierto | — |
| C13 | Registro append-only con cadena de hashes; toda decisión de cumplimiento genera evento | AD-4, AD-6; DATA §6 | cubierto | — |
| C14 | Cambios de BD solo por migración, reversibles cuando sea práctico, con **índices por `tenant_id`, `status`, `owner_id`, FK y `due_date`** | AD-20 (`prisma migrate deploy`); DATA §9 (índices) y §10 (reversibilidad, `down` documentado) | parcial | La regla de índices y la de reversibilidad son de las que dos cambios decidirían distinto: añadir una fila "Migraciones e índices" a Consistency Conventions de la espina (índices mínimos, `down` en `design.md`, irreversible = aprobación humana). |
| C15 | Pruebas por cambio: unitarias, integración, E2E, seguridad; fixtures de `resumenes/` | AD-23 | cubierto | — |
| C16 | Secretos solo vía abstracción; Zod en toda entrada; **cabeceras seguras; rate limiting** | AD-14, AD-15 (Zod); cabeceras y rate limiting solo en SECURITY §6-§7 y API §11 (`[ASSUMPTION]`) | parcial | AD-15 debe nombrar "cabeceras seguras (CSP, HSTS, `frame-ancestors`) y rate limiting por sesión/API key/IP/tenant" como parte del contrato uniforme, aunque los valores queden en SECURITY. Hoy la espina no menciona ninguno de los dos. |
| C17 | Commits convencionales (incl. `security:`); ramas `feature/<capacidad>`; no reescribir historia; no commitear secretos | Convención "Commits y ramas"; SECURITY §10 (gitleaks) | cubierto | Añadir `security:` a la lista de tipos permitidos si se configura commitlint. |
| C18 | **Aprobación humana antes de**: supuestos mayores, bloquear arquitectura, migraciones irreversibles, seguridad, credenciales de producción, integraciones reales, interpretación normativa, declarar requisito implementado, borrar/alterar evidencia, **publicar (PR, push, despliegue)**, **copyleft** | SYSTEM §11 (bloqueo, D-3, D-6, auth, D.4, Q6, v1, SeaweedFS); DATA §13 (D-2, migraciones irreversibles); SECURITY §10 (copyleft); INTEGRATION §11.4 (cuentas reales). **Publicar/desplegar** no aparece en ninguna parte | parcial | Añadir a AD-20 "todo despliegue a `staging`/`production` y todo PR a rama compartida exige aprobación humana registrada (A12)"; añadir a AD-23 que la lista de licencias permitidas falla el build ante copyleft (SECURITY §10 ya lo detalla). Consolidar en la espina una única tabla "Puertas de aprobación humana" que hoy está repartida en 6 documentos y omite D-2 en SYSTEM §11. |
| C19 | **Prohibido**: UI con botones falsos | Solo la fila 22 del mapa de capacidades ("navegación §37 sin botones falsos") y AI §12 | ausente (como convención) | Añadir convención "Módulos y navegación": toda entrada de navegación visible tiene backend; módulos F2/F3 se ocultan por feature flag (`module_flags` por tenant, FR-131) y nunca se muestran deshabilitados; prueba E2E "toda entrada visible → respuesta de backend + entrada de auditoría" en AD-23 (NFR-011). Hoy no existe convención de feature flags en espina ni derivados. |
| C20 | Prohibido afirmar que un conector funciona sin validarlo contra el proveedor real | AD-18; INTEGRATION §5, §10 | cubierto | — |
| C21 | Prohibido borrar registros de cumplimiento en silencio | AD-11, AD-21; DATA §8 | cubierto | — |
| C22 | Paso 5 "Cómo trabajar un cambio": al archivar, **actualizar `TRACEABILITY_MATRIX.md`** | TRACEABILITY_MATRIX.md §5 remite al procedimiento; la espina no lo incluye en la Definition of Done (AD-23) | parcial | AD-23: "Un `[x]` en `tasks.md` exige todo en verde **y `/opsx:archive` exige la matriz actualizada (`pnpm traceability:check`)**". Ver también §6 de esta revisión. |

---

## 2. `PROMPT_REVIEW_AND_ADJUSTMENTS.md` A1-A13

| # | Exigencia | Dónde aterriza | Estado | Corrección propuesta |
|---|---|---|---|---|
| A1 | Arquitectura de dominio completa (21 contextos, modelo y eventos) desde el inicio; MVP vertical; diferidos (conectores reales, report builder, búsqueda semántica, sectoriales, madurez, agentes, 25 informes) | AD-1 (21 módulos), árbol semilla (21 paquetes), Deferred, REPORTING §8/§10, INTEGRATION §11 | parcial | PRD §8.7 exige que los 21 contextos "existan en el modelo desde el MVP". DATA §3.2 no lista **ninguna tabla para Lifecycle ni para Incidents** (tampoco `candidate_ai_systems` ni `agents`). Fijar en la espina qué significa "existir en el modelo": paquete + fragmento Prisma con las tablas F2 marcadas, o solo paquete; y completar DATA §3.2 con las filas de Lifecycle e Incidents. |
| A2 | Una única hoja de ruta | Mapa capacidad → arquitectura sigue los 24 cambios de ROADMAP §3 | cubierto | Ver §5 (inversiones). |
| A3 | Un solo árbol `docs/` con subcarpetas (product, architecture, decisions, compliance, domain, integrations, security, operations, testing) | Árbol semilla: `docs/ # arquitectura, decisiones, cumplimiento`; API §10 usa `docs/api/openapi.v1.json` | parcial | La espina solo nombra tres subcarpetas; NFR-022 y A3 nombran nueve más `docs/api`. Enumerar en el árbol semilla las subcarpetas de `docs/` que cada cambio debe alimentar (al menos `security`, `operations`, `testing`, `api`), para que `mvp-hardening` no las invente. |
| A4 | Monorepo en dos capas; ningún módulo importa `resumenes/`; semilla desde `packages/standards-seed` revisada por humanos | AD-7, árbol, DATA §11 (`reviewed_by`) | cubierto | Reforzar con la prueba de C1. |
| A5 | Docs en español, código en inglés, UI bilingüe desde fundación (NFR de fase 1), glosario oficial | AD-12; capability 1 incluye i18n | cubierto | — |
| A6 | Semilla solo con `identifier, title, requirement_summary, record_type (4 valores), expected_evidence, references`; nunca texto literal; cada registro cita `resumenes/NN §sección` | AD-7 (cita y prohibición de literal); DATA §11 (formato completo, n-gramas) | cubierto | — |
| A7 | `StandardVersion` soporta desde el inicio 42001:2023 + **Amd 1:2024**, **UNE 2025 `identical_adoption`**, 19011:2026 (`supersedes` 2018), 23894 parcial, **42006 `pending_dependency`** | Solo DATA §2.1 (`amendments`, `adoption_body`, `StandardRelationship.type`); la espina dice "42001, 19011, 23894 parcial" y omite UNE, Amd 1 y 42006 | parcial | AD-7 debe fijar la forma de `StandardVersion` (`edition + amendments + adoption_body`) y los cuatro tipos de `StandardRelationship`, porque `standards-and-requirements-engine` y `internal-audit-core` (criterios 19011) podrían modelarlo distinto. Una línea basta. |
| A8 | RLS + prefijo de almacenamiento; monolito modular + outbox; pg-boss con interfaz abstracta; cadena de hashes; stack por defecto | AD-2, AD-5 (`JobScheduler`), AD-6, Stack | cubierto | — |
| A9.1 | La plataforma es un sistema de IA: registrada en el inventario del tenant plataforma y sujeta a A.6, A.8, A.9 | AD-17; AI §9; DATA `ai_systems.is_platform_self` | cubierto | — |
| A9.2 | **WCAG 2.2 AA explícita** | 0 apariciones en la espina; SYSTEM §9 ("axe en E2E `[ASSUMPTION]`") | parcial | Añadir a AD-23 "auditoría axe sin errores críticos en todas las pantallas del E2E" y a la fila `packages/ui` del árbol "design system WCAG 2.2 AA, teclado, ≥ 768 px". Es criterio de salida §16.2.5 y hoy la espina no lo nombra. |
| A9.3 | Residencia por tenant; copias de seguridad y DR con RPO/RTO; legal hold | AD-20 (`data_region`), AD-21 (`LegalHold`); SYSTEM §7 (copias 35 días, PITR, restauración probada en hardening, RPO/RTO D-7) | parcial | AD-20 debería citar RPO ≤ 1 h / RTO ≤ 8 h (NFR-008, D-7) y "prueba de restauración en `mvp-hardening` y semestral", aunque sigan `[ASSUMPTION]`. |
| A9.4 | Coste y límites de uso de LLM por tenant (presupuesto, caché, tokens); proveedor abstraído | AD-17 (`LLMUsageRecord` con coste); árbol `ai-core # presupuesto, caché`; AI §3.2-§3.4 | cubierto | — |
| A9.5 | Pruebas de dominio con fixtures de `resumenes/` (38/26/11/7) | AD-23; DATA §2.1, §11 | cubierto | — |
| A9.6 | Pruebas de contrato de conectores con mock etiquetado | AD-18, AD-23; INTEGRATION §10 | cubierto | — |
| A10 | Nodo "documento fuente" antepuesto a la cadena de trazabilidad | AD-7 (`SeedRecordSource`, cita `resumenes/NN §sección`); TRACEABILITY_MATRIX columna `source_doc` | cubierto | — |
| A11 | Herramientas verificadas (BMAD headless, OpenSpec, límite `context` 51.200 bytes) | `.memlog.md`; Stack verificado en web | cubierto | Registrar en la espina que ningún `design.md` de OpenSpec debe embeber la espina completa (34 KB) más un derivado (30 KB) porque superaría el límite de `context` de OpenSpec; remitir por referencia. |
| A12 | **Dos puertas nuevas**: publicar (PR, push, despliegue) y dependencias copyleft/no auditadas | Copyleft: SECURITY §10, SYSTEM §9 (`license-checker`). Publicar: **en ningún documento** | parcial | Ver C18. |
| A13 | Checkpoint I (planificación aceptada) antes de implementar `foundation-project-bootstrap` | Mapa capacidad fila 1 ("Checkpoint de planificación aceptado"); SYSTEM §11 fila 1 | cubierto | — |

---

## 3. PRD §7 NFR, §8 datos, §9 invariantes, §16.2, §17 D-1..D-9, §18

### 3.1 §7 Requisitos no funcionales

| NFR | Exigencia | Dónde aterriza | Estado | Corrección propuesta |
|---|---|---|---|---|
| NFR-001 | TLS 1.2+, cabeceras, CSRF, rate limiting (100/min), Zod, URLs firmadas ≤ 15 min ligadas al tenant, ZAP baseline | AD-13 (≤ 15 min), AD-15 (Zod); SECURITY §5-§7, §12; API §11 | parcial | Ver C16: la espina no nombra cabeceras, CSRF ni rate limiting. |
| NFR-002 | Ningún secreto ni dato ajeno al cliente; pruebas por cambio | AD-2, AD-14, AD-23 (`tests/security`) | cubierto | — |
| NFR-003 | Escaneo de dependencias, SAST, secretos; copyleft con aprobación | SECURITY §10; SYSTEM §9; capability 1 | parcial | Añadir a AD-23 la categoría "cadena de suministro" (escaneo, SBOM, licencias) para que `foundation-project-bootstrap` la herede de la espina y no solo de SECURITY. |
| NFR-004 | Tipo y tamaño (≤ 100 MB) en MVP; **antimalware y procesamiento aislado en F2** | AD-13: "escaneo antivirus asíncrono (ClamAV) antes de que la evidencia pase a `Collected`"; AD-20: `clamav` en `docker-compose` local; SECURITY §8.4 lo detalla como MVP; ROADMAP F2-27 y PRD PR-EXT-010 lo fijan en **F2** | **contradicción** | Dos salidas válidas: (a) mantener en la espina el **puerto** `AntivirusScanner` y el campo `scan_status` desde `evidence-library`, con adaptador `NoopScanner` etiquetado honestamente en UI ("sin escaneo antimalware en MVP") y adaptador ClamAV en F2-27, alineando con el PRD; o (b) registrar el adelanto de ClamAV al MVP como decisión que cambia fase del PRD y llevarla a aprobación humana. La espina no puede presentarlo como `[ADOPTED]` sin una de las dos. |
| NFR-005 | Pruebas de seguridad bloqueantes en cada cambio | AD-23; SECURITY §12 | cubierto | — |
| NFR-006 | Escalabilidad de diseño (miles de sistemas, cientos de miles de evidencias); carga en F2 | DATA §9 (índices, cursor); Deferred no la lista | parcial | Añadir a Deferred "prueba de carga (200 usuarios, 100 000 evidencias) — F2-27". |
| NFR-007 | Paginación/filtrado en servidor, informes asíncronos, índices en `tenant_id, status, owner_id, requirement_id, control_id, risk_id, ai_system_id, created_at, due_date`; p95 < 500/300 ms | AD-15 (cursor), AD-16 (`ReportRun` asíncrono); DATA §9 | parcial | Ver C14 (índices en la espina). Los umbrales p95 no aparecen en ningún documento de arquitectura: añadirlos a AD-19 como métricas OTel que `mvp-hardening` verifica. |
| NFR-008 | Disponibilidad ≥ 99,5 %, RPO ≤ 1 h, RTO ≤ 8 h, copias diarias cifradas en región, restauración semestral | SYSTEM §7 (35 días, PITR); espina solo "pendiente Q6" | parcial | Ver A9.3. |
| NFR-009 | WCAG 2.2 AA, teclado, responsive ≥ 768 px, axe sin críticos, revisión manual de formularios de decisión | SYSTEM §9 (axe); "responsive"/"768"/"teclado": 0 apariciones en espina y derivados | parcial | Ver A9.2. |
| NFR-010 | Ningún módulo completo sin estados vacío/carga/error, paginación, accesibilidad y responsive verificados | 0 apariciones en espina y derivados | ausente | Añadir a la convención de UI (`packages/ui`): componentes `EmptyState`, `LoadingState`, `ErrorState` únicos y obligatorios en toda lista/detalle; añadir a AD-23 "checklist de módulo (NFR-010) en la Definition of Done de todo cambio con UI". |
| NFR-011 | Sin implementación solo mock; E2E que recorre toda entrada de navegación y verifica backend + auditoría | Solo AI §12 y fila 22 del mapa | ausente (como convención/prueba) | Ver C19. |
| NFR-012 | Errores uniformes con código, `correlation_id`, remediación, idioma del usuario | AD-15, convención de errores; API §6 (`Content-Language`) | cubierto | — |
| NFR-013 | OTel completo; **prueba de redacción** (ningún log con secretos ni datos de evidencia) | AD-19 ("nunca con secretos ni contenido de evidencia"); la prueba no está en AD-23 | parcial | Añadir "prueba de redacción de logs" a la lista de AD-23. |
| NFR-014 | `correlation_id` de extremo a extremo | AD-5, AD-19; EVENT §9 | cubierto | — |
| NFR-015 | `data_region` por tenant; mono-región en MVP | AD-20; Deferred "multi-región" | cubierto | — |
| NFR-016 | Retención configurable sin borrado silencioso | AD-21; DATA §8 | cubierto | — |
| NFR-017 | Toda operación atribuible y exportable; verificación de cadena en UI/API | AD-6; API §13 | cubierto | — |
| NFR-018 | Bilingüe ES/EN en UI, **correos, informes**, explicaciones; 100 % de claves; glosario accesible desde la UI | AD-12 (UI, prueba de claves); DATA (plantillas por `locale`); capability 19 (informes ES/EN). "Glosario de UI accesible desde cualquier término técnico": 0 apariciones | parcial | Añadir a la convención i18n: componente `GlossaryTerm` en `packages/ui` alimentado por `resumenes/13` vía la semilla (`glossary_terms`, 26 términos de la cláusula 3 + bilingües), para que FR-132 no se implemente ad hoc. |
| NFR-019 | Solo migraciones; reversibles; irreversibles con aprobación | AD-20; DATA §10; SYSTEM §7 | parcial | Ver C14. |
| NFR-020 | Sin texto literal; cita por registro; revisión humana por versión; prueba de coincidencias largas | AD-7, AD-23; DATA §11 (n-gramas ≥ 12) | cubierto | — |
| NFR-021 | Nueva norma / conector / informe / **jurisdicción** sin reescritura | AD-7, AD-16, AD-18, AD-23 (pruebas de arquitectura); jurisdicción: DATA `RegulatoryRequirement` (F2) | parcial | Añadir "jurisdicción = `RegulatoryRequirement` + `StandardRelationship`, sin `hardcoding`" a Deferred con condición F2 (Q7). |
| NFR-022 | Documentación viva en `docs/` actualizada con cada cambio archivado | Árbol semilla `docs/`; SYSTEM §10 | parcial | Ver A3 y C22. |
| NFR-023 | Pirámide completa; cada regla de §9 con prueba unitaria | AD-23; DOMAIN §4 (prueba nombrada por PR-DR) | cubierto | — |
| NFR-024 | Pruebas de dominio y de contrato | AD-23 | cubierto | — |

### 3.2 §8 Requisitos de datos

| § | Exigencia | Dónde aterriza | Estado | Corrección propuesta |
|---|---|---|---|---|
| 8.1 | Catálogo global con 13 entidades (incl. `SubClause`, `AuditQuestion`, `SeedRecordSource`); versiones desde el inicio (A7); `record_type` por registro | AD-7 lista 12 (omite `SubClause`); DATA §2.1 fusiona `Clause`/`SubClause` en `clauses` con `parent_id` | parcial | Añadir `SubClause` a la lista de AD-7 o anotar que se modela como `Clause` jerárquica. Ver A7 para versiones. |
| 8.2 | Semilla semver en `packages/standards-seed`, campos mínimos, revisión humana, cambiar paráfrasis = interpretación normativa | AD-7; DATA §11, §13 | cubierto | — |
| 8.3 | Derechos de autor: nunca almacenar/mostrar/exportar texto literal; informes citan por identificador | AD-7; REPORTING §1 | cubierto | — |
| 8.4 | Separación catálogo/estado (D-3) | AD-7 `[pendiente ratificación D-3]` | cubierto | — |
| 8.5 | Campos comunes; enumeraciones canónicas del addendum §C | AD-11, AD-12 | cubierto | — |
| 8.6 | Nombres de evento `UPPER_SNAKE`; catálogo DOMAIN_MAP §5; Administration único escritor | AD-5, AD-6; EVENT §3 | cubierto | — |
| 8.7 | Cada entidad con un único propietario; **los 21 contextos existen en el modelo desde el MVP** | AD-3; DATA §3.2 sin Lifecycle ni Incidents | parcial | Ver A1. |
| 8.8 | `classification` en evidencia, `personal_data`/`sensitive_data` en sistemas, exportaciones respetan clasificación | DATA §3.1, §12; la espina no lo menciona | parcial | Añadir `classification` a la lista de "extensiones habituales" en AD-11 (es transversal: Evidence, Documents, Reporting, exportaciones). |

### 3.3 §9 Invariantes PR-DR-001..030

| Grupo | Dónde aterriza | Estado | Corrección propuesta |
|---|---|---|---|
| PR-DR-001..004 (SoA) | AD-9; DATA §4 (`CHECK`); DOMAIN §4 | cubierto | — |
| PR-DR-005..007, 028 (riesgo/impacto) | Mapa fila 10-11; DOMAIN §3.10-3.11, §4 | cubierto | — |
| PR-DR-008, 012, 013, 014, 016, 025, 026 (F2/F3) | AD-17, AD-18; DOMAIN §4 | cubierto | — |
| PR-DR-009, 010 (puntuaciones, "certificado") | AD-12, AD-16 | cubierto | — |
| PR-DR-011 (subida ≠ aceptada) | AD-10; DATA (constraint + servicio) | cubierto | — |
| PR-DR-015, 017 (borrado, registro inmutable) | AD-6, AD-11, AD-21 | cubierto | — |
| PR-DR-018, 019 (normativo vs orientación; nunca inventar) | AD-7, AD-9 | cubierto | — |
| PR-DR-020, 021 (demo/mock etiquetados; conector validado) | AD-18; mapa fila 23; DATA `tenants.is_demo` | cubierto | — |
| PR-DR-022..024 (auditoría, CAPA) | Mapa fila 15-17; DOMAIN §3.18-3.19 | cubierto | — |
| PR-DR-027 (21 elementos documentales) | Mapa fila 20; DATA `mandatory_documented_information_catalog`; DOMAIN §3.15 | cubierto | — |
| PR-DR-029, 030 (roles de IA; objetivos coherentes) | Solo DOMAIN §3.8, §3.13, §3.16, §4 | parcial | Son reglas **entre contextos** (AIMS → Controls & SoA; Documents → Objectives). Añadir a AD-9 "`AIRolesReadPort` alimenta la aplicabilidad sugerida" y a AD-10 o AD-16 "`defineObjective` exige `DocumentReadPort.getApprovedPolicy`", o una tabla PR-DR → AD en la espina (hoy la espina cubre 21 de 30 por nombre). |

### 3.4 §16.2 Criterios técnicos de salida del MVP

| # | Criterio | Dónde aterriza | Estado | Corrección |
|---|---|---|---|---|
| 1 | Pruebas de dominio 38/26/11/7 | AD-23 | cubierto | — |
| 2 | Pruebas de seguridad | AD-23 | cubierto | — |
| 3 | Cadena verificable; decisiones con actor/motivo/anterior | AD-6 | cubierto | — |
| 4 | Ninguna clave sin traducción | AD-12 | cubierto | — |
| 5 | **WCAG 2.2 AA automática** | Ausente en espina | parcial | Ver A9.2. |
| 6 | **Ningún botón ni entrada sin implementación** | Ausente en espina | ausente | Ver C19. |
| 7 | Semilla sin literal, cita por registro | AD-7, AD-23 | cubierto | — |
| 8 | SDK con mock etiquetado y pruebas de contrato | AD-18, AD-23 | cubierto | — |
| 9 | **Matriz con una fila por `PR-*` del MVP** | Ausente en espina; matriz solo cubre cambios 1-5 | parcial | Ver C22 y §6. |
| 10 | Checkpoint humano de cierre | Mapa fila 24 | cubierto | — |

### 3.5 §17 Decisiones D-1..D-9

| D | Decisión | Dónde aterriza | Estado | Corrección |
|---|---|---|---|---|
| D-1 | Política y objetivos en MVP sin LLM | Mapa filas 7-8; DOMAIN §3.15-3.16 | cubierto | — |
| D-2 | **`Not Applicable` de requisito restringido; interpretación normativa, aprobación obligatoria** | Solo DATA §4 y §13, DOMAIN §3.8; **la espina y SYSTEM §11 no la listan** | parcial | La espina afirma que "toda decisión que toca interpretación normativa queda pendiente de aprobación humana" pero no nombra D-2. Añadir a AD-7 (o a la tabla Deferred como "pendiente de aprobación, antes de `standards-and-requirements-engine`") y a SYSTEM §11. |
| D-3 | Separación catálogo/estado | AD-7 | cubierto | — |
| D-4 | `Finding` genérico + especializaciones; `AIProvider` ≠ `Supplier` ≠ `Integration` | ERD de la espina (FINDING); DATA §3.3 (base + extensión 1:1, `FindingPort.register`); DOMAIN §3.9 | parcial | El patrón "tabla base + extensión 1:1 creada por `FindingPort.register` de CAPA en la misma transacción" es exactamente lo que dos cambios (11 Impact, 15 Audit) harían distinto sin la espina: subirlo a AD-3 o a un AD propio, junto con la triple distinción `AIProvider/Supplier/Integration`. Ver también inversión I3 (§5). |
| D-5 | `Operation & Monitoring` (F2) | DOMAIN §3.12 | cubierto | La enumeración vive en `kernel/enums` (AD-12): anotar allí el valor para que `ai-inventory-core` no siembre `Monitoring`. |
| D-6 | Matriz rol × permiso addendum §B | AD-8 | cubierto | — |
| D-7 | Umbrales operativos (RPO/RTO, 30 días, 7 años, 7 días, 100 MB, p95, MFA por defecto) | Repartidos: AD-13 (15 min), DATA §8 (7 años), SECURITY §1 (7 días, MFA), API §5 (100 MB), SYSTEM §7 (RPO/RTO); p95 y 30 días: solo en PRD | parcial | Una fila "Umbrales operativos (D-7)" en Consistency Conventions con los valores y su fuente evita que cada cambio los redefina; hoy hay que leer cinco documentos. |
| D-8 | Nombres de los 5 primeros cambios; 24 cambios | Mapa capacidad → arquitectura | cubierto | TRACEABILITY_MODEL §6.2 sigue usando `standards-engine-seed` (nombre pre-D-8): propagar el renombrado al insumo de nivel 2. |
| D-9 | Política/objetivos justo tras contexto/alcance | Mapa filas 7-8 | cubierto | — |

### 3.6 §18 Preguntas abiertas

| # | Pregunta | Dónde aterriza | Estado | Corrección |
|---|---|---|---|---|
| 1 | Algoritmo de riesgo residual | Deferred; REPORTING §6.3 | cubierto | — |
| 2 | Criterios de preparación | Deferred; REPORTING §6.4 | cubierto | — |
| 3 | Recálculo de estado de requisito = sugerencia + confirmación | DOMAIN §3.8, §5 (`RequirementStatusSuggestion`) | cubierto | — |
| 4 | People & Competence | Deferred | cubierto | — |
| 5 | `Exception` | Deferred | cubierto | — |
| 6 | AI Assistance como contexto separado | AD-17; árbol; DOMAIN §5 | cubierto | — |
| 7 | Administration único escritor | AD-6 | cubierto | — |
| 8 | Separación de funciones en tenants pequeños | AD-10 (pendiente) | cubierto | — |
| 9 | Panel multi-cliente consultoras | Deferred | cubierto | — |
| 10 | **Mapeo cruzado ISO/IEC 27001** | 0 apariciones en espina y derivados | ausente | Añadir a Deferred: "mapeo 42001 ↔ 27001 mediante `StandardRelationship`/`RequirementControlMapping.source = cross_standard` (F3; condición: decisión de producto §12)". La forma ya la soporta DATA §2.1; solo falta nombrarlo para que nadie lo modele como texto libre. |

---

## 4. `DOMAIN_MAP.md` §4 (entidades no listadas en §5) y §7 (decisiones abiertas)

| Entidad (P2 §) | Contexto (DOMAIN_MAP) | Dónde aterriza | Estado | Corrección |
|---|---|---|---|---|
| `CandidateAISystem` (§18) | AI Portfolio | DOMAIN §3.9 (F2); INTEGRATION §2 pipeline; AD-18; **sin tabla en DATA §3.2** | parcial | Ver A1: añadir `candidate_ai_systems` (F2) a DATA o declarar el criterio "existe en el modelo". |
| `AssessmentTemplate` / `Question` / `Answer` (§10) | Impact | DATA §3.2 (`assessment_templates`, `assessment_questions`, `assessment_answers`); DOMAIN §3.11 | cubierto | — |
| `ControlTest` (§14) | Controls & SoA | DATA §3.2 (`control_tests` F2); DOMAIN §3.13 | cubierto | — |
| `EvidenceProvenance` (§15) | Evidence | DATA §3.2 (`evidence_provenance`, MVP modelo); DOMAIN §3.14; INTEGRATION §4 | cubierto | — |
| `ImprovementOpportunity` (§56) | CAPA | DATA §3.2; DOMAIN §3.19 | cubierto | — |
| `ScoreSnapshot` (§39) | Objectives & KPIs | AD-16; DATA §3.2; REPORTING §6 | cubierto | — |
| `NotificationRule` (§30) | Notifications | DATA §3.2 (`notification_rules`); DOMAIN §3.4 | cubierto | — |
| `DataQualityCheck` (§55) | Administration | DATA §3.2; DOMAIN §3.2 | cubierto | — |
| `ImportJob` (§54) | Administration | DATA §3.2; DOMAIN §3.2 | cubierto | — |
| `Agent` (§50) | AI Portfolio (F2/F3) | DOMAIN §3.9 (`Agent (F2)`); sin tabla | parcial | Anotar en Deferred "gobierno de agentes de IA (F3, A1)" con `Agent` como subtipo de `AIAsset` para que no se cree entidad paralela. |
| `Permission` | Identity & Organization (DOMAIN_MAP §4) frente a `packages/kernel/permissions.ts` (AD-8) | DOMAIN §3.1 aclara "`Permission` (asignación)" vs catálogo en kernel | cubierto | Anotar la aclaración en AD-8 (catálogo = kernel, asignación = Identity) para que DOMAIN_MAP §4 y la espina no parezcan contradecirse. |
| `Policy`/`PolicyVersion` | Documents & Policies | DATA modela `policies` como subtipo de `documents` `[ASSUMPTION]` | cubierto | — |
| Lifecycle (`LifecycleStage/Gate/Review`), Incidents (`Incident/IncidentEvent`) | F2 | Paquetes en el árbol; DOMAIN §3.12, §3.17; **sin filas en DATA §3.2** | parcial | Ver A1. |
| §7.1 separación catálogo/estado | | AD-7 | cubierto | — |
| §7.2 People & Competence | | Deferred | cubierto | — |
| §7.3 `Exception` | | Deferred (Controls & SoA + `Approval`) | cubierto | — |
| §7.4 AI Assistance separado, inventariado en AI Portfolio | | AD-17; DOMAIN §5 | cubierto | — |
| §7.5 Administration único escritor, consume outbox completo | | AD-6 | cubierto | — |

---

## 5. `ROADMAP.md` §3: ¿la espina permite construir los 24 cambios en ese orden?

Método: para cada cambio, contrastar lo que su fila del mapa capacidad → arquitectura y sus AD exigen con lo que la hoja de ruta entrega antes. Se señalan solo dependencias hacia un cambio **posterior**.

| # | Inversión | Evidencia | Gravedad | Corrección propuesta |
|---|---|---|---|---|
| I1 | **2 ← 5, 3 ← 5**: AD-4 exige que *toda* mutación escriba en `outbox_events` en la misma transacción desde el cambio 2 (criterio de salida del 2: "`TENANT_CREATED` en outbox"); pero `packages/events` y `packages/jobs` se crean en el cambio 5 (mapa fila 5; EVENT §1: "lo materializa el cambio 5"); el mapa del cambio 1 lista solo `kernel, db, i18n, ui, observability` | Espina, mapa filas 1, 2, 5; EVENT_ARCHITECTURE línea 36 | **alta** | Mover a `foundation-project-bootstrap` el sustrato L1 mínimo: tabla `outbox_events`, `appendOutbox`, `processed_events`, puerto `JobScheduler` con adaptador pg-boss y relay básico. Dejar en el cambio 5 el catálogo de eventos, versionado, reintentos/dead-letter, `object_versions`, `Approval` mínimo y la observabilidad de colas. Actualizar la fila 1 del mapa (`packages/events, jobs`) y EVENT §1. |
| I2 | **4 ← 5**: AD-6 define `audit-trail-writer` como *consumidor pg-boss singleton por tenant* que procesa el outbox; sin relay ni pg-boss (cambio 5) el cambio 4 no puede cumplir su criterio "entrada por cada mutación de los cambios 2-3"; la propia hoja de ruta pone "Administration consume todos los eventos hacia el registro" como criterio del **5** | AD-6; ROADMAP filas 4-5; EVENT §8 | **alta** | Se resuelve con I1 (el relay y pg-boss existen desde el 1). Alternativa si no se acepta I1: intercambiar 4 y 5 (rompe D-8 solo en orden, no en nombres) y registrar el desvío en PRD §17. |
| I3 | **11 ← 16, 15 ← 16**: `ImpactAssessmentFinding` (cambio 11) y `AuditFinding` (cambio 15) son extensiones 1:1 de `findings`, tabla propiedad de CAPA que se crea vía `FindingPort.register` (DATA §3.3, D-4); `domain-capa` llega en el cambio 16 | DATA §3.3; DOMAIN §3.11, §3.18, §3.19; ROADMAP filas 11, 15, 16 | **alta** | La espina debe declarar (como hace con `domain-workflow` y `Approval` mínimo en el 5) que `domain-capa` existe desde `impact-assessment-core` con `Finding` base + `FindingPort` mínimo, y que `capa-core` añade no conformidad, causa raíz, acción correctiva y verificación. Añadir a las filas 10-11 y 15-17 del mapa. |
| I4 | **12 ← 13**: criterio del cambio 12 "26 controles activan tareas **y solicitudes** [de evidencia]"; `EvidenceRequest` la crea `domain-evidence` al consumir `CONTROL_ACTIVATED` (EVENT §11(a)), y Evidence es el cambio 13 | ROADMAP fila 12; EVENT §11(a); DOMAIN §3.14 | media | Reformular el criterio del 12: "`CONTROL_ACTIVATED`/`CONTROL_DEACTIVATED` emitidos y consumidos por Workflow (tarea mínima); 12 `Not Applicable` no emiten activación"; verificar la creación de `EvidenceRequest` en el criterio del 13. O adelantar un `domain-evidence` mínimo (`evidence_requests`) al 12. |
| I5 | **9 ← 10/11**: criterio del cambio 9 "cambio de versión emite evento **consumido por riesgo e impacto**"; Risk e Impact son los cambios 10 y 11 | ROADMAP fila 9; EVENT §11(b) | baja | Criterio del 9 = evento en outbox y en `audit_entries`; la consumición se verifica en los criterios de 10 (`reassessment_required`) y 11. |
| I6 | **12 ← 19**: el cambio 12 incluye "informe de completitud" (FR-067 = *SoA Report* #3, exportable PDF/CSV/JSON ES/EN, con versión de SoA y norma); AD-16 dice que todo informe es una `ReportDefinition` con `ReportRun` asíncrono y render en worker, que llegan en el 19 (que además vuelve a listar los 4 informes esenciales, incluido el de SoA) | ROADMAP filas 12, 19; AD-16; REPORTING §7 | media | Fijar en AD-16: "cada cambio de dominio entrega su(s) dataset(s) `ReportDataSourcePort` (p. ej. `soa.items`, `soa.completeness` en el 12); las `ReportDefinition`, el render y la exportación llegan en `reporting-essentials`". El criterio del 12 pasa a "`SoAReadPort.getCompleteness` y dataset `soa.items` con los 38 controles incl. `Not Applicable`". |
| I7 | **17 ↔ 18**: `aggregateInputs` de la revisión por la dirección lee `KPIReadPort` y `ScoreReadPort` (DOMAIN §3.20) y las 14 entradas incluyen objetivos y KPI (FR-098); `ScoreSnapshot`/KPI llegan en el 18, que depende de 12, 13, 15, 16 y **no** del 17 | ROADMAP filas 17-18; DOMAIN §3.20 | baja | Intercambiar 17 y 18 (18 solo depende de 12, 13, 15, 16, así que es viable) o anotar que en el 17 las entradas KPI/puntuación se marcan "sin datos en el periodo" (FR-098 lo permite) y se completan en el 18. |
| I8 | **15 ← 19, 17 ← 19**: los criterios/FR de auditoría (FR-092 "informe exportable FR-080") y de revisión por la dirección (FR-098 "acta exportable a PDF FR-080") dependen del servicio de renderizado del 19 | PRD FR-092, FR-098; ROADMAP fila 19 ("servicio de renderizado") | baja | Declarar en AD-16 que el `render` HTML→PDF es un job de `packages/` disponible desde el 15 (mover "servicio de renderizado" al 15) o marcar la exportación como verificada en el 19. |
| I9 | **7 ← 18** (suave): el centro de control 4.4 (FR-028) exige que cada indicador sea una puntuación transparente (FR-100) o un contador con enlace; `ScoreSnapshot` es del 18 | ROADMAP fila 7; PRD FR-028 | baja | Anotar en el mapa fila 7: "solo contadores con enlace a la lista fuente; puntuaciones en el 18". |
| I10 | **3 ← ?**: SECURITY §1 guarda el `client_secret` OIDC como `secret_ref` (AD-14) en el cambio 3, pero `packages/secrets` no está asignado a ningún cambio del mapa (la fila 3 cita AD-14 sin listar el paquete; la fila 21 tampoco) | Mapa filas 3, 21; SECURITY §1, §4 | baja | Añadir `packages/secrets` (puerto + adaptador `env-encrypted`) a la fila 1 o 3 del mapa. |
| — | Sin inversión: 5 → 7..13 (`Approval` mínimo desde el 5), 6 → 7, 13 → 21 (`EvidenceProvenance` en Evidence), 2 → 23 (`tenants.is_demo` desde el 2), 20 → 22, 23 → 24 | | cubierto | — |

Conclusión de §5: con I1-I3 corregidas en la espina (sustrato de eventos en el cambio 1; `domain-capa` mínimo desde el 11) y I4-I6 reformuladas en los criterios de salida, el orden de los 24 cambios es construible sin dependencia hacia adelante.

---

## 6. `TRACEABILITY_MODEL.md` §3-§5 frente a `docs/compliance/TRACEABILITY_MATRIX.md`

| # | Exigencia del modelo | Matriz | Estado | Corrección |
|---|---|---|---|---|
| T1 | 16 columnas de §4.2 en el orden dado | 16 columnas, mismo orden y nombres | cubierto | — |
| T2 | Frontmatter `titulo, ultima_actualizacion, ultimo_cambio_archivado, version_semilla` | Presentes (más `fecha, estado, nivel, inputs, spine`) | cubierto | — |
| T3 | Estructura §1 Resumen / §2 Principal / §3 Sin requisito ISO / §4 Brechas / §5 Historial | Presente | cubierto | — |
| T4 | Estados: `planned, in_progress, implemented, verified, unverified, gap, deferred(F2), deferred(F3), retired` | Solo `planned` | cubierto | — |
| T5 | Formato `source_doc` = `resumenes/NN §sección`; `iso_requirement` = `42001:5.3`; capacidad kebab | Cumple (`resumenes/01 §4 5.3`, `42001:5.3`, `identity-and-rbac`) | cubierto | — |
| T6 | `record_type` ∈ {`normative_requirement`, `implementation_guidance`, `recommended_practice`, `evidence_example`} (§3) | Filas §3 usan `product_requirement` (valor de `CLAUDE.md`, no del modelo) | parcial | Corregir el artefacto **superior** (TRACEABILITY_MODEL §3, fila "Tipo de registro") añadiendo `product_requirement` como valor válido de la columna 3 para filas con `[1] = —`; la matriz ya lo usa correctamente en espíritu. |
| T7 | Rutas de implementación `packages/...#símbolo`; abreviaturas permitidas si el pie las declara | Matriz declara `pkg:`, `app:`, `t:` | cubierto | Los ejemplos del modelo §6 usan `packages/domain/soa/` (pre-espina) y `standards-engine-seed` (pre-D-8): actualizar el insumo a `packages/domain-controls-soa/` y `standards-and-requirements-engine`. |
| T8 | §4.3 integridad: todo `PR-*` aparece; 38 controles y 4.1-10.2 aparecen; `verified` con columnas 11-13; script `pnpm traceability:check` en CI (MVP tardío) | Matriz cubre solo 22 PR de los cambios 1-5 (esperado); script no aparece en AD-23 ni en `tests/` | parcial | AD-23: añadir `tests/traceability` o script `traceability:check` a la lista de pruebas y a la Definition of Done del archivo (§16.2.9). |
| T9 | §5 procedimiento al archivar (9 pasos), commit `docs(compliance): update traceability matrix for <name>` | Matriz remite al procedimiento; la espina no lo vincula | parcial | Ver C22. |
| T10 | §2.1 regla 5: en F2, `docs/compliance/traceability.json` mostrado en el tenant plataforma (A9) | 0 apariciones en espina y derivados | ausente | Añadir a Deferred: "exportación JSON de la matriz y su visualización como evidencia A.6.2.3/A.6.2.7 del tenant plataforma — F2, con AI §9". |
| T11 | Columna `context` = bounded context de DOMAIN_MAP | Filas de NFR de plataforma (CI, observabilidad, WCAG, migraciones) llevan `Administration`, que no es su propietario (viven en `apps/*`, `packages/observability`, `packages/ui`, `packages/db`) | parcial | Permitir en el modelo el valor `plataforma` (o `—`) para filas L0/L1 sin contexto de dominio, y corregir las 8 filas del cambio 1. |
| T12 | §1 Resumen por cláusula y dominio del Anexo A | Diferido al cambio 6 con nota explícita | cubierto | — |

---

## 7. `MASTER_ISO42001_PLATFORM_REQUIREMENTS.md` §43 (rutas API) y §37 (navegación)

### 7.1 §43 frente a `API_ARCHITECTURE.md` y AD-15

| # | Exigencia | Dónde aterriza | Estado | Corrección |
|---|---|---|---|---|
| R1 | 27 prefijos de recurso (`/organizations` … `/management-reviews`) | API §2 lista los 27 más `/approvals`, `/notifications`, `/audit-trail`, `/health`, `/openapi.json` | cubierto | — |
| R2 | Prefijo `/api/` | Espina y API usan `/api/v1` `[ASSUMPTION]`; elevado a aprobación humana (API §15 A-1, SYSTEM §11) | contradicción declarada | Ninguna adicional: correctamente escalada. Cuando se apruebe, actualizar el nivel 1 (§43) para que el inferior no lo sobreescriba en silencio. |
| R3 | Un recurso, un contexto propietario (AD-3) | API §2: **`/findings` → Audit**; DOMAIN_MAP §4 y D-4: `Finding` genérico es de **CAPA**, `AuditFinding` es su extensión | **contradicción** | `/findings` → CAPA (lista genérica filtrable por `source_type`); hallazgos de una auditoría como subrecurso `/audits/{auditId}/findings` (handler de Audit que invoca `FindingReadPort`). Corregir API §2 y añadir a AD-15 la regla "el prefijo de primer nivel pertenece al propietario de la tabla base". |
| R4 | Consistencia de rutas entre derivados | API §5: `GET /reports/runs/{runId}`; REPORTING §4, §12: `POST /api/v1/report-runs`, `GET /api/v1/report-runs/{id}/download`, `/dashboards` — ninguno de los dos últimos figura en API §2 | **contradicción** (entre derivados) | Fijar en AD-15 la convención de agregados propios vs subrecursos: `ReportRun` es agregado (tiene ciclo de vida, hash, descarga) → `/report-runs`; añadir `/report-runs` y `/dashboards` (Reporting) a API §2 y corregir API §5. |
| R5 | `/providers` | API §2 → AI Portfolio (`AIProvider`), coherente con D-4; pero §37 tiene también "Integrations → Providers" (catálogo de conectores) | parcial | Anotar en API §2 que el catálogo de adaptadores de conector es `/integrations/providers` (o `/connector-providers`), nunca `/providers`, para no mezclar A.10 con el hub de conectores (D-4). |
| R6 | Esquemas tipados, errores consistentes, paginación, filtrado, orden, autorización, audit logging | AD-15, AD-4, AD-8; API §4-§8 | cubierto | — |
| R7 | `/lifecycle`, `/incidents` | API §2 F2 | cubierto | — |

### 7.2 §37 (navegación) frente a la espina

| # | Exigencia | Dónde aterriza | Estado | Corrección |
|---|---|---|---|---|
| N1 | Árbol de navegación de 10 áreas / 41 entradas (Dashboard, AI Portfolio, ISO 42001, Risk, Impact, Evidence, Audit, Operations, Reports, Integrations, Administration) | Solo la fila 22 del mapa ("navegación §37"); ningún documento mapea áreas → contextos → fases | ausente | Añadir a la espina una convención "Rutas de UI": `apps/web/app/[locale]/<area>/<recurso>` con la tabla §37 → contexto propietario → fase (MVP/F2/F3). TRACEABILITY_MODEL §6 ya asume `app/[locale]/iso42001/soa/page.tsx`; la espina no fija ni `[locale]` ni el nombre de área. |
| N2 | Entradas cuyo módulo es F2/F3: `Lifecycle`, `Incidents`, `Changes`, `Exceptions`, `Discovery`, `Sync History`, `Report Builder` | FR-131: "módulos deshabilitados por feature flag desaparecen de la navegación"; CLAUDE.md prohíbe botones falsos; **feature flags: 0 apariciones** | ausente | Ver C19: convención `module_flags` por tenant en `tenant_settings`, con prueba E2E NFR-011. |
| N3 | Principios: denso pero legible, responsive, accesible, teclado, audit-friendly | SYSTEM §9 (axe); resto ausente | parcial | Ver A9.2 y NFR-010. |
| N4 | "Impact → Mitigations", "Operations → Changes", "Administration → Notifications", "Reports → Dashboards" | DATA `mitigations`, `change_requests`, `notifications`; REPORTING §9 | cubierto | — |
| N5 | "Integrations → Providers / Connections / Discovery / Sync History" | INTEGRATION §2, §8 (`integrations`, `integration_syncs`, candidatos F2) | cubierto | Ver R5 para el nombre de ruta. |

---

## 8. Otras observaciones menores sobre la espina

| # | Observación | Corrección |
|---|---|---|
| M1 | Frontmatter `companions: ADR-001..ADR-015`; existen 16 ADR | `ADR-001..ADR-016`. |
| M2 | AD-13 y AD-20 nombran ClamAV y SeaweedFS como `[ASSUMPTION]` pero el memlog registra que SeaweedFS sustituyó a MinIO tras verificación; la sección Stack lo fija | Coherente; sin acción salvo NFR-004 (§3.1). |
| M3 | Deferred no incluye: prueba de carga (NFR-006), `traceability.json` (T10), mapeo 27001 (§18.10), jurisdicciones (NFR-021), gobierno de agentes (A1/`Agent`) | Añadir las cinco filas. |
| M4 | La espina cubre por nombre 21 de las 30 PR-DR; DOMAIN §4 cubre las 30 | Añadir a la espina una tabla PR-DR → AD (una línea por regla) o remitir explícitamente a DOMAIN §4 desde AD-23. |
| M5 | Las puertas de aprobación humana están repartidas en SYSTEM §11, DATA §13, API §15, SECURITY §14, EVENT §14, INTEGRATION §12, REPORTING §13, AI §13 y ninguna lista es completa (SYSTEM §11 omite D-2 y A12) | Tabla única "Puertas de aprobación humana" en la espina con referencia a cada derivado. |

---

## 9. Recuento

- Filas evaluadas: 128.
- **Contradicciones**: 4 (NFR-004 antimalware en MVP; `/findings` → Audit; `/reports/runs` vs `/report-runs`; `/api` vs `/api/v1` — esta última correctamente escalada).
- **Ausentes**: 8 (botones falsos / feature flags como convención; NFR-010 estados de módulo; NFR-011 prueba E2E de navegación; §16.2.6; §18.10 mapeo 27001; `traceability.json` F2; convención de rutas de UI §37; feature flags de módulos F2 en navegación).
- **Parciales**: 38.
- **Cubiertos**: 78.
- **Inversiones de la hoja de ruta**: 10 (3 altas: I1, I2, I3; 3 medias: I4, I6; 4 bajas).

Ningún hallazgo exige rehacer la espina; todos se resuelven con adiciones puntuales a AD-3, AD-7, AD-15, AD-16, AD-20, AD-23, a Consistency Conventions, a Deferred y al mapa capacidad → arquitectura, más tres correcciones en `API_ARCHITECTURE.md`, `DATA_ARCHITECTURE.md` y `TRACEABILITY_MODEL.md`.
