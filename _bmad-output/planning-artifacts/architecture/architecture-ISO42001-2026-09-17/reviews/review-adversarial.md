---
title: Revisión adversarial de la espina de arquitectura
artifact: ../ARCHITECTURE-SPINE.md
lens: adversarial (dos unidades un nivel abajo que obedecen todas las AD y aun así construyen incompatiblemente)
units: 24 cambios OpenSpec (docs/product/ROADMAP.md §3, líneas 54-86)
date: 2026-09-17
reviewer: revisor independiente (headless)
status: findings
---

# Revisión adversarial — ARCHITECTURE-SPINE.md

## Veredicto

**No apta todavía como sustrato de construcción.** La espina fija bien lo técnico transversal (RLS, outbox, envelope, hash chain, API, enums), pero deja sin cerrar las costuras *entre contextos*: en 3 zonas dos cambios OpenSpec pueden obedecer las 23 AD al pie de la letra y producir tablas, estados y rutas de mutación incompatibles (Finding y sus especializaciones; el outbox que el registro de auditoría necesita un cambio antes de existir; los puertos de escritura entre contextos que AD-3 prohíbe y AD-10 exige). Varias de estas costuras ya están resueltas en los compañeros (`DATA_ARCHITECTURE.md`, `EVENT_ARCHITECTURE.md`, `DOMAIN_ARCHITECTURE.md`), pero **un compañero no obliga a un cambio OpenSpec**: la propia espina dice que "fija lo que dos cambios construidos por separado podrían decidir de forma incompatible". Lo que solo vive en un compañero es, para esta lente, un agujero. Recomendación: cerrar los 3 críticos y los 6 altos con las AD/Rules propuestas abajo antes del checkpoint 0.

Método: para cada par se construyen dos implementaciones plausibles, se comprueba AD por AD que ambas cumplen, y se muestra el choque. Severidad: **critical** = bloquea la integración o rompe una invariante de cumplimiento; **high** = fuerza refactor de esquema o de contrato entre cambios; **medium** = divergencia que se detecta tarde pero se corrige localmente; **low** = cosmético o de convención.

Resumen: 3 critical · 6 high · 7 medium · 3 low (19 hallazgos, 21 pares de choque).

---

## Hallazgos critical

### C-1 · `Finding` nace tres veces: Impact (11) y Audit (15) se construyen antes que su propietario CAPA (16)

**Pares:** `internal-audit-core` (15) vs `capa-core` (16) sobre `AuditFinding`/`Finding`; `impact-assessment-core` (11) vs `capa-core` (16) sobre `ImpactAssessmentFinding`/`Finding`.

**Cómo ambos cumplen las AD.**
- Cambio 15 (Audit) obedece AD-3 ("cada tabla tiene un único contexto propietario y solo su repositorio la escribe") y AD-11 (`status, version…`): como `domain-capa` no existe aún (16 depende de 15), crea `audit_findings` **completa** con `status`, `severity`, `classification_id`, `description`, `criterion_id`, y emite `AUDIT_FINDING_CREATED` (AD-4/AD-5). Todo legal.
- Cambio 11 (Impact) hace lo mismo cinco cambios antes: `impact_findings` completa con su propio `status` y `severity`.
- Cambio 16 (CAPA) obedece AD-3 y DOMAIN_MAP §4 ("`Finding` genérico con `source_type`; CAPA propietario"): crea `findings (source_type, source_id, status…)` y, como solo puede reaccionar a eventos (AD-3), consume `AUDIT_FINDING_CREATED` e `IMPACT_ASSESSMENT_COMPLETED` para *copiar* una fila base.

**Choque.** Un hallazgo de auditoría existe en dos tablas con dos `status` (Audit lo cierra en su seguimiento; CAPA lo cierra por `closeCapa`), dos `severity` con enumeraciones potencialmente distintas (Audit: clasificación configurable PR-DR-022; CAPA: enum del addendum §C), y dos ids. `FindingReadPort.listOpen` y `AuditReadPort.listOpenFindings` devuelven conjuntos distintos; el informe de preparación (20) y la revisión por la dirección (17) cuentan hallazgos dos veces o ninguna. `DATA_ARCHITECTURE.md` §3.3 propone "tabla base + extensión 1:1 con `FindingPort.register` en la misma transacción `[ASSUMPTION]`", pero (a) es un compañero, (b) exige un puerto de **escritura** entre contextos que AD-3 prohíbe (ver C-3), y (c) exige que `domain-capa` exista antes del cambio 11, cosa que el ROADMAP niega.

**Cierre propuesto — nueva AD-24 (Especialización de agregados compartidos):**
> Toda entidad que DOMAIN_MAP §4 declara "especialización de X" se modela como **tabla base propiedad del contexto de X + tabla de extensión 1:1 propiedad del especializador** (`findings` ← `audit_findings`, `impact_findings`; `documents` ← `policies`). La fila base se crea por el *command port* del propietario (`FindingPort.register`) **dentro de la transacción del llamante** (AD-25). `status`, `severity` y `owner_id` viven **solo en la base**; la extensión no repite ningún campo de la base. El paquete propietario existe con la entidad base mínima y su puerto **desde el primer cambio que la especializa** (`domain-capa` con `Finding` mínimo desde el cambio 11, análogo a `domain-workflow` con `Approval` mínimo desde el 5). Una prueba de arquitectura falla si una tabla de extensión declara `status`.

Además, fila de ROADMAP a corregir (fuera del alcance de esta revisión, pero necesaria): el cambio 11 depende de "`domain-capa` mínimo", o `capa-core` se parte en `finding-base` (antes del 11) + `capa-core`.

---

### C-2 · El registro de auditoría (4) consume un outbox que nace en el cambio 5, y los cambios 2-3 deben escribir un outbox que aún no existe

**Pares:** `immutable-audit-trail` (4) vs `domain-events-outbox-jobs` (5); `organization-tenancy` (2) / `identity-and-rbac` (3) vs (5).

**Cómo ambos cumplen las AD.**
- Cambios 2 y 3 obedecen AD-4 al pie de la letra: cada command hace `transaction { mutate; append outbox }`. Como `packages/events` es del cambio 5, cada uno crea `outbox_events` a su manera (o el 2 la crea y el 3 la extiende con columnas propias). Criterio de salida del 2: "`TENANT_CREATED` en outbox" — exige la tabla.
- Cambio 4 obedece AD-6: "solo el consumidor `audit-trail-writer` escribe, procesando el outbox completo serializado por tenant (clave singleton pg-boss por tenant)". Sin pg-boss (cambio 5) tiene dos salidas legales: (a) leer `outbox_events` con un bucle propio y cursor; (b) escribir `audit_entries` sincrónicamente en el command mediante un `AuditTrailPort.append` — cumple "único escritor" y "append-only", y el criterio "entrada por cada mutación de los cambios 2-3".
- Cambio 5 obedece AD-5: relay que marca `published`, colas `events.<TYPE>`, `processed_events`, y (compañero EVENT §4) borra filas `published` a los 30 días.

**Choques.**
1. Si el 4 eligió (b), al llegar el 5 hay **dos rutas** hacia `audit_entries` (sincrónica y por consumidor) → cadena de hashes con entradas duplicadas o intercaladas fuera de orden; la verificación pasa pero el contenido miente.
2. Si el 4 eligió (a) como consumidor de `events.<TYPE>` cuando aparece pg-boss, el orden por tenant se pierde (AD-5 no garantiza orden entre colas) y `previous_hash` deja de ser reproducible.
3. El 5 puede borrar filas `published` que el cursor del 4 aún no procesó: la espina no vincula la retención del outbox al cursor del writer.
4. Forma del `payload`: AD-4 solo fija `before/after` para `ENTITY_MUTATED`; para eventos del catálogo el writer del 4 no sabe de dónde sacar "previous state / new state" (campos obligatorios de AD-6) → cada publicador decide si incluye `before/after` o no.
5. RLS: AD-2 prohíbe `BYPASSRLS`; el relay y el writer necesitan recorrer **todos** los tenants → `packages/events` (L1) debe leer `tenants` (tabla de un L2), arco prohibido por AD-3.

**Cierre propuesto — Rule de AD-6 endurecida + fila de Consistency Conventions:**
> AD-6 (añadir): `audit-trail-writer` **nunca** consume de `events.<TYPE>`; lee `outbox_events` por `(tenant_id, seq)` desde `audit_trail_cursors(tenant_id, last_seq)` (Administration) y es el **único** consumidor con orden total. Ninguna fila de `outbox_events` se elimina con `seq > audit_trail_cursors.last_seq` del mismo tenant. La única forma de escribir `audit_entries` es ese consumidor; no existe `AuditTrailPort.append`. Todo evento del catálogo que represente transición de estado incluye en `payload` `previous_state` y `new_state` (o `null` explícito); el writer los copia y, si faltan, registra `payload` completo como `new_state`.
> AD-5 (añadir): `outbox_events`, `processed_events`, `object_versions`, `idempotency_keys` y el `append` mínimo del outbox pertenecen a **`foundation-project-bootstrap` (1)**; el cambio 5 añade relay, colas y consumidores. El relay y los barridos multi-tenant se ejecutan con el rol `app_worker` que tiene una política RLS adicional `USING (true)` **solo** sobre `outbox_events`, `processed_events` y `tenants(id)`, nunca sobre tablas de negocio; queda excluido de `BYPASSRLS` (AD-2 lo sigue prohibiendo).

---

### C-3 · AD-3 prohíbe lo que AD-10 exige: puertos de escritura entre contextos (`ApprovalPort`, `TaskPort`, `FindingPort`, `NotificationPort`) y la propagación de transacción

**Pares:** cualquier propietario de decisión (7, 8, 10, 12, 13) vs `tasks-approvals-notifications` (14); `internal-audit-core` (15) vs `capa-core` (16); `management-review-core` (17) vs Workflow (14).

**Cómo ambos cumplen las AD.**
- AD-3: "Entre contextos L2 solo se importa el `index.ts` público del otro (**puertos de lectura** y tipos) y se reacciona a sus eventos". Un cambio literal (p. ej. 12) concluye que **no puede** invocar `ApprovalPort.request` y modela la aprobación solo por eventos: emite `SOA_APPROVAL_REQUESTED`, Workflow lo consume y crea `Approval`; el 12 consume `APPROVAL_GRANTED`.
- AD-10: "El contexto propietario ejecuta la transición al recibir `APPROVAL_GRANTED` (**o sincrónicamente vía `ApprovalPort` en el mismo command**)". Otro cambio literal (p. ej. 8) invoca `ApprovalPort.request/decide` y transiciona en el mismo command.

**Choques.**
1. Dos protocolos de aprobación coexistentes: por evento y por puerto. Workflow (14) debe soportar ambos y, si un propietario implementa **los dos** (consumidor de `APPROVAL_GRANTED` + transición sincrónica), la transición se ejecuta dos veces (`processed_events` no protege la ruta sincrónica) → `INVARIANT_VIOLATION` o versión doble.
2. "En el mismo command" exige que el puerto **se una a la transacción del llamante**; AD-4 solo dice `transaction { mutate; append outbox }` por command. Un cambio implementa `ApprovalPort.decide` abriendo su propia transacción (Workflow escribe su outbox y confirma aunque el propietario haga rollback → `APPROVAL_GRANTED` huérfano); otro pasa `ctx.tx`. Ambas implementaciones cumplen AD-4.
3. Mismo problema para `TaskPort.create` (¿consumidor de Workflow o llamada del propietario?), `FindingPort.register` (C-1) y `NotificationPort.notify`.
4. La prueba de arquitectura `dependency-cruiser` de AD-3 no puede distinguir un puerto de lectura de uno de escritura: o permite ambos (y AD-3 es letra muerta) o prohíbe ambos (y AD-10 no compila).

**Cierre propuesto — nueva AD-25 (Command ports entre contextos y transacción compartida):**
> Además de los `*ReadPort`, un contexto puede ofrecer **command ports** de escritura entre contextos **solo** para agregados compartidos por diseño; la lista cerrada del MVP es `ApprovalPort`, `TaskPort`, `CommentPort`, `FindingPort`, `NotificationPort`. Un command port (a) se ejecuta **siempre dentro de la transacción del llamante** (`ctx.tx` obligatorio en la firma; nunca abre una propia), (b) realiza su propio `authorize` con el principal del `TenantContext`, (c) escribe su outbox en esa misma transacción con `causation_id` = command del llamante. Para cada agregado con command port, la espina fija **una** ruta de mutación: `Approval` → el propietario **siempre** transiciona en el consumidor de `APPROVAL_GRANTED` (nunca en el command que solicita), y `decide` solo lo invoca Workflow; `Task` → **solo** Workflow crea tareas, por consumidor de eventos del catálogo declarados en `EVENT_ARCHITECTURE` §3 (los propietarios no llaman a `TaskPort.create` salvo desde una acción explícita de usuario). Se elimina de AD-10 la frase "o sincrónicamente vía `ApprovalPort` en el mismo command". La prueba de arquitectura permite importar de otro `index.ts` solo símbolos con sufijo `ReadPort`, los cinco command ports listados y tipos.

---

## Hallazgos high

### H-1 · `Approval` "mínimo" (cambio 5) sin forma fijada: seis propietarios (7, 8, 10, 11, 12, 13) aprueban de seis maneras antes del cambio 14

**Pares:** `aims-context-parties-scope` (7) y `aims-policy-objectives-roles` (8) vs `tasks-approvals-notifications` (14); `evidence-library` (13) `EvidenceReview` vs `Approval`; `risk-engine-core` (10) `RiskAcceptance` vs `Approval`; `capa-core` (16) `EffectivenessVerification` vs `Approval`.

**Cómo ambos cumplen.** AD-10 fija los campos de `Approval` pero no `status` (pendiente/decidida), ni `approver_permission`, ni `due_date`, ni `subject_version`, ni de dónde sale `created_by` del objeto para la regla `decided_by ≠ created_by` (Workflow no puede leer la tabla del propietario, AD-3). El cambio 7 pasa `subject_created_by` en `request()`; el 8 no lo pasa y Workflow no puede comprobarlo → separación de funciones **no aplicada** para políticas, cumpliendo AD-10 textualmente. El 13 modela `EvidenceReview (reviewer_id ≠ uploaded_by, evidence.approve)` como aprobación propia (PR-DR-011) **sin** `Approval` — DOMAIN_ARCHITECTURE 3.14 lo respalda — mientras AD-10 dice "`Approval` es la **única** forma de aprobar" y binds Evidence. El 10 hace lo mismo con `RiskAcceptance (expires_at)`; el 16 con `EffectivenessVerification`. Al llegar el 14 con `Workflow`/`WorkflowStep`, las aprobaciones previas carecen de estado `Pending` y no pueden encajarse en pasos.

**Choque.** Cuatro formas de "decisión de cumplimiento" (Approval, EvidenceReview, RiskAcceptance, EffectivenessVerification), cada una con su propia regla de separación de funciones, sin un lugar donde el registro inmutable (PR-DR-017) las vea uniformes; `SegregationOfDutiesException` se registra en unas y no en otras.

**Cierre propuesto — Rule de AD-10 endurecida:**
> `Approval` mínimo (cambio 5) tiene exactamente `{ id, tenant_id, subject_type (enum kernel AggregateType), subject_id, subject_version, subject_created_by, requested_by, approver_permission, status ∈ {Pending, Granted, Rejected, Cancelled}, decided_by, decision, reason, previous_state, new_state, requested_at, decided_at, sod_exception_id }`; el 14 solo añade `workflow_step_id` y plazos. `subject_created_by` y `subject_version` los aporta el solicitante y Workflow rechaza `request()` sin ellos. **Toda** entidad de decisión de dominio (`EvidenceReview`, `RiskAcceptance`, `AuditConclusion`, `EffectivenessVerification`, `ManagementReviewDecision`) lleva `approval_id NOT NULL` hacia el `Approval` que la autorizó; la separación de funciones se comprueba **una sola vez**, en Workflow. Añadir a Consistency Conventions la fila "Decisiones: registro de dominio con campos propios + `approval_id`; nunca un segundo `decided_by`".

---

### H-2 · Tres identidades para "el control": `Control.id` (catálogo global), `SoAItem.id`, `ControlImplementation.id`; nadie fija cuál viaja en eventos, enlaces y puertos

**Pares:** `soa-control-activation` (12) vs `evidence-library` (13) sobre `CONTROL_ACTIVATED.payload` y `EvidenceLink.target`; (12) vs `scoring-kpi-dashboard` (18) sobre `isControlActive`; (12) vs `internal-audit-core` (15) sobre `AuditCriterion.target`.

**Cómo ambos cumplen.** AD-9 fija `SoAReadPort.isControlActive(tenantId, controlId)`; AD-5 fija el envelope (`aggregate_type, aggregate_id`) pero no el payload. El 12 emite `CONTROL_ACTIVATED` con `aggregate_type = 'ControlImplementation'` y `payload.control_id` = id global del catálogo (AD-7: el estado referencia `catalog_id`). El 13, reaccionando, crea `EvidenceRequest.target = ('control', payload.control_id)` y `EvidenceLink(target_type='control', target_id=<id global>)`. Pero la ERD de la propia espina dice `CONTROL_IMPLEMENTATION ||--o{ EVIDENCE_LINK`. Y los **controles adicionales** (`OrganizationControl`, PR-CAP-058) no tienen id de catálogo: el 12 usa `organization_control_id`; `isControlActive(controlId)` con ese id devuelve falso o lanza.

**Choques.** `EvidenceReadPort.getCoverage(controlRef)` y `ControlImplementationReadPort.getStatus` no comparten clave; la cobertura de evidencia de los controles adicionales es siempre 0; al migrar de versión de norma (AD-7, nuevas filas de estado) los enlaces por id global apuntan al control viejo. Además, "activo" no está definido: el 12 lo deriva de `SoAItem.applicability = Applicable AND SoA aprobada`; el 18 lo deriva de `ControlImplementation.status ∈ {Implemented, Operating}` para el denominador (PR-DR-001) → dos denominadores para la misma puntuación.

**Cierre propuesto — Rule de AD-9 endurecida + fila de Consistency Conventions:**
> La clave de un control **en estado de tenant** es siempre `control_implementation_id` (existe una `ControlImplementation` por `SoAItem` aplicable, también para `OrganizationControl`); `Control.id` del catálogo solo aparece como `catalog_control_id` informativo. Todos los eventos `CONTROL_*` llevan `aggregate_type = 'ControlImplementation'` y `payload = { control_implementation_id, soa_item_id, soa_version_id, catalog_control_id | null, organization_control_id | null, previous_state, new_state }`. `SoAReadPort.isControlActive(controlImplementationId)`; *activo* = `SoAItem.applicability = Applicable` **y** SoA aprobada **y** `ControlImplementation.status ≠ Retired`; el estado de implementación no interviene. Convención "Referencias polimórficas": `target_type` es siempre el enum kernel `AggregateType` (PascalCase de la entidad) y `target_id` su `id`; ninguna referencia polimórfica apunta a una tabla del catálogo global.

---

### H-3 · `ScoreSnapshot` sin propietario ni "as-of": Riesgo (10) lo necesita antes de que exista Objectives & KPIs (18) y Reporting (19) puede recomputarlo

**Pares:** `risk-engine-core` (10) vs `scoring-kpi-dashboard` (18) sobre riesgo residual; (18) vs `reporting-essentials` (19) sobre `latest` vs `as-of`; `management-review-core` (17) vs (18) sobre KPIs que el 17 necesita y el 18 crea después.

**Cómo ambos cumplen.** AD-16: "toda puntuación es un `ScoreSnapshot {…}`" y binds Objectives & KPIs y Reporting; la tabla `score_snapshots` es de Objectives & KPIs (DATA_ARCHITECTURE). AD-3 impide que Risk la escriba. El 10 (que precede al 18 y tiene el residual "dentro de `ScoreSnapshot`" según Deferred) guarda el residual como `residual_risks.score jsonb` con la forma de `ScoreSnapshot` — cumple AD-16 (es "un ScoreSnapshot") y AD-3. El 18 luego calcula "KPI de riesgo" leyendo `RiskReadPort.getExposureDataset` y produce **otro** `ScoreSnapshot` en `score_snapshots` con otra fórmula. El 19 exporta un informe con `cutoff_date`, `soa_version_id` y `standard_version_id` (AD-16) pero `ScoreReadPort.latest(scoreType, scope)` no acepta fecha: o muestra el último valor (incoherente con la versión de SoA que pinna) o recomputa desde los datasets crudos (AD-16 lo permite: "datasets nombrados expuestos por puertos de lectura"). El 17 (depende de 10, 15, 16 — **no de 18**) debe agregar "desempeño y KPIs" (entrada obligatoria de 9.3) sin `KPIReadPort` → define su propio dataset.

**Choque.** Dos fórmulas de riesgo, dos fuentes de KPI, un informe cuyo número no es el que vio el usuario en el dashboard, y `PR-DR-010` ("ninguna puntuación sin desglose") comprobado por una prueba de UI que no cubre el PDF.

**Cierre propuesto — Rule de AD-16 endurecida:**
> `ScoreSnapshot` es un **tipo de valor del kernel** (`packages/kernel/score-snapshot.ts`) y la tabla `score_snapshots` es append-only, propiedad de Objectives & KPIs, con clave `(tenant_id, score_type, scope_type, scope_id, computed_at)`. El **único** productor de `score_snapshots` es el job `kpi-recalculation`; los demás contextos exponen **datasets** (`*ReadPort.get*Dataset`), nunca puntuaciones agregadas. Excepción explícita: el riesgo residual **por riesgo** es dato de dominio de Risk (`residual_risks.value, scale_ref, formula_ref`) y no un `ScoreSnapshot`; la exposición agregada de riesgo sí es un `ScoreSnapshot` del 18. `ScoreReadPort.asOf(scoreType, scope, at)` es obligatorio; `ReportRun` guarda `score_snapshot_ids[]` y Reporting **nunca** computa una puntuación (prueba de arquitectura: ninguna `ReportDefinition` contiene `numerator`/`denominator`). Corregir dependencia del ROADMAP: 17 depende de 18 (o el 18 se adelanta al 17).

---

### H-4 · Arranque de identidad: quién es `created_by` del primer tenant, qué tabla es `users`, y si `session`/`account` de better-auth son "tablas de negocio"

**Pares:** `organization-tenancy` (2) vs `identity-and-rbac` (3); (2)/(3) vs `domain-events-outbox-jobs` (5) sobre principales técnicos.

**Cómo ambos cumplen.** AD-11 exige `created_by` en toda entidad importante y AD-22 `uuid` v7. El 2 crea `tenants` y necesita un actor: crea una tabla `users` mínima `(id, email, tenant_id)` y un usuario semilla, o usa `created_by = '00000000-…'`. El 3 adopta better-auth (Stack) cuyo esquema por defecto es `user`, `session`, `account`, `verification` (singular, sin `tenant_id`, usuario global multi-tenant). Cumple AD-2 si decide que `session`/`account` no son "tablas de negocio"; cumple AD-22 si decide que las tablas de una librería quedan fuera de la convención. `Membership` (cambio 2, Identity & Org) y `user_roles` con `scope = business_unit` (cambio 3) modelan la misma relación usuario ↔ unidad.

**Choques.** Dos tablas de usuario (`users` del 2 y `user` de better-auth) o una migración destructiva en el 3; un usuario global sin `tenant_id` contradice `users con tenant_id` (DATA_ARCHITECTURE) y hace que `UserReadPort.listUsersWithPermission(tenantId, …)` (base de la excepción de separación de funciones, AD-10) devuelva usuarios de otro tenant; `created_by` de consumidores y jobs (cambio 5) queda a criterio de cada contexto (`event.actor_id` del usuario original, un uuid fijo, o `NULL` prohibido por AD-11) y AD-4 exige `authorize(permission)` también en consumidores: unos ejecutan con el actor original (escalada: el usuario que activó un control "crea" solicitudes de evidencia sin permiso `evidence.request`), otros con un principal `system` sin catálogo de permisos.

**Cierre propuesto — Rule de AD-8/AD-11 endurecida + fila de Consistency Conventions:**
> `packages/kernel/principals.ts` define los **principales técnicos** con uuid fijos: `SYSTEM`, `SEED_LOADER`, `AI_ASSISTANT`, `CONNECTOR` (más `connector:<integration_id>` derivado por uuid v5). `created_by/updated_by` referencian `users.id` **o** uno de esos ids; el `Authorizer` resuelve para los técnicos un rol interno con permisos declarados en el mismo catálogo (`SYSTEM` = todos los `*.create|update` de reacción, nunca `*.approve`). Un consumidor de eventos ejecuta **siempre** como `SYSTEM` y conserva `event.actor_id` como `on_behalf_of` en el envelope de los eventos que emite (nuevo campo opcional, versión 2 del envelope) y en `audit_entries`. `users` es tabla de negocio de Identity & Organization con `tenant_id NOT NULL` (un usuario por tenant; federación por `external_subject`); las tablas del adaptador de auth viven en esquema `auth` separado, mapeadas por el adaptador a `users`, y **sí** llevan `tenant_id` y RLS. `Membership` = pertenencia a unidad; `user_roles.scope_id` referencia `memberships.id`, nunca una unidad directamente.

---

### H-5 · `CandidateAISystem`, `EvidenceProvenance` y el vínculo `AIProvider ↔ Supplier ↔ Integration` tienen propietario declarado pero constructor en otro cambio

**Pares:** `ai-inventory-core` (9) vs `connector-sdk-mock` (21); `evidence-library` (13) vs (21) sobre `EvidenceProvenance`.

**Cómo ambos cumplen.** AD-18 fija el pipeline "… → `Evidence | CandidateAISystem | IntegrationFinding`" y binds Integrations y AI Portfolio. El 9 (AI Portfolio) construye `AISystem`, `AIProvider`, `Supplier` y deja `CandidateAISystem` para F2 (DOMAIN_ARCHITECTURE 3.9). El 21, obedeciendo AD-18 y con el mock obligado a "producir" candidatos etiquetados, no puede escribir `candidate_ai_systems` (AD-3) y el consumidor de `CONNECTOR_SYNC_COMPLETED` en AI Portfolio no existe: guarda los candidatos como `integration_findings (kind = 'candidate_ai_system', payload jsonb)`. Cumple todo. El 13 modela `evidence_provenance` (DOMAIN_ARCHITECTURE: "modelo desde el MVP") con sus columnas; el 21 tiene en su objetivo "`EvidenceProvenance` en modelo" y añade su versión (`connector_id`, `external_id`, `sync_id`, `raw_hash`). Para vincular Azure OpenAI como `AIProvider` (9), `Supplier` (9) e `Integration` (21), cada contexto añade una FK en **su** tabla (`ai_providers.integration_id` vs `integrations.ai_provider_id`).

**Choques.** En F2 los candidatos hay que migrarlos de `integration_findings` a `candidate_ai_systems` (dos formas); `EvidenceProvenance` tiene dos definiciones; el vínculo proveedor-integración es bidireccional sin dueño y puede divergir; PR-DR-012 ("descubrimiento nunca marca cumplimiento") no tiene tabla que probar hasta F2.

**Cierre propuesto — Rule de AD-18 endurecida:**
> El cambio 9 crea `candidate_ai_systems` y el consumidor `ai-portfolio.on-connector-sync` (aunque el único productor del MVP sea el mock); el resultado normalizado de un sync viaja **solo** en el payload de `CONNECTOR_SYNC_COMPLETED` (`records[] { kind ∈ {evidence, candidate_ai_system, integration_finding}, external_id, normalized jsonb, provenance }`) y lo materializa el contexto propietario. `EvidenceProvenance` es propiedad y esquema del cambio 13 (`evidence_provenance (evidence_id, source ∈ {manual, connector, import}, integration_id, external_id, sync_id, collected_at, raw_sha256)`); el 21 solo lo **rellena**. El vínculo con la organización externa es unidireccional: `integrations.ai_provider_id` y `integrations.supplier_id` (nullable) apuntan a AI Portfolio; AI Portfolio no conoce integraciones.

---

### H-6 · Evidencia enlazada a criterios de auditoría: `AuditEvidence` (15) duplica `EvidenceLink` (13) y el auditor externo no tiene camino de permiso hacia el archivo

**Pares:** `evidence-library` (13) vs `internal-audit-core` (15).

**Cómo ambos cumplen.** Evidence posee `evidence_links (evidence_id, target_type, target_id)`; Audit posee `audit_evidence` que "vincula `Evidence` con criterio" (DOMAIN_MAP §4). El 15, sin poder llamar a `linkEvidence` (AD-3, ver C-3), escribe `audit_evidence (audit_id, criterion_id, evidence_id, evidence_version, auditor_note)`. Cumple todo. El 13 nunca añadió `AuditCriterion` a su enum de `target_type` porque el 15 no existía.

**Choques.** Dos tablas de enlace; `EvidenceReadPort.getCoverage` ignora las evidencias usadas en auditoría; `EVIDENCE_EXPIRED`/`SUPERSEDED` no llegan a `audit_evidence` (el 15 no lo consume, la evidencia del informe emitido cambia bajo sus pies sin `evidence_version` pinneada si el 15 no la guardó). Permiso: la descarga por URL firmada exige "permiso verificado" (AD-13) con `evidence.read`; `ExternalAuditorAccess` (15) da "Read Only con caducidad" a un principal que no es usuario del tenant: el 15 crea un `users` temporal con rol `External Auditor` (Identity & Org) o un token propio → dos mecanismos de acceso, uno fuera del `Authorizer`.

**Cierre propuesto — Rule de AD-13 y fila de conventions:**
> Existe **una** relación "evidencia ↔ objetivo": `evidence_links` de Evidence, con `target_type ∈ AggregateType` (incluye `AuditCriterion`, `ControlImplementation`, `RequirementImplementation`, `Risk`, `LifecycleGate`) y `evidence_version_id NOT NULL` (pinnea versión). Audit crea enlaces por el command port `EvidenceLinkPort.link` (se añade a la lista cerrada de AD-25) y `audit_evidence` desaparece o queda como extensión 1:1 de `evidence_links` con la nota del auditor. Acceso externo: `ExternalAuditorAccess` **siempre** materializa un `User` con `status = External` y rol `External Auditor` con `expires_at`; no existen tokens de acceso fuera del `Authorizer`; la URL firmada se emite con `evidence.read` evaluado sobre ese usuario y alcance `audit:<audit_id>`.

---

## Hallazgos medium

### M-1 · Tareas duplicadas al activar un control (12 / 13 / 14)
DOMAIN_MAP §5 lista a Evidence **y** a Workflow como consumidores de `CONTROL_ACTIVATED`; el criterio de salida del 12 dice "26 controles activan tareas y solicitudes". El 13 crea `EvidenceRequest` y, para que alguien la atienda, una `Task` (vía `TaskPort`); el 14 crea una `Task` "implementar control" por consumidor. Ambos cumplen; el responsable recibe dos o tres tareas por control. **Cierre:** en la fila Consistency Conventions "Tareas": `Task` solo se crea (a) por consumidor de Workflow ante eventos listados con la columna `task_template` en el catálogo de eventos, o (b) por acción explícita de usuario; `EvidenceRequest` **no** genera `Task`, es ella misma el ítem de trabajo y aparece en la bandeja unificada por `WorkItemReadPort` de cada contexto.

### M-2 · Riesgo ↔ Impacto con doble propietario del vínculo (10 vs 11)
`impact_assessments.risk_ids[]` (Impact) y la reacción de Risk a `IMPACT_ASSESSMENT_COMPLETED` guardando `risks.last_impact_assessment_id / last_impact_reviewed_at` (Risk) son dos fuentes para PR-DR-028 ("impacto posterior sin revisar"). Ambos cumplen AD-3. **Cierre:** el vínculo es unidireccional y lo posee Impact (`impact_assessment_risks (impact_assessment_id, risk_id)`); Risk consulta `ImpactReadPort.getLatestApproved(aiSystemId).approved_at` para PR-DR-028 y guarda solo `risks.impact_reviewed_at`.

### M-3 · `management-review-core` (17): entradas vivas o instantánea, y quién convierte decisiones en tareas
`aggregateInputs` puede guardar snapshot (`management_review_inputs.snapshot jsonb`) o solo referencias; `exportMinutes` (19) puede releer puertos en vivo → acta distinta de lo revisado. `MANAGEMENT_REVIEW_APPROVED` "crea tareas": por consumidor de Workflow o por `TaskPort.create` en `approveReview` (ver C-3). **Cierre:** las 14 entradas se guardan como snapshot inmutable con `source_record_refs[]` y `score_snapshot_ids[]` al aprobar (patrón `*_versions` de AD-11: añadir `management_review_versions` a la lista); las tareas nacen solo en Workflow a partir de `payload.decisions[]`.

### M-4 · `SearchDocument` con dos propietarios declarados
DOMAIN_MAP §3.20 lo asigna a Administration; la espina (Capability Map, fila 22 y DOMAIN_ARCHITECTURE 3.5) a Reporting `[ASSUMPTION]`. El cambio 22 puede elegir cualquiera cumpliendo AD-3. **Cierre:** fila en Capability Map y nota en AD-3: "`search_documents`: Reporting; DOMAIN_MAP §3.20 queda superseded".

### M-5 · `RequirementImplementation` y `TenantStandardAdoption` en el cambio 6 sin paquete AIMS
El 6 crea `RequirementImplementation` en `domain-aims`, que nace formalmente en el 7; `tenant_standard_adoptions` (DATA_ARCHITECTURE) no figura en DOMAIN_MAP §4 (¿Standards, con `tenant_id`, violando "catálogo sin tenant_id" de AD-7? ¿AIMS?). **Cierre:** AD-7 añade "`TenantStandardAdoption` es estado de tenant propiedad de AIMS; `domain-aims` existe desde el cambio 6 con `TenantStandardAdoption` y `RequirementImplementation`".

### M-6 · Enumeración `AggregateType`/`subject_type`/`target_type`/`source_type` sin dueño
AD-12 fija que los enums viven en kernel, pero un enum con todos los agregados de 21 contextos obliga a que cada cambio lo edite (contención, y una entidad nueva sin registrar compila y rompe la referencia polimórfica). **Cierre:** fila Consistency Conventions "Referencias polimórficas": enum kernel `AggregateType` generado desde los `index.ts` (`export const aggregates = [...] as const`) por script en `packages/kernel`, con prueba de arquitectura que falla si una tabla declara `*_type` con valor fuera del enum.

### M-7 · Anexo C / `RiskSource` y `AuditQuestion`: catálogo global consultado por tablas de tenant sin `standard_version_id`
AD-7 exige `catalog_id + standard_version_id` en el estado del tenant; DOMAIN_ARCHITECTURE 3.10 solo dice "`RiskSource` referencia la semilla". El 10 puede referenciar solo `catalog_id`; el 6 puede publicar una `StandardVersion` nueva con ids nuevos de fuentes → riesgos huérfanos en la migración. **Cierre:** Rule de AD-7: "toda referencia de tenant al catálogo es el par `(catalog_id, standard_version_id)` como columnas explícitas, incluidas `risk_sources`, `audit_criteria` y `impact` (dimensiones semilla)".

---

## Hallazgos low

### L-1 · Tres significados de `version`
`version` (concurrencia optimista, AD-11), `version_number` (`*_versions`), `version` del envelope (AD-5) y `AIModelVersion`. Riesgo de confusión en DTOs (`If-Match` con el número equivocado). **Cierre:** convención de nombres: `row_version` para optimista, `version_number` para snapshots aprobados, `schema_version` en el envelope.

### L-2 · `idempotency_keys` la escribe L3
AD-15 exige `Idempotency-Key` en POST y AD-4 dice que ningún route handler llama a Prisma. **Cierre:** `IdempotencyPort` en `packages/db` usado por un middleware de `apps/web`, con `(tenant_id, key, request_hash, response)`.

### L-3 · `tenants.is_demo` (23) vs `tenant_settings.demo`
El 23 puede etiquetar el tenant demo de dos formas. **Cierre:** `tenants.is_demo boolean NOT NULL DEFAULT false` desde el cambio 2, expuesto por `TenantReadPort.getTenant().is_demo`.

---

## Tabla resumen de pares y agujeros

| # | Par de cambios | Tipo de choque | Sev. | Cierre |
|---|---|---|---|---|
| 1 | 15 `internal-audit-core` vs 16 `capa-core` | doble propietario + campo de estado duplicado (`Finding`) | critical | AD-24 |
| 2 | 11 `impact-assessment-core` vs 16 `capa-core` | idem sobre `ImpactAssessmentFinding` | critical | AD-24 |
| 3 | 4 `immutable-audit-trail` vs 5 `domain-events-outbox-jobs` | orden de eventos + ruta de mutación doble a `audit_entries` | critical | AD-6 endurecida, AD-5 |
| 4 | 2/3 vs 5 | forma compartida (`outbox_events`) definida tres veces | critical | AD-5 (outbox al cambio 1) |
| 5 | 5 relay vs 2 RLS | permiso ambiguo (relay multi-tenant sin BYPASSRLS) | critical | AD-5 rol `app_worker` |
| 6 | 7/8/12 vs 14 | ruta de mutación en conflicto (evento vs puerto) | critical | AD-25 |
| 7 | 15 vs 16 `FindingPort` | transacción propia vs compartida | critical | AD-25 |
| 8 | 7/8/10/11/12/13 vs 14 | forma compartida (`Approval` mínimo), SoD sin `created_by` | high | AD-10 endurecida |
| 9 | 13 `EvidenceReview`, 10 `RiskAcceptance`, 16 `EffectivenessVerification` vs AD-10 | segunda forma de aprobar | high | AD-10 `approval_id` |
| 10 | 12 vs 13 | referencia polimórfica (`control_id` vs `control_implementation_id`) | high | AD-9 endurecida |
| 11 | 12 vs 18 | estado duplicado (definición de "activo") | high | AD-9 endurecida |
| 12 | 10 vs 18 | doble propietario (`ScoreSnapshot` residual) | high | AD-16 endurecida |
| 13 | 18 vs 19 | `latest` vs `as-of`; Reporting recomputa | high | AD-16 `asOf`, prueba arq. |
| 14 | 17 vs 18 | orden de dependencias (KPIs antes de existir) | high | ROADMAP + AD-16 |
| 15 | 2 vs 3 | forma compartida (`users` vs `user`), `tenant_id` en auth | high | AD-8/AD-11 endurecidas |
| 16 | consumidores (5+) vs AD-4 `authorize` | permiso ambiguo (principal técnico) | high | `kernel/principals.ts` |
| 17 | 9 vs 21 | entidad sin constructor (`CandidateAISystem`), FK bidireccional | high | AD-18 endurecida |
| 18 | 13 vs 21 | doble definición (`EvidenceProvenance`) | high | AD-18 endurecida |
| 19 | 13 vs 15 | dos tablas de enlace + acceso externo fuera del Authorizer | high | AD-13 + AD-25 |
| 20 | 12/13/14 | tareas duplicadas | medium | conv. "Tareas" |
| 21 | 10 vs 11 | doble propietario del vínculo riesgo-impacto | medium | conv. + `ImpactReadPort` |

---

## Nota sobre los compañeros

Siete de los diecinueve hallazgos (C-1, C-2 parcial, H-1 parcial, H-2 parcial, M-2, M-4, M-5) están **resueltos o casi** en `DATA_ARCHITECTURE.md`, `EVENT_ARCHITECTURE.md` o `DOMAIN_ARCHITECTURE.md`, marcados allí como `[ASSUMPTION]`. La corrección más barata es **promover** esas decisiones a la espina (AD o fila de conventions) y dejar en el compañero solo la elaboración, porque un cambio OpenSpec que "obedece la espina" no tiene por qué haber leído `EVENT_ARCHITECTURE.md` §8. Dos casos (C-3 y H-1) requieren además **retirar** texto de la espina (la ruta sincrónica de AD-10 y la frase "puertos de lectura" en AD-3) porque los compañeros y la espina se contradicen entre sí.

Ningún archivo distinto de esta revisión ha sido modificado.
