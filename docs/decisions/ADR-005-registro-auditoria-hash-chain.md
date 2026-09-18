---
titulo: "ADR-005: Registro de auditoría append-only con cadena de hashes por tenant y Administration como único escritor"
fecha: 2026-09-17
estado: "propuesto, requiere aprobación humana"
nivel: 3
decisiones_spine: [AD-6, AD-4, AD-22]
inputs: [CLAUDE.md, docs/product/PROMPT_REVIEW_AND_ADJUSTMENTS.md (A8), _bmad-output/planning-artifacts/prds/prd-ISO42001-2026-09-17/prd.md (FR-007, NFR-017, PR-DR-017), _bmad-output/planning-artifacts/prds/prd-ISO42001-2026-09-17/addendum.md (D.6), docs/product/DOMAIN_MAP.md (§7.5), docs/product/MASTER_ISO42001_PLATFORM_REQUIREMENTS.md (§41, §63)]
sustituye: []
---

# ADR-005: Registro de auditoría append-only con cadena de hashes por tenant y Administration como único escritor

## Contexto

FR-007 exige registrar toda acción de gobernanza en un registro "inviolable"; NFR-017 que toda operación sea atribuible y exportable; PR-DR-017 que toda decisión de cumplimiento genere un registro inmutable con actor, momento, motivo, valor anterior, nuevo y aprobador. P2 §41 enumera los campos (actor, timestamp, action, object, previous/new state, reason, session metadata, correlation id) y §63 exige auditabilidad de la propia aplicación. A8 interpreta "immutable" como tabla append-only con cadena de hashes por tenant. `DOMAIN_MAP.md` §7.5 pide confirmar que Administration sea el único escritor y consuma el outbox completo; el PRD §8.6 lo da por adoptado.

## Decisión

- Tabla `audit_entries` **append-only**: triggers `BEFORE UPDATE OR DELETE` que lanzan excepción; `REVOKE UPDATE, DELETE` a los roles de aplicación; RLS por tenant como cualquier tabla de negocio.
- **Cadena de hashes por tenant**: `hash = SHA-256(previous_hash || canonical_json(entry_sin_hash))` con JSON canónico RFC 8785; `sequence` monótona sin huecos por tenant; primer eslabón con `previous_hash = SHA-256(tenant_id)`; cabeza de cadena en `audit_chain_heads (tenant_id, last_sequence, last_hash)` bloqueada con `FOR UPDATE` durante la escritura.
- **Único escritor**: el consumidor `audit-trail-writer` del contexto Administration, que procesa **todos** los eventos del outbox (catálogo y `ENTITY_MUTATED`), serializado por tenant con `singletonKey = tenant_id` en pg-boss (ADR-004). Ningún command escribe `audit_entries` directamente.
- **Contenido**: `event_id` (idempotencia), `event_type`, `actor_id`/`actor_type` (`user | system | ai-assistant | connector | api_key`), `occurred_at`, `recorded_at`, `object_type`/`object_id`, `action`, `previous_state`, `new_state` (diff de la entidad, sin binarios ni secretos), `reason`, `correlation_id`, `session_meta` (ip, user agent, api key id). Las aprobaciones llegan como `APPROVAL_GRANTED/REJECTED` con `decided_by`, cumpliendo "aprobador".
- **Verificación y exportación**: `verifyChain(tenantId, fromSeq, toSeq)` por API y UI, que recalcula y devuelve el primer eslabón roto; exportación JSON Lines con hashes para custodia externa; procedimiento ante rotura (alerta, congelar escritura del tenant, investigación) documentado en `SECURITY_ARCHITECTURE.md`.
- **Ancla externa** (publicar diariamente el último hash en un medio externo de solo escritura) diferida a F2 `[ASSUMPTION]`.
- Los objetos de negocio conservan además su propio historial (`*_versions`, `object_versions`, ADR-015); el audit trail es la crónica de decisiones, no el almacén de versiones.

## Alternativas consideradas

| Alternativa | Por qué se descarta |
| --- | --- |
| Escritura síncrona del `AuditEntry` en cada command | Múltiples escritores rompen la serialización de la cadena bajo concurrencia; acopla cada contexto al formato de auditoría; el outbox ya garantiza que nada se pierde |
| Solo triggers de BD que copian filas a una tabla de historial | Captura cambios, no decisiones (motivo, aprobador, correlación); sin encadenamiento |
| Firma digital por entrada (clave privada del servidor) | Mayor coste y gestión de claves sin ganancia para el modelo de amenaza del MVP (integridad frente a modificación interna); la cadena detecta cualquier alteración; firma/ancla externa se añaden en F2 |
| Ledger externo (QLDB, blockchain, servicio SaaS) | Dependencia externa y de proveedor pendiente; el requisito es *tamper-evident*, no *tamper-proof* distribuido |
| Cadena global única (no por tenant) | Exportar y verificar la cadena de un tenant exigiría revelar hashes de otros; la cadena por tenant es exportable de forma aislada |

## Consecuencias

- Positivas: toda mutación (no solo las "decisiones") queda registrada porque todo command escribe outbox (`AD-4`); verificación criptográfica exportable por tenant; independencia del formato de cada contexto; una sola implementación probada.
- Negativas: consistencia eventual (la entrada aparece milisegundos o segundos después de la acción; la UI de "historial" lee `object_versions` para inmediatez y el audit trail para la crónica verificable); throughput de escritura limitado por la serialización por tenant (suficiente para gobernanza; medido en `mvp-hardening`).
- Riesgos: rotura de cadena por bug del escritor → detectada por la verificación programada diaria (`AUDIT_CHAIN_VERIFIED/BROKEN`); pérdida del orden si dos instancias procesan el mismo tenant → impedido por `singletonKey` y `FOR UPDATE` en la cabeza; crecimiento de la tabla → particionado por tenant y año evaluado en F2, nunca borrado.
- Cambios OpenSpec que lo implementan: `immutable-audit-trail` (tabla, triggers, escritor, verificación, exportación), `domain-events-outbox-jobs` (outbox del que se alimenta; el cambio 4 usa un outbox mínimo que el 5 generaliza `[ASSUMPTION]`), `mvp-hardening` (verificación programada, rendimiento).

## Verificación

- Prueba de integración: `UPDATE`/`DELETE` sobre `audit_entries` fallan con los roles de aplicación; `INSERT` fuera del escritor falla por permisos.
- Prueba unitaria `PR-DR-017`: cada evento del catálogo produce una entrada con actor, momento, motivo, antes, después y aprobador cuando aplica.
- Prueba de cadena: alterar un byte de una entrada hace que `verifyChain` señale ese `sequence`.
- Prueba de concurrencia: 100 eventos concurrentes de un tenant producen `sequence` 1..100 sin huecos y hashes válidos.
- Criterio de salida del MVP §16.2.3: cadena verificable por tenant en UI y API.

## Estado y aprobación

Propuesto. Es arquitectura de seguridad y fija cómo se interpreta "inmutable"; `CLAUDE.md` exige aprobación humana. Decisiones explícitas pendientes: aceptar la consistencia eventual del registro, diferir el ancla externa a F2, y el procedimiento operativo ante rotura de cadena. Punto de aprobación: antes de `immutable-audit-trail`.
