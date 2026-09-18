---
titulo: "Matriz de trazabilidad"
fecha: 2026-09-17
estado: "borrador para revisión"
nivel: 3
ultima_actualizacion: "2026-09-17"
ultimo_cambio_archivado: "—"
version_semilla: "standards-seed@0.0.0 (no publicada)"
inputs:
  - docs/product/TRACEABILITY_MODEL.md (§3, §4, §5)
  - docs/product/ROADMAP.md (§3, cambios 1-5)
  - _bmad-output/planning-artifacts/prds/prd-ISO42001-2026-09-17/prd.md (§6.1, §7, §9)
  - _bmad-output/planning-artifacts/prds/prd-ISO42001-2026-09-17/addendum.md (§A)
  - docs/product/DOMAIN_MAP.md (§3, §4)
  - resumenes/01 §4 (identificadores de cláusula, solo verificación)
spine: _bmad-output/planning-artifacts/architecture/architecture-ISO42001-2026-09-17/ARCHITECTURE-SPINE.md
---

# Matriz de trazabilidad

Esqueleto conforme a `TRACEABILITY_MODEL.md` §4. Contiene las filas iniciales de los **cinco primeros cambios OpenSpec** de `ROADMAP.md` §3. Ningún cambio está propuesto ni implementado: las columnas `epic`, `story`, `implementation`, `tests` y `evidence` están en `pendiente` y todas las filas en estado `planned`. El procedimiento de actualización al archivar cada cambio es el de `TRACEABILITY_MODEL.md` §5; las reglas de integridad de §4.3 se comprobarán por script en CI en la fase tardía del MVP.

Convenciones: listas separadas por `;`; rutas abreviadas con el prefijo del paquete declarado al pie de cada tabla; `[ASSUMPTION]` marca todo vínculo con un requisito ISO inferido por la arquitectura y pendiente de confirmación en la propuesta OpenSpec del cambio. Nunca se cita texto literal de la norma; `iso_title` es paráfrasis corta.

## 1. Resumen

| Ámbito | Requisitos ISO en la semilla | Con PR-* | Cubiertos por un cambio | Implementados | Verificados | Brechas |
| --- | --- | --- | --- | --- | --- | --- |
| Cláusulas 4-10 (42001) | pendiente de `standards-seed@1.0.0` | pendiente | 2 filas iniciales `[ASSUMPTION]` | 0 | 0 | pendiente |
| Anexo A (38 controles) | 38 | pendiente | 1 fila inicial `[ASSUMPTION]` | 0 | 0 | pendiente |
| Requisitos de producto sin requisito ISO (§3) | — | 22 PR-* de los cambios 1-5 | 22 | 0 | 0 | 0 |

El resumen por cláusula y por dominio del Anexo A se completa al archivar `standards-and-requirements-engine` (cambio 6), cuando exista `standards-seed@1.0.0`.

## 2. Matriz principal

Una fila por par Requisito ISO × PR. Las filas de esta sección son las únicas de los cambios 1-5 con un vínculo normativo defendible; todas llevan `[ASSUMPTION]` hasta que la propuesta OpenSpec del cambio las confirme o las retire.

| source_doc | iso_requirement | record_type | iso_title | product_requirement | context | capability | epic | story | change | implementation | tests | evidence | phase | status | notes |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| resumenes/01 §4 5.3 | 42001:5.3 | normative_requirement | Roles, responsabilidades y autoridades asignadas y comunicadas | PR-XC-004 | Identity & Organization | identity-and-rbac | pendiente | pendiente | identity-and-rbac | pendiente | pendiente | pendiente | MVP | planned | `[ASSUMPTION]` los 12 roles y el catálogo de permisos soportan la asignación de autoridades; la definición RACI del SGIA vive en `aims-policy-objectives-roles` (FR-030) |
| resumenes/02 §4.12 A.3.2 | 42001:A.3.2 | normative_requirement | Roles y responsabilidades en materia de IA definidos y asignados | PR-XC-004 | Identity & Organization | identity-and-rbac | pendiente | pendiente | identity-and-rbac | pendiente | pendiente | pendiente | MVP | planned | `[ASSUMPTION]` el control se opera desde Controls & SoA (cambio 12); RBAC es soporte técnico, no implementación del control |
| resumenes/01 §4 7.5.3 | 42001:7.5.3 | normative_requirement | Control de la información documentada: protección, acceso, trazabilidad de cambios | PR-XC-006 | Administration | immutable-audit-trail | pendiente | pendiente | immutable-audit-trail | pendiente | pendiente | pendiente | MVP | planned | `[ASSUMPTION]` el registro append-only con cadena de hashes aporta evidencia de control de cambios; la gestión documental completa llega con FR-079 (cambio 8) |

## 3. Requisitos de producto sin requisito ISO

Mismas columnas; `source_doc = producto`, `iso_requirement = —`, `record_type = product_requirement`, `iso_title = —`. Fuente de cada PR-*: `REQUIREMENTS_ANALYSIS.md` y `ROADMAP.md` §3; FR/NFR del PRD en `notes`.

### 3.1 Cambio 1 — `foundation-project-bootstrap`

| source_doc | iso_requirement | record_type | iso_title | product_requirement | context | capability | epic | story | change | implementation | tests | evidence | phase | status | notes |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| producto | — | product_requirement | — | PR-NFR-003 | Administration | foundation-project-bootstrap | pendiente | pendiente | foundation-project-bootstrap | pendiente | pendiente | pendiente | MVP | planned | NFR-003 cadena de suministro: escaneo de dependencias, SAST, secretos en CI; ADR-001 |
| producto | — | product_requirement | — | PR-NFR-007 (base) | Administration | foundation-project-bootstrap | pendiente | pendiente | foundation-project-bootstrap | pendiente | pendiente | pendiente | MVP | planned | rendimiento percibido: base (p95 medido en `mvp-hardening`) |
| producto | — | product_requirement | — | PR-NFR-008 (base) | Administration | foundation-project-bootstrap | pendiente | pendiente | foundation-project-bootstrap | pendiente | pendiente | pendiente | MVP | planned | disponibilidad, copias y DR: base Docker; valores D-7 `[ASSUMPTION]`; AD-20 |
| producto | — | product_requirement | — | PR-NFR-013 | Administration | foundation-project-bootstrap | pendiente | pendiente | foundation-project-bootstrap | pendiente | pendiente | pendiente | MVP | planned | NFR-013 observabilidad OpenTelemetry; AD-19 |
| producto | — | product_requirement | — | PR-NFR-022 | Administration | foundation-project-bootstrap | pendiente | pendiente | foundation-project-bootstrap | pendiente | pendiente | pendiente | MVP | planned | NFR-022 documentación viva en `docs/` |
| producto | — | product_requirement | — | PR-XC-012 (base) | Reporting | foundation-project-bootstrap | pendiente | pendiente | foundation-project-bootstrap | pendiente | pendiente | pendiente | MVP | planned | NFR-018 UI bilingüe ES/EN desde la fundación; AD-12; ADR-007 |
| producto | — | product_requirement | — | PR-EXT-009 | Administration | foundation-project-bootstrap | pendiente | pendiente | foundation-project-bootstrap | pendiente | pendiente | pendiente | MVP | planned | NFR-009 WCAG 2.2 AA: design system accesible `packages/ui` |
| producto | — | product_requirement | — | PR-EXT-012 | Administration | foundation-project-bootstrap | pendiente | pendiente | foundation-project-bootstrap | pendiente | pendiente | pendiente | MVP | planned | NFR-019 cambios de esquema solo por migración; migración inicial aplicada |

### 3.2 Cambio 2 — `organization-tenancy`

| source_doc | iso_requirement | record_type | iso_title | product_requirement | context | capability | epic | story | change | implementation | tests | evidence | phase | status | notes |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| producto | — | product_requirement | — | PR-XC-002 | Identity & Organization | organization-tenancy | pendiente | pendiente | organization-tenancy | pendiente | pendiente | pendiente | MVP | planned | FR-001 crear y estructurar la organización (tenant, unidades, departamentos, ubicaciones, equipos, membresías); `TENANT_CREATED` en outbox |
| producto | — | product_requirement | — | PR-XC-003 | Identity & Organization | organization-tenancy | pendiente | pendiente | organization-tenancy | pendiente | pendiente | pendiente | MVP | planned | FR-002 aislamiento por tenant: RLS FORCE, prueba automática cross-tenant; AD-2; ADR-002 |
| producto | — | product_requirement | — | PR-NFR-011 (campo) | Identity & Organization | organization-tenancy | pendiente | pendiente | organization-tenancy | pendiente | pendiente | pendiente | MVP | planned | NFR-015 residencia: campo `data_region` por tenant; despliegue por región diferido (PRD Q6) |
| producto | — | product_requirement | — | PR-EXT-009 | Identity & Organization | organization-tenancy | pendiente | pendiente | organization-tenancy | pendiente | pendiente | pendiente | MVP | planned | FR-006 configuración del tenant (`tenant_settings`), accesible WCAG |

### 3.3 Cambio 3 — `identity-and-rbac`

| source_doc | iso_requirement | record_type | iso_title | product_requirement | context | capability | epic | story | change | implementation | tests | evidence | phase | status | notes |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| producto | — | product_requirement | — | PR-XC-001 | Identity & Organization | identity-and-rbac | pendiente | pendiente | identity-and-rbac | pendiente | pendiente | pendiente | MVP | planned | FR-003 autenticación local + MFA TOTP + OIDC, sesiones; ADR-011 (better-auth `[ASSUMPTION]`, pendiente de aprobación humana) |
| producto | — | product_requirement | — | PR-XC-004 | Identity & Organization | identity-and-rbac | pendiente | pendiente | identity-and-rbac | pendiente | pendiente | pendiente | MVP | planned | FR-004 12 roles, catálogo de permisos `resource.action`, autorización a nivel de objeto; AD-8; matriz por defecto addendum §B (D-6, pendiente de aprobación humana) |
| producto | — | product_requirement | — | PR-XC-005 | Identity & Organization | identity-and-rbac | pendiente | pendiente | identity-and-rbac | pendiente | pendiente | pendiente | MVP | planned | FR-005 ciclo de vida del usuario: invitación (caducidad 7 días `[ASSUMPTION D-7]`), activación, desactivación |
| producto | — | product_requirement | — | PR-NFR-004 | Identity & Organization | identity-and-rbac | pendiente | pendiente | identity-and-rbac | pendiente | pendiente | pendiente | MVP | planned | NFR-002 ningún secreto ni dato ajeno llega al cliente; pruebas de escalada de privilegios y BOLA |
| producto | — | product_requirement | — | PR-EXT-007 | Identity & Organization | identity-and-rbac | pendiente | pendiente | identity-and-rbac | pendiente | pendiente | pendiente | MVP | planned | arquitectura OIDC-ready y MFA-compatible (P2 §4); SAML diferido |

### 3.4 Cambio 4 — `immutable-audit-trail`

| source_doc | iso_requirement | record_type | iso_title | product_requirement | context | capability | epic | story | change | implementation | tests | evidence | phase | status | notes |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| producto | — | product_requirement | — | PR-XC-006 | Administration | immutable-audit-trail | pendiente | pendiente | immutable-audit-trail | pendiente | pendiente | pendiente | MVP | planned | FR-007 registro append-only con cadena de hashes por tenant, consulta, verificación y exportación; AD-6; ADR-005 |
| producto | — | product_requirement | — | PR-NFR-010 | Administration | immutable-audit-trail | pendiente | pendiente | immutable-audit-trail | pendiente | pendiente | pendiente | MVP | planned | NFR-017 toda operación de gobernanza atribuible y exportable |
| producto | — | product_requirement | — | PR-DR-017 | Administration | immutable-audit-trail | pendiente | pendiente | immutable-audit-trail | pendiente | pendiente | pendiente | MVP | planned | invariante: toda decisión de cumplimiento genera registro inmutable con actor, momento, motivo, valor anterior, nuevo y aprobador; prueba nombrada `PR-DR-017` |

### 3.5 Cambio 5 — `domain-events-outbox-jobs`

| source_doc | iso_requirement | record_type | iso_title | product_requirement | context | capability | epic | story | change | implementation | tests | evidence | phase | status | notes |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| producto | — | product_requirement | — | PR-XC-007 | Administration | domain-events-outbox-jobs | pendiente | pendiente | domain-events-outbox-jobs | pendiente | pendiente | pendiente | MVP | planned | FR-012 trabajo en segundo plano fiable: pg-boss tras `JobScheduler`; AD-5; ADR-004 |
| producto | — | product_requirement | — | PR-XC-014 | Administration | domain-events-outbox-jobs | pendiente | pendiente | domain-events-outbox-jobs | pendiente | pendiente | pendiente | MVP | planned | bus de eventos con outbox y catálogo de `DOMAIN_MAP.md` §5; AD-4; ADR-003 |
| producto | — | product_requirement | — | PR-XC-016 | Administration | domain-events-outbox-jobs | pendiente | pendiente | domain-events-outbox-jobs | pendiente | pendiente | pendiente | MVP | planned | NFR-014 correlación de extremo a extremo (`correlation_id`); AD-19 |
| producto | — | product_requirement | — | PR-XC-019 | Administration | domain-events-outbox-jobs | pendiente | pendiente | domain-events-outbox-jobs | pendiente | pendiente | pendiente | MVP | planned | NFR-012 errores útiles y uniformes (`problem+json`, códigos canónicos); AD-15 |
| producto | — | product_requirement | — | PR-XC-020 | Administration | domain-events-outbox-jobs | pendiente | pendiente | domain-events-outbox-jobs | pendiente | pendiente | pendiente | MVP | planned | FR-009 versionado e historial estándar (`version`, `*_versions`, `object_versions`); AD-11; ADR-015; `Approval` mínimo de Workflow `[ASSUMPTION]` |

Prefijos de ruta para las columnas `implementation` y `tests` cuando se completen: `pkg:` = `packages/`, `app:` = `apps/`, `t:` = `tests/`.

## 4. Brechas

Pendiente hasta la publicación de `standards-seed@1.0.0` (cambio 6). A partir de entonces esta sección listará todo requisito de las cláusulas 4.1-10.2 y todo control A.2.2-A.10.4 sin PR-* o sin cambio, con justificación o fase. Brechas ya conocidas por los insumos y que se registrarán como filas: ISO/IEC 42006 (sin contenido en el corpus, `pending_dependency`), ISO/IEC 23894 cláusulas 5.4 en adelante (`SOURCE_DETAIL_REQUIRED`).

## 5. Historial de actualizaciones

| Fecha | Cambio archivado | Filas añadidas/modificadas | Autor |
| --- | --- | --- | --- |
| 2026-09-17 | — (esqueleto inicial de arquitectura) | 3 filas `[ASSUMPTION]` en §2; 22 filas en §3 para los cambios 1-5; todas `planned` | bmad-architecture (headless), pendiente de revisión humana |
