---
titulo: "ADR-014: Gestor de secretos por puerto SecretStore con referencias opacas"
fecha: 2026-09-17
estado: "propuesto, requiere aprobación humana"
nivel: 3
decisiones_spine: [AD-14, AD-4, AD-19]
inputs:
  - _bmad-output/planning-artifacts/architecture/architecture-ISO42001-2026-09-17/ARCHITECTURE-SPINE.md
  - CLAUDE.md
  - docs/product/PROMPT_REVIEW_AND_ADJUSTMENTS.md (A9)
  - docs/product/ROADMAP.md (§3, cambio 21)
sustituye: []
---
# ADR-014: Gestor de secretos por puerto SecretStore con referencias opacas

## Contexto

La plataforma custodia secretos de tenant: credenciales de conectores (Integrations), claves de proveedores LLM (AI Assistance), `client_secret` de OIDC (Identity & Organization) y credenciales SMTP (Notifications). CLAUDE.md exige "secretos solo vía abstracción de gestor de secretos, nunca en columnas normales ni en el frontend" y aprobación humana para credenciales de producción. AD-14 fija que la BD guarda solo `secret_ref` y que toda lectura y escritura emite evento. El KMS/Vault concreto está diferido hasta antes de las primeras credenciales reales de conectores (F2), porque depende del proveedor de nube (Q6).

## Decisión

1. **Puerto `SecretStore`** en `packages/secrets`: `create(ctx, purpose, value, metadata) → secret_ref`, `read(ctx, secret_ref) → SecretValue`, `rotate(ctx, secret_ref, value)`, `revoke(ctx, secret_ref)`. `secret_ref` es opaco con formato `sec_<uuid v7>` `[ASSUMPTION]`; las entidades lo guardan en columnas `*_secret_ref` y nunca el valor.
2. **Dos clases de secreto.** Los secretos de plataforma (`DATABASE_URL`, `SECRET_STORE_MASTER_KEY`, DSN de OTel) llegan por variables de entorno validadas en `packages/kernel/config.ts` y los inyecta el gestor de secretos del despliegue; los secretos de tenant pasan siempre por `SecretStore`. Ningún módulo lee `process.env` fuera de `config.ts`.
3. **Adaptador MVP `EnvEncryptedSecretStore`**: cifra con AES-256-GCM `[ASSUMPTION]` usando la clave maestra de entorno (32 bytes, validada con Zod) y guarda el cifrado en la tabla `secret_values(secret_ref, tenant_id, purpose, ciphertext, nonce, key_version, created_at, rotated_at, revoked_at)`, propiedad exclusiva de `packages/secrets` y con RLS. `key_version` permite rotar la clave maestra recifrando. Se usa en `local`, `ci` y también en `staging`/`production` del MVP, donde los únicos secretos de tenant previstos son OIDC y SMTP `[ASSUMPTION]`.
4. **Adaptador F2 `KmsSecretStore`** (KMS gestionado o Vault, según Q6) obligatorio antes de que exista cualquier credencial real de conector (spine, Deferred).
5. **Eventos.** Cada `read` emite `CREDENTIAL_ACCESSED { secret_ref, purpose, actor_id, correlation_id }`; `create/rotate/revoke` emiten `CREDENTIAL_CREATED | CREDENTIAL_ROTATED | CREDENTIAL_REVOKED` (`CREDENTIAL_UPDATED` cuando cambian solo metadatos). Ningún evento ni log contiene el valor.
6. **Contención del valor.** `SecretValue` es una clase cuyo `toJSON` y `toString` lanzan error `[ASSUMPTION mecanismo]`, de modo que no puede serializarse por accidente en un DTO, un Server Component ni un log; el logger de `packages/observability` además redacta patrones conocidos. Solo los servicios de aplicación de los cuatro contextos propietarios invocan `read`; ningún rol de usuario tiene permiso de lectura de valores; los usuarios crean, rotan y revocan, nunca leen.

## Alternativas consideradas

| Alternativa | Por qué se descarta |
| --- | --- |
| Columnas cifradas dentro de cada tabla de entidad | Cifrado disperso en 4 contextos, sin rotación ni auditoría uniforme; imposible sustituir por KMS sin migrar cada tabla. |
| Vault desde el día 1 | Infraestructura y operación adicionales para un MVP sin credenciales reales de conectores. |
| Variables de entorno sin abstracción | Sin secretos por tenant, sin rotación, sin `CREDENTIAL_*`; incumple CLAUDE.md. |
| Llamar al KMS del proveedor desde el dominio | Acopla el dominio a un proveedor antes de decidir Q6. |

## Consecuencias

**Positivas.** Un solo lugar para custodia, rotación y auditoría; sustituir el adaptador no toca dominio; la prueba de exposición se escribe una vez.

**Negativas.** La clave maestra es el punto único de fallo: su pérdida inutiliza todos los secretos de tenant; su custodia y copia se documentan en el runbook de despliegue. Un evento por lectura genera volumen en el registro de auditoría; se acepta porque la lectura de credenciales es exactamente lo que un auditor quiere ver.

**Cambios OpenSpec.** `foundation-project-bootstrap` (`packages/secrets`, adaptador env, `config.ts`); `identity-and-rbac` (`client_secret_ref` de OIDC); `tasks-approvals-notifications` (SMTP); `connector-sdk-mock` (`CredentialRef` del pipeline); `mvp-hardening` (prueba de exposición completa); F2: `secret-store-kms` `[ASSUMPTION nombre]`.

## Verificación

- `tests/security` "exposición de secretos": crea un secreto con un valor centinela y comprueba que no aparece en ninguna respuesta de la API, en `openapi.json`, en las props serializadas de Server Components ni en los logs capturados.
- Prueba de arquitectura: solo `packages/secrets` referencia `secret_values`; `process.env` solo en `kernel/config.ts`.
- Unitarias: `read` emite `CREDENTIAL_ACCESSED`; `read` de un `secret_ref` revocado falla con `CREDENTIAL_REVOKED`; `JSON.stringify(secretValue)` lanza.
- Integración: rotación de la clave maestra recifra todas las filas y conserva la legibilidad.
- CI: escáner de secretos en commits (`gitleaks` `[ASSUMPTION]`).

## Estado y aprobación

Propuesto. Requiere aprobación humana porque es arquitectura de seguridad: esquema criptográfico, custodia de la clave maestra, uso del adaptador de entorno en producción durante el MVP y momento de adopción del KMS. Toda credencial de producción y toda integración contra cuentas reales siguen exigiendo aprobación explícita (CLAUDE.md).
