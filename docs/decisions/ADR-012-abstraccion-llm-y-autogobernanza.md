---
titulo: "ADR-012: Abstracción del proveedor LLM y autogobernanza de la plataforma como sistema de IA"
fecha: 2026-09-17
estado: "propuesto, requiere aprobación humana"
nivel: 3
decisiones_spine: [AD-17, AD-8, AD-14, AD-3]
inputs:
  - _bmad-output/planning-artifacts/architecture/architecture-ISO42001-2026-09-17/ARCHITECTURE-SPINE.md
  - CLAUDE.md
  - docs/product/PROMPT_REVIEW_AND_ADJUSTMENTS.md (A9)
  - docs/product/ROADMAP.md (§3, F2)
sustituye: []
---
# ADR-012: Abstracción del proveedor LLM y autogobernanza de la plataforma como sistema de IA

## Contexto

A9 establece que la plataforma es a su vez un sistema de IA: sus funciones asistidas por LLM (borradores de políticas, sugerencia de riesgos, análisis de evidencia) deben inscribirse en el inventario de IA del tenant plataforma y quedar sujetas a los controles A.6, A.8 y A.9 tal como los nombra A9. CLAUDE.md fija que "la IA sugiere; una persona autorizada aprueba". La IA asistida es F2, pero el puerto, las entidades y los permisos deben existir desde el MVP (AD-17).

## Decisión

1. **Puerto `LLMProvider`** en `packages/ai-core`: `complete(req: LLMRequest): Promise<LLMResponse>` y `stream` opcional. `LLMRequest { task_id, tenant_id, model_ref, messages[], max_tokens, temperature, tools? }`; `LLMResponse { content, model, provider, usage { input_tokens, output_tokens }, finish_reason, latency_ms }`. El dominio solo importa este puerto; los adaptadores `anthropic` (Anthropic TypeScript SDK 0.126), `openai` y `azure-openai` viven en `packages/ai-core/adapters/` y se cablean en `apps/*`. En el MVP se registra `NullLLMProvider`, que responde `AI_NOT_CONFIGURED`.
2. **Modelos por referencia lógica, nunca cableados.** El tenant configura `model_refs { default, fast, reasoning }` hacia identificadores de proveedor. Valores por defecto de plataforma, definidos en configuración validada con Zod y no en código de dominio `[ASSUMPTION mapeo]`: `default → claude-sonnet-5`, `fast → claude-haiku-4-5-20251001`, `reasoning → claude-opus-5`; `claude-fable-5-1` disponible como opción. Cualquier identificador es sustituible por modelos de OpenAI o Azure OpenAI sin tocar dominio.
3. **Flujo explícito** (nunca por evento): command → `authorize` → `AIAssistanceTask { type, requested_by, input_refs[], status }` → comprobación de `TenantAIBudget` (tope mensual de tokens y coste estimado, tope por tarea; exceso → `AI_BUDGET_EXCEEDED`) → prompt desde plantillas de `domain-ai-assistance` con referencias al catálogo → `LLMProvider` → `LLMUsageRecord { model, provider, input_tokens, output_tokens, cost_estimate, prompt_hash, latency_ms }` → `AIGeneratedArtifact { generated_by_ai: true, model, generated_at, task_id, source_refs[], review_status: Pending Review, reviewer_id }`.
4. **Principal `ai-assistant`** por tenant con permisos exclusivamente `*.suggest`; no puede ejecutar ningún command que cambie estado de requisito, control, riesgo, evidencia ni CAPA.
5. **Validación normativa.** Toda referencia a cláusula o control en un artefacto se resuelve contra el catálogo global (AD-7); las no resueltas se marcan y jamás se muestran como cita normativa.
6. **Datos.** Nunca se envían binarios de evidencia; solo extractos de texto y solo si el tenant activa `ai_data_sharing_enabled` (por defecto desactivado) `[ASSUMPTION]`. Prompts y respuestas no van a logs. Credenciales vía `SecretStore` (ADR-014).
7. **Autogobernanza.** Cada función asistida se registra como `AISystem` del tenant plataforma con `kind = platform_feature` `[ASSUMPTION]`, modelo, proveedor y propósito; cambiar el `model_ref` por defecto emite `MODEL_VERSION_CHANGED` sobre ese `AISystem`, disparando la reevaluación de riesgo e impacto como en cualquier sistema inventariado.

## Alternativas consideradas

| Alternativa | Por qué se descarta |
| --- | --- |
| Importar el SDK del proveedor en los paquetes de dominio | Acopla 21 contextos a un proveedor; contradice A9 y AD-17. |
| Framework de agentes u orquestación (LangChain, Vercel AI SDK) | Arrastra dependencias y ritmo de cambios al dominio; puede usarse dentro de un adaptador en F2 `[ASSUMPTION]`, no como puerto. |
| Invocar el LLM por eventos de dominio | Coste no controlable y artefactos sin solicitante humano identificable. |
| Sin registro de la plataforma como sistema de IA | Incumple A9 y desaprovecha la mejor demostración del producto. |

## Consecuencias

**Positivas.** Sustitución de proveedor sin tocar dominio; coste acotado por tenant; cada artefacto es trazable a tarea, modelo y fuentes; el propio producto demuestra los controles que vende.

**Negativas.** La tabla de precios para `cost_estimate` debe mantenerse como configuración; los identificadores de modelo caducan y obligan a revisar los valores por defecto.

**Cambios OpenSpec.** `foundation-project-bootstrap` (`packages/ai-core` con puerto y `NullLLMProvider`); `identity-and-rbac` (permisos `*.suggest`, principal `ai-assistant`); `ai-inventory-core` (registro de la plataforma como `AISystem`); `mvp-hardening` (prueba de permisos del principal); F2: cambio `ai-assistance-core` `[ASSUMPTION nombre]` con adaptadores, presupuesto y caché.

## Verificación

- Prueba de matriz de permisos: el principal `ai-assistant` recibe `PERMISSION_DENIED` en todo command que no termine en `.suggest`.
- `dependency-cruiser` `[ASSUMPTION]`: `@anthropic-ai/sdk`, `openai` y equivalentes solo en `packages/ai-core/adapters/`.
- Unitarias: presupuesto excedido rechaza la tarea; artefacto siempre `generated_by_ai: true` y `Pending Review`; referencia normativa desconocida queda marcada.
- Integración: tras la semilla existe al menos un `AISystem` con `kind = platform_feature`.

## Estado y aprobación

Propuesto. Requiere aprobación humana por el mapeo de modelos por defecto, los topes de presupuesto, el valor por defecto de `ai_data_sharing_enabled` (decisión de privacidad y seguridad) y porque registrar la plataforma como sistema de IA es una `organizational_decision` del tenant plataforma. Las credenciales de producción siempre requieren aprobación humana.
