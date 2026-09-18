---
titulo: "ADR-003: Monolito modular por bounded context con eventos de dominio y patrón outbox"
fecha: 2026-09-17
estado: "propuesto, requiere aprobación humana"
nivel: 3
decisiones_spine: [AD-1, AD-3, AD-4, AD-5, AD-9, AD-10]
inputs: [CLAUDE.md, docs/product/PROMPT_REVIEW_AND_ADJUSTMENTS.md (A8), docs/product/DOMAIN_MAP.md (§2-§5), _bmad-output/planning-artifacts/prds/prd-ISO42001-2026-09-17/prd.md (§8.6, §8.7, FR-012, FR-064), docs/product/MASTER_ISO42001_PLATFORM_REQUIREMENTS.md (§44, §65)]
sustituye: []
---

# ADR-003: Monolito modular por bounded context con eventos de dominio y patrón outbox

## Contexto

`DOMAIN_MAP.md` define 21 bounded contexts con propiedad única de entidades y una matriz de más de 50 eventos que encadenan flujos (por ejemplo `MODEL_VERSION_CHANGED` → reevaluación de riesgo → revisión de impacto → revisión de controles → solicitud de evidencia; `CONTROL_ACTIVATED` → tareas, solicitudes y métricas). El PRD §8.6 exige que todo cambio de estado relevante escriba su evento en la misma transacción y que Administration consuma todos los eventos hacia el registro de auditoría. El equipo es pequeño y el MVP debe desplegarse como una unidad; A8 fija monolito modular con eventos internos y outbox, sin microservicios.

## Decisión

- **Monolito modular hexagonal**: cada contexto es `packages/domain-<context>` con entidades, máquinas de estado, commands, queries, puertos y sus propios adaptadores Prisma; `apps/web` y `apps/worker` solo componen (`AD-1`).
- **Dependencias**: L3 → L2 → L1 → L0; entre contextos solo `index.ts` público (puertos de lectura, tipos, nombres de evento) y eventos; cada tabla tiene un único escritor (`AD-3`); Reporting, Administration y AI Assistance leen por puertos registrados en el contenedor, sin import estático del núcleo.
- **Mutación**: todo cambio de estado es un command con `authorize → validate → transaction { mutate; append outbox }`; toda mutación produce al menos un evento del catálogo o `ENTITY_MUTATED` (`AD-4`).
- **Outbox**: tabla `outbox_events` escrita en la misma transacción que la mutación; un relay en el worker la publica a colas pg-boss (`events.<TYPE>`); consumidores idempotentes con `processed_events(consumer_name, event_id)`; reintentos exponenciales y `dead_letter`; `correlation_id`/`causation_id` propagados (`AD-5`).
- **Sincronía permitida**: lecturas por id a través de `*ReadPort` para componer pantallas y validar referencias; **nunca** una mutación síncrona en otro contexto. La única excepción controlada es `ApprovalPort` (Workflow), invocable dentro del command que necesita aprobación (`AD-10`).
- **Regla de la SoA**: solo Controls & SoA emite `CONTROL_ACTIVATED/DEACTIVATED/CONTROL_APPLICABILITY_CHANGED/SOA_APPROVED`; el resto reacciona o consulta `SoAReadPort.isControlActive` (`AD-9`).

## Alternativas consideradas

| Alternativa | Por qué se descarta |
| --- | --- |
| Microservicios por contexto | 21 servicios, transacciones distribuidas y RLS por servicio para un equipo pequeño; coste operativo desproporcionado en el MVP; el monolito modular conserva la opción de extraer un contexto (puertos y eventos ya existen) |
| Monolito sin módulos (una capa `services/`) | Imposible garantizar propiedad única de tablas ni prevenir que Evidence infiera aplicabilidad leyendo `soa_items`; contradice `DOMAIN_MAP.md` |
| Publicar eventos directamente al bus tras el commit (sin outbox) | Pérdida de eventos si el proceso cae entre commit y publish; el PRD exige misma transacción |
| Event sourcing completo | Curva de aprendizaje y complejidad de proyecciones para 21 contextos; el requisito es auditabilidad de decisiones, que cubre el outbox + audit trail + `*_versions` (ADR-005, ADR-015) |
| Bus externo (Kafka, RabbitMQ) | Nueva dependencia operativa sin necesidad de throughput; pg-boss sobre PostgreSQL basta (ADR-004) |
| Llamadas síncronas entre contextos para mutar | Acopla los flujos y hace imposible la regla "la SoA gobierna los flujos" con un único emisor |

## Consecuencias

- Positivas: un despliegue; límites verificables por pruebas de arquitectura; eventos garantizados; auditoría completa como consumidor; extraer un contexto en el futuro es mover un paquete y sustituir colas locales por un bus.
- Negativas: consistencia eventual entre contextos (una tarea aparece milisegundos después de activar un control; la UI debe reflejarlo con estados "en proceso"); disciplina de código exigente (sin acceso directo a Prisma fuera de adaptadores); el relay y los consumidores son código propio que hay que operar y observar.
- Riesgos: consumidores no idempotentes que dupliquen tareas → mitigado por `processed_events` y pruebas obligatorias; orden entre eventos de distinto tipo no garantizado → los consumidores se diseñan tolerantes al orden y `audit-trail-writer` se serializa por tenant.
- Cambios OpenSpec que lo implementan: `foundation-project-bootstrap` (estructura de paquetes, pruebas de arquitectura), `domain-events-outbox-jobs` (outbox, relay, consumidores, `Approval` mínimo), `soa-control-activation` (regla de la SoA), todos los cambios de dominio (un paquete cada uno).

## Verificación

- `dependency-cruiser` `[ASSUMPTION herramienta]` con reglas: sin import de `packages/domain-*/src/**` salvo `index.ts`; sin import de `@prisma/client` fuera de `packages/db` y `adapters/prisma`; sin ciclos.
- Prueba en `packages/db`: cada modelo Prisma se declara en un único fragmento de contexto.
- Prueba de integración: una mutación con fallo tras el `INSERT` en outbox no publica ningún evento; un evento entregado dos veces produce un único efecto.
- Prueba nombrada `PR-DR-001`: control `Not Applicable` no genera tareas, solicitudes ni métricas.

## Estado y aprobación

Propuesto. Bloquea la arquitectura (`CLAUDE.md`). Requiere decisión explícita sobre: consistencia eventual aceptable en la UI, y la excepción de invocación síncrona de `ApprovalPort`. Punto de aprobación: Checkpoint 0.
