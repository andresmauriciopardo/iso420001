---
titulo: "ADR-016: SDK de conectores fuera del dominio con estados honestos"
fecha: 2026-09-17
estado: "propuesto, requiere aprobación humana"
nivel: 3
decisiones_spine: [AD-18, AD-14, AD-3, AD-5]
inputs:
  - _bmad-output/planning-artifacts/architecture/architecture-ISO42001-2026-09-17/ARCHITECTURE-SPINE.md
  - CLAUDE.md
  - docs/product/PROMPT_REVIEW_AND_ADJUSTMENTS.md (A9)
  - docs/product/MASTER_ISO42001_PLATFORM_REQUIREMENTS.md (§62)
  - _bmad-output/planning-artifacts/prds/prd-ISO42001-2026-09-17/prd.md (NFR-021, NFR-024)
  - _bmad-output/planning-artifacts/prds/prd-ISO42001-2026-09-17/addendum.md (§C)
  - docs/product/ROADMAP.md (§3, cambio 21)
sustituye: []
---
# ADR-016: SDK de conectores fuera del dominio con estados honestos

## Contexto

CLAUDE.md prohíbe "afirmar que un conector funciona sin validarlo contra el proveedor real" y "UI con botones falsos"; NFR-021 exige que un nuevo conector no toque el dominio; NFR-024 pide pruebas de contrato contra un adaptador mock etiquetado (PR-DR-020, PR-DR-021). P2 §62 fija diez tipos de error de conector con guía de remediación y P2 §16 la disponibilidad por dato. El cambio `connector-sdk-mock` construye el SDK en el MVP; los conectores reales llegan en F2.

## Decisión

1. **`packages/connectors-sdk`** define el contrato:
   - `ConnectorManifest { provider_id, display_name, version, is_mock, capabilities[], required_credentials[], data_items[]: { key, availability } }`.
   - `ProviderAdapter { testConnection(creds) → ConnectionTestResult; listCapabilities(); sync(ctx, capability, cursor) → AsyncIterable<RawRecord>; normalize(raw) → NormalizedRecord }`, donde `NormalizedRecord.kind ∈ { EvidenceCandidate, CandidateAISystem, IntegrationFinding }`.
   - `ConnectorError { code: ConnectorErrorCode, provider_message, remediation, retryable }`, con `ConnectorErrorCode` exactamente los diez valores de P2 §62 (`Authentication Failed` … `Unknown Error`) definidos en `packages/kernel/enums`.
   - Disponibilidad por dato: `Available | Not Available | Permission Required | Not Supported by Provider API | Not Configured` (addendum §C). La UI muestra la etiqueta, nunca un vacío.
2. **Pipeline fijo**, ejecutado por el runtime del SDK en `apps/worker`: `ConnectorInterface → ProviderAdapter → CredentialRef (SecretStore, ADR-014) → ConnectionTest → SyncJob (pg-boss) → Normalization → Mapping → Evidence | CandidateAISystem | IntegrationFinding`. El runtime aplica reintentos con backoff ante `Rate Limited` y `Provider Unavailable` (AD-5).
3. **Adaptadores en `packages/connectors-<provider>`**, que dependen solo de `connectors-sdk` y `kernel`, y se registran en un registro estático en la composición de `apps/worker`; no hay carga dinámica.
4. **El dominio no conoce adaptadores.** `domain-integrations` posee `Integration`, `IntegrationSync`, `MappingRule` e `IntegrationFinding`; entrega los registros normalizados a otros contextos solo por sus puertos públicos (`EvidenceIngestPort`, `AISystemCandidatePort` `[ASSUMPTION nombres]`), nunca escribiendo sus tablas (AD-3). Toda evidencia ingerida lleva `EvidenceProvenance { integration_id, provider_id, external_id, fetched_at, is_mock }`.
5. **Estados honestos.** `Integration.status ∈ { mock, configured, validated }`; `validated` solo lo asigna un `ConnectionTest` real satisfactorio y se registra con `validated_at`; un fallo posterior o una rotación de credenciales devuelve el estado a `configured`. El adaptador `mock` (`is_mock: true`) no puede alcanzar `validated`; sus registros llevan `is_mock` y la UI los etiqueta (PR-DR-020).
6. **Kit de contrato** exportado desde `connectors-sdk/testing`: `runContractTests(adapter)` verifica manifiesto, mapeo de errores a los diez códigos con remediación no vacía, esquema de normalización y reanudación por cursor. Las pruebas contra proveedores reales se activan solo con credenciales de entorno y nunca en CI por defecto.

## Alternativas consideradas

| Alternativa | Por qué se descarta |
| --- | --- |
| Integraciones cableadas dentro de cada contexto | Incumple NFR-021; cada conector tocaría Evidence y AI Portfolio; pruebas imposibles sin credenciales. |
| iPaaS externo (Zapier, n8n, Workato) | Los datos del tenant transitan por terceros (NFR-015); procedencia de la evidencia débil; puede ser un proveedor más en F2, no la base. |
| Plugins dinámicos cargados en tiempo de ejecución | Ejecución de código no auditado y riesgo de cadena de suministro en una plataforma de cumplimiento. |

## Consecuencias

**Positivas.** Añadir un conector es añadir un paquete; la procedencia de cada evidencia es explícita; el estado `validated` es demostrable; los errores tienen taxonomía y remediación desde el MVP.

**Negativas.** Abstracción construida antes del primer conector real, con riesgo de que el primer adaptador (Q9) revele huecos; se acepta y se revisa el SDK tras ese adaptador.

**Cambios OpenSpec.** `connector-sdk-mock` (SDK, runtime, mock etiquetado, kit de contrato, `EvidenceProvenance`); `evidence-library` (campo de procedencia y `EvidenceIngestPort`); `ai-inventory-core` (aceptación de `CandidateAISystem` `[ASSUMPTION fase]`); `identity-and-rbac` (API keys para integraciones); `mvp-hardening` (prueba de exposición de secretos en DTOs de integración); F2: `connector-azure-ai-foundry` y siguientes `[ASSUMPTION nombres]`.

## Verificación

- Prueba de arquitectura "nuevo conector sin tocar dominio": un adaptador de fixture bajo `packages/connectors-fixture` compila, pasa `runContractTests` y no produce ningún diff en `packages/domain-*`.
- `dependency-cruiser` `[ASSUMPTION]`: `connectors-*` importan solo `connectors-sdk` y `kernel`; ningún `domain-*` importa `connectors-*`.
- Unitarias: el adaptador mock nunca alcanza `validated`; todo `ConnectorError` mapea a uno de los diez códigos con `remediation` no vacía; cada registro del mock lleva `is_mock: true`.
- `tests/security`: los DTOs de `Integration` no contienen valores de credenciales (ADR-014).

## Estado y aprobación

Propuesto. Requiere aprobación humana porque fija el contrato que usarán todos los conectores futuros (bloqueo de arquitectura), porque la semántica de `validated` es la garantía frente a la prohibición de CLAUDE.md de afirmar integraciones no validadas, y porque toda prueba contra cuentas reales y toda credencial de producción exigen aprobación explícita.
