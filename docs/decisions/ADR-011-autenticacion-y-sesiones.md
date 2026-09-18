---
titulo: "ADR-011: Autenticación, MFA, OIDC, sesiones y claves de API"
fecha: 2026-09-17
estado: "propuesto, requiere aprobación humana"
nivel: 3
decisiones_spine: [AD-8, AD-14, AD-15]
inputs:
  - _bmad-output/planning-artifacts/architecture/architecture-ISO42001-2026-09-17/ARCHITECTURE-SPINE.md (convención Autenticación, Stack, Deferred SAML)
  - CLAUDE.md
  - _bmad-output/planning-artifacts/prds/prd-ISO42001-2026-09-17/prd.md (§17 D-7, NFR-015)
  - docs/product/ROADMAP.md (§3, cambio 3)
sustituye: []
---
# ADR-011: Autenticación, MFA, OIDC, sesiones y claves de API

## Contexto

`identity-and-rbac` exige autenticación local, MFA TOTP, OIDC, sesiones, invitaciones (7 días, D-7), ciclo de vida de usuario y claves de API para integraciones; D-7 fija MFA por defecto para ejecutivos y administradores. El spine deja la librería como `[ASSUMPTION]` y remite a este ADR. El estado del ecosistema el 2026-09-17: Auth.js/next-auth está en modo mantenimiento bajo el equipo de Better Auth desde septiembre de 2025, su v5 no alcanzó GA y no ofrece TOTP nativo; better-auth 1.7 ofrece adaptador Prisma, OIDC genérico y TOTP mediante plugins. Este ADR es arquitectura de seguridad y por tanto no puede adoptarse sin aprobación humana (CLAUDE.md).

## Decisión

1. **Librería: better-auth 1.7** `[ASSUMPTION]` en `apps/web`, con adaptador Prisma y los plugins OIDC genérico y `twoFactor` (TOTP con códigos de recuperación). Todo el acoplamiento se confina en `apps/web/lib/auth/`; ningún paquete `packages/*` importa better-auth. La única salida hacia el resto del sistema es `resolveTenantContext(request) → TenantContext { user_id | api_key_id, tenant_id, roles, session_id, locale, correlation_id }`.
2. **Sesiones** persistidas en tabla `sessions` con `tenant_id` y RLS; cookie `httpOnly, Secure, SameSite=Lax`; caducidad absoluta de 12 h e inactividad de 60 min, ambas configurables por tenant `[ASSUMPTION, alineado con SECURITY_ARCHITECTURE.md §1]`; revocación individual y "cerrar todas las sesiones".
3. **Contraseñas** con `argon2id` `[ASSUMPTION, SECURITY_ARCHITECTURE.md §1]` (better-auth usa scrypt por defecto; se configura el hasher), parámetros revisados en `mvp-hardening`; tokens de invitación y de restablecimiento de un solo uso y con caducidad.
4. **MFA TOTP** obligatoria por política de tenant para los roles `Executive` y administradores (D-7); enrolamiento forzado en el primer inicio de sesión; códigos de recuperación almacenados con hash.
5. **OIDC por tenant**: `issuer`, `client_id` y `client_secret_ref` (ADR-014) en la configuración del tenant; aprovisionamiento just-in-time solo si el tenant lo activa; mapeo de roles por claims `[ASSUMPTION]`. SAML diferido (spine, Deferred).
6. **Claves de API** fuera de better-auth: tabla `api_keys { id, tenant_id, prefix, key_hash (SHA-256), scopes (subconjunto de permisos), expires_at, last_used_at, revoked_at }`. `Authorization: Bearer <key>` se resuelve al mismo `TenantContext` con `principal_type = api_key`; el valor solo se muestra una vez al crearse.
7. **Eventos** al outbox: `USER_LOGGED_IN`, `LOGIN_FAILED`, `MFA_ENROLLED`, `SESSION_REVOKED`, `API_KEY_CREATED`, `API_KEY_REVOKED` `[ASSUMPTION nombres]`, consumidos por el registro de auditoría. Rate limiting en los endpoints de autenticación.

## Alternativas consideradas

| Alternativa | Por qué se descarta |
| --- | --- |
| Auth.js / next-auth v4 | En mantenimiento; sin TOTP nativo; su modelo de sesión no encaja con sesiones revocables por tenant sin trabajo adicional. |
| Auth.js v5 | Sin GA ni TOTP; el equipo que lo mantiene recomienda better-auth. |
| Keycloak u otro IdP externo como único mecanismo | Añade un servicio con estado y operación propia; contradice la envolvente de una imagen (AD-20). Sigue disponible como proveedor OIDC del tenant. |
| Clerk / Auth0 (SaaS) | Datos de identidad fuera de la región del tenant (NFR-015), coste por usuario activo y dependencia del proveedor. |

## Consecuencias

**Positivas.** TOTP y OIDC sin código criptográfico propio; sesiones revocables por tenant; API keys con permisos acotados y trazables; el dominio no conoce la librería, por lo que sustituirla afecta a una carpeta.

**Negativas.** Librería joven (1.x); sus peer dependencies declaran Prisma 5-7 mientras el spine pinnea Prisma 7.10 y anticipa Prisma 8: la migración a Prisma 8 puede quedar bloqueada por el adaptador.

**Riesgos y mitigación.** Si better-auth deja de ser viable, el plan de contingencia es Auth.js v4 o una implementación acotada sobre primitivas `[ASSUMPTION]`; ambos caben tras `resolveTenantContext` sin tocar dominio.

**Cambios OpenSpec.** `identity-and-rbac` (todo lo anterior); `organization-tenancy` (política de MFA y OIDC como configuración del tenant); `connector-sdk-mock` (consumo de API keys por integraciones); `mvp-hardening` (rate limiting, cabeceras seguras, revisión de parámetros criptográficos).

## Verificación

- `tests/security`: fijación de sesión, atributos de cookie, CSRF (SameSite más comprobación de `Origin`), intento de saltar MFA, fuerza bruta bloqueada por rate limiting, API key del tenant A sobre recursos del tenant B → `TENANT_MISMATCH`.
- `dependency-cruiser` `[ASSUMPTION]`: `better-auth` solo se importa desde `apps/web/lib/auth/`.
- Integración: OIDC contra un proveedor de pruebas en contenedor `[ASSUMPTION]`; enrolamiento TOTP con reloj controlado.

## Estado y aprobación

Propuesto y **bloqueado hasta aprobación humana**: CLAUDE.md exige aprobación para cambios de arquitectura de seguridad, y aquí se fijan librería, parámetros de sesión, política de MFA, algoritmo de hash de contraseñas y formato de claves de API. Además debe ratificarse la aceptación del riesgo de peer dependencies frente a Prisma 8 y el diferimiento de SAML.
