---
titulo: "ADR-004: Colas y trabajos en segundo plano con pg-boss tras un puerto JobScheduler"
fecha: 2026-09-17
estado: "propuesto, requiere aprobación humana"
nivel: 3
decisiones_spine: [AD-5, AD-19, AD-20]
inputs: [CLAUDE.md, docs/product/PROMPT_REVIEW_AND_ADJUSTMENTS.md (A8), _bmad-output/planning-artifacts/prds/prd-ISO42001-2026-09-17/prd.md (FR-012, NFR-014), docs/product/MASTER_ISO42001_PLATFORM_REQUIREMENTS.md (§44, §45)]
sustituye: []
---

# ADR-004: Colas y trabajos en segundo plano con pg-boss tras un puerto `JobScheduler`

## Contexto

P2 §44 exige trabajo en segundo plano reintentable, idempotente, observable y auditable para sincronización de conectores, generación de informes y documentos, notificaciones, cálculo de KPI, reevaluación de riesgos y detección de evidencia caducada; FR-012 lo recoge. El outbox (ADR-003) necesita un relay y colas por tipo de evento; el registro de auditoría (ADR-005) necesita procesamiento serializado por tenant. A8 fija pg-boss para evitar Redis como dependencia temprana, con una interfaz abstracta que permita migrar a BullMQ/Redis si hiciera falta. pg-boss 12.33 verificado el 2026-09-17 (MIT, mantenimiento muy activo).

## Decisión

- pg-boss 12 sobre la misma instancia PostgreSQL (esquema `pgboss`), usado exclusivamente a través del puerto `JobScheduler` de `packages/jobs`:

```ts
interface JobScheduler {
  publish(queue: string, payload: unknown, opts?: { singletonKey?: string; startAfter?: Date; retryLimit?: number; expireIn?: string }): Promise<string>;
  schedule(name: string, cron: string, payload?: unknown): Promise<void>;
  work<T>(queue: string, handler: (job: Job<T>) => Promise<void>, opts?: { teamSize?: number; batchSize?: number }): Promise<void>;
}
```

- Convenciones de cola: `events.<EVENT_TYPE>` para eventos de dominio publicados por el relay; `jobs.<job-name>` para trabajos programados o bajo demanda; `dead_letter` para agotados.
- Reintentos exponenciales (límite 5 por defecto, configurable por cola), `expireIn` por tipo de trabajo, `singletonKey = tenant_id` para consumidores que exigen orden por tenant (`audit-trail-writer`, `kpi-recalculation`).
- Todo handler recibe un `TenantContext` reconstruido desde el payload (`tenant_id`, `actor_id = system`, `correlation_id` heredado) y ejecuta bajo `withTenantTransaction` (ADR-002).
- Programación (cron) declarada en código en `apps/worker/src/schedules.ts` y registrada al arrancar; nunca desde la UI en el MVP.
- Observabilidad: métricas de pg-boss (pendientes, activos, fallidos, latencia por cola) exportadas por OpenTelemetry; panel en Administration; cada job abre un span con `correlation_id` (`AD-19`).
- El worker es la misma imagen Docker con `MODE=worker` (`AD-20`); `web` no ejecuta handlers, solo publica.

## Alternativas consideradas

| Alternativa | Por qué se descarta |
| --- | --- |
| BullMQ + Redis | Añade Redis como dependencia operativa, de copia y de seguridad desde el día 1; sin necesidad de throughput que lo justifique; queda como destino de migración detrás del puerto |
| Graphile Worker | Sólido, pero sin colas nombradas con `singletonKey` ni cron integrado al mismo nivel; pg-boss cubre más superficie |
| Colas gestionadas (SQS, Pub/Sub, Service Bus) | Acopla al proveedor de nube (pendiente de decisión, PRD Q6) y rompe el entorno local sin emuladores |
| Cron del sistema / Next.js `after()` | Sin reintentos, sin idempotencia, sin observabilidad; incumple P2 §44 |
| Temporal / Inngest | Potentes pero introducen un servidor adicional o un SaaS externo; desproporcionado para el MVP |

## Consecuencias

- Positivas: una sola dependencia de infraestructura para datos, outbox, colas y auditoría; transacciones compartidas cuando conviene; entorno local trivial; migración posible sustituyendo el adaptador.
- Negativas: la carga de colas compite con la de negocio en la misma base (mitigación: pool separado para el worker, `teamSize` acotado, opción de instancia PostgreSQL dedicada para `pgboss` en F2); pg-boss es polling (latencia de segundos, aceptable para gobernanza).
- Riesgos: crecimiento de tablas `pgboss` sin mantenimiento → retención de trabajos completados configurada (7 días `[ASSUMPTION]`) y monitorizada; un handler no idempotente duplica efectos → `processed_events` obligatorio en consumidores de eventos.
- Cambios OpenSpec que lo implementan: `domain-events-outbox-jobs` (puerto, adaptador, relay, `dead_letter`, métricas), `evidence-library` (`evidence-expiry-sweeper`), `risk-engine-core` (`risk-acceptance-expiry`), `tasks-approvals-notifications` (`task-overdue-sweeper`, `notification-dispatch`), `scoring-kpi-dashboard` (`kpi-recalculation`), `reporting-essentials` (`report-render`), `connector-sdk-mock` (`connector-sync`), `mvp-hardening` (observabilidad completa).

## Verificación

- Prueba de arquitectura: ningún paquete importa `pg-boss` salvo `packages/jobs`.
- Pruebas de integración: reintento con backoff hasta `dead_letter`; `singletonKey` serializa por tenant; un handler que lanza tras escribir no deja efectos parciales (transacción).
- Métricas visibles en `/health` del worker y en el panel de administración; alerta cuando `dead_letter` > 0.

## Estado y aprobación

Propuesto. Bloquea la arquitectura de ejecución asíncrona y de auditoría; `CLAUDE.md` exige aprobación antes de bloquear la arquitectura. Decisiones explícitas pendientes: compartir instancia PostgreSQL entre negocio y colas en producción, y retención de trabajos completados. Punto de aprobación: antes de `domain-events-outbox-jobs`.
