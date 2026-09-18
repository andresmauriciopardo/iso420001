---
titulo: "ADR-015: Versionado de objetos, concurrencia optimista y borrado gobernado"
fecha: 2026-09-17
estado: "propuesto, requiere aprobación humana"
nivel: 3
decisiones_spine: [AD-11, AD-21, AD-4, AD-10, AD-22]
inputs:
  - _bmad-output/planning-artifacts/architecture/architecture-ISO42001-2026-09-17/ARCHITECTURE-SPINE.md
  - CLAUDE.md
  - _bmad-output/planning-artifacts/prds/prd-ISO42001-2026-09-17/prd.md (§17 D-7)
  - docs/product/ROADMAP.md (§3, cambios 2, 5, 13)
sustituye: []
---
# ADR-015: Versionado de objetos, concurrencia optimista y borrado gobernado

## Contexto

CLAUDE.md prohíbe "borrar registros de cumplimiento en silencio" y exige aprobación humana para borrar o alterar evidencia y para migraciones irreversibles. FR-009 pide historial de cambios de toda entidad; PR-DR-017 y AD-6 exigen que el registro de auditoría responda quién cambió qué, cuándo y cuál era el valor anterior. D-7 fija una retención provisional de 7 años y P2 §42 nombra el `legal hold`.

## Decisión

1. **Campos base** en toda entidad importante: `id, tenant_id, created_at, created_by, updated_at, updated_by, status, version, deleted_at` (AD-11), más `deleted_by` y `deletion_reason` `[ASSUMPTION]`.
2. **Concurrencia optimista.** `version` se incrementa en cada mutación dentro de la transacción; la API exige `If-Match: "<version>"` en PATCH/PUT (ADR-013) y devuelve `CONFLICT_VERSION → 412` si no coincide. Sin `If-Match` no hay mutación (`428`).
3. **Dos mecanismos de historial, según la naturaleza del objeto.**
   - **Documentos aprobados** (`AIMSScope`, `Policy`, `Document`, `StatementOfApplicability`, `ImpactAssessment`, `RiskMethodology`, informe de `Audit`): tabla `<tabla>_versions` inmutable. Cada `APPROVAL_GRANTED` (AD-10) inserta `{ version_number, snapshot jsonb, approved_by, approved_at, sha256, previous_version_id }`; `sha256` sobre JSON canónico RFC 8785 (AD-22). Triggers rechazan `UPDATE` y `DELETE`; el rol de aplicación no tiene esos privilegios.
   - **Resto de entidades**: tabla única `object_versions { id, tenant_id, entity_type, entity_id, version, snapshot jsonb, changed_by, changed_at, correlation_id, event_id }`, escrita por un paso genérico de la tubería `authorize → validate → transaction { mutate; snapshot; outbox }` (AD-4). Cubre FR-009 y alimenta `before/after` de `ENTITY_MUTATED`.
4. **Borrado lógico.** Borrar es `deleted_at = now()` más evento (`<ENTITY>_DELETED` o `ENTITY_MUTATED`). Los repositorios filtran `deleted_at IS NULL` por defecto; `includeDeleted` solo en consultas de administración y auditoría. Los índices únicos son parciales (`WHERE deleted_at IS NULL`).
5. **Retención en Administration.** Cada entidad declara `retentionClass` estático en su paquete; `RetentionPolicy` (por clase y tenant, 7 años por defecto, D-7) y `LegalHold { scope: tenant | ai_system | audit | object, reason, placed_by }` viven en `domain-administration`. El job `retention-sweeper` marca `RETENTION_EXPIRED` y pasa a `status = Archived`; en el MVP nunca borra.
6. **Purga física, solo F2.** Command `purgeArchivedObjects` que exige ausencia de `LegalHold`, un `Approval` (AD-10) y emite `PURGE_EXECUTED`; borra filas y llama a `ObjectStorage.remove` (ADR-008). La evidencia queda excluida de la purga en el MVP.

## Alternativas consideradas

| Alternativa | Por qué se descarta |
| --- | --- |
| Event sourcing completo | Reconstruir estado desde eventos complica RLS, informes y consultas; el outbox ya ofrece la traza de eventos sin ese coste. |
| Tablas temporales / versionado de sistema de PostgreSQL | Extensión no incluida en el motor, opaca para Prisma y con interacción incierta con RLS. |
| Borrado físico con solo registro de auditoría | Incumple CLAUDE.md y deja huérfanas las referencias de trazabilidad. |
| Un solo mecanismo (snapshots para todo) | Los documentos aprobados necesitan número de versión semántico, hash y aprobador; un snapshot por mutación no es una "versión aprobada". |
| Un solo mecanismo (tablas `_versions` para todo) | Multiplica tablas y migraciones por 21 contextos sin beneficio para entidades sin ciclo de aprobación. |

## Consecuencias

**Positivas.** Historial uniforme y consultable por `entity_type/entity_id`; versiones aprobadas inmutables y verificables por hash; ninguna ruta de código borra registros de cumplimiento; retención y legal hold en un único propietario.

**Negativas.** Crecimiento de `object_versions`; se mitiga con índice `(tenant_id, entity_type, entity_id, version)` y la misma clase de retención que la entidad. Fricción de `If-Match` en clientes.

**Cambios OpenSpec.** `organization-tenancy` (campos base, borrado lógico, primeras filas de `object_versions`); `domain-events-outbox-jobs` (paso genérico de snapshot, `ENTITY_MUTATED`, "historial estándar"); `aims-context-parties-scope` (`aims_scope_versions`); `aims-policy-objectives-roles` (`policy_versions`); `risk-engine-core` (`risk_methodology_versions`); `impact-assessment-core`; `soa-control-activation` (`statement_of_applicability_versions`); `internal-audit-core` (informe); `evidence-library` (retención sin borrado físico); `immutable-audit-trail` (consumo de `before/after`); F2: `governed-purge` `[ASSUMPTION nombre]`.

## Verificación

- Integración: `UPDATE`/`DELETE` sobre cualquier `*_versions` falla con el rol de aplicación; `If-Match` obsoleto → 412; ausente → 428.
- Prueba genérica sobre el registro de commands: toda mutación deja exactamente una fila en `object_versions` o en una `*_versions`, con `correlation_id` igual al del evento.
- `retention-sweeper`: marca `Archived` y el recuento de filas no disminuye.
- Prueba de arquitectura: ningún repositorio de dominio contiene `deleteMany`/`DELETE FROM` salvo `processed_events` e `idempotency_keys` `[ASSUMPTION]`.

## Estado y aprobación

Propuesto. Requiere aprobación humana porque toca borrado y alteración de evidencia, define el momento y las condiciones de la purga física (F2), fija la retención por defecto de 7 años (D-7 provisional) y la semántica del `LegalHold`, y porque las tablas inmutables con triggers son migraciones difícilmente reversibles.
