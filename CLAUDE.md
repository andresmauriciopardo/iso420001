# CLAUDE.md — Plataforma de gobernanza de IA ISO/IEC 42001

## Propósito del repositorio
Monorepo con dos capas:
1. `resumenes/` — base de conocimiento normativa (13 resúmenes en español de ISO/IEC 42001, 23894, ISO 19011:2026, guía y plantillas). Nivel 0/1 de la jerarquía de fuentes de verdad. Solo lectura para el código; nunca se importa en tiempo de ejecución.
2. Plataforma SaaS multi-tenant para implementar, operar, monitorear y auditar un Sistema de Gestión de IA (SGIA / AIMS) según ISO/IEC 42001. Código en `apps/` y `packages/` (se crean en el cambio `foundation-project-bootstrap`).

La carpeta `Material de estudio Complementario  ISO 42001/` contiene normas con licencia y está en `.gitignore`. No la copies ni la cites textualmente.

## Método de desarrollo
- **BMAD** (`_bmad/`, skills `bmad-*`) decide y documenta el nivel PRODUCTO: brief, PRD, arquitectura, ADRs, épicas e historias. Salidas en `_bmad-output/planning-artifacts/`. Conocimiento de proyecto: `docs/`.
- **OpenSpec** (`openspec/`, skills `openspec-*`, comandos `/opsx:*`) contrata cada CAMBIO: `proposal.md` → `specs/<capacidad>/spec.md` → `design.md` → `tasks.md` → implementar → verificar → archivar. Esquema `spec-driven`.
- **Claude Code** ejecuta. **Git** preserva.
- Nunca un solo cambio gigante. Un cambio = una capacidad coherente e independientemente verificable.

## Jerarquía de fuentes de verdad (el inferior nunca sobreescribe al superior en silencio)
0. Normas (representadas por `resumenes/`) → 1. `docs/product/MASTER_ISO42001_PLATFORM_REQUIREMENTS.md` + `docs/product/PROMPT_REVIEW_AND_ADJUSTMENTS.md` → 2. Brief y PRD (BMAD) → 3. Arquitectura y ADRs → 4. OpenSpec → 5. Código → 6. Pruebas y evidencia.
Si hay conflicto: identificarlo, decidir si es cambio de requisito o defecto, corregir el artefacto superior y propagar hacia abajo.

## Reglas del dominio de cumplimiento (no negociables)
- Nunca inventar requisitos, identificadores de cláusula o control, texto normativo, obligaciones regulatorias ni capacidades de API de proveedores. Falta detalle → `SOURCE_DETAIL_REQUIRED`.
- Distinguir siempre: `normative_requirement`, `implementation_guidance`, `recommended_practice`, `organizational_decision`, `product_requirement`, `technical_design_decision`.
- Hechos verificados del corpus: Anexo A = 38 controles (A.2:3, A.3:2, A.4:5, A.5:4, A.6:9, A.7:5, A.8:4, A.9:3, A.10:3); cláusula 3 = 26 términos; Anexo C = 11 objetivos y 7 fuentes de riesgo; UNE traduce safety = protección y security = seguridad.
- La SoA gobierna los flujos: un control no aplicable no genera tareas, evidencia recurrente ni métricas activas, pero permanece visible y auditable.
- Riesgo tratado ≠ plan de tratamiento existente. Evidencia subida ≠ evidencia aceptada. La IA sugiere; una persona autorizada aprueba. La plataforma nunca declara "certificado ISO".
- Toda puntuación expone fórmula, numerador, denominador, exclusiones y registros fuente.

## Idioma
Documentos (producto, arquitectura, OpenSpec) en español. Código, identificadores, enumeraciones canónicas, API y mensajes de commit en inglés. UI bilingüe ES/EN desde la fundación. Terminología oficial: `resumenes/13_Glosario-Bilingue-ES-EN_ISO42001_y_Nota-Version-Inglesa.md`.

## Estándares de ingeniería
- Stack por defecto (ADR-001): TypeScript estricto, Next.js App Router, React, Tailwind, shadcn/ui, PostgreSQL + Prisma, Zod, pg-boss, OpenTelemetry, Vitest, Playwright, Docker, GitHub Actions.
- Multi-tenencia: `tenant_id` en toda tabla de negocio + Row Level Security (ADR-002). Monolito modular con eventos de dominio y outbox (ADR-003).
- Toda entidad importante: `id, tenant_id, created_at, created_by, updated_at, updated_by, status, version, deleted_at`.
- Registro de auditoría append-only con cadena de hashes; toda decisión de cumplimiento (aplicabilidad, aceptación de riesgo, aprobación de evidencia o política, cierre de CAPA) genera evento.
- Cambios de base de datos solo por migración, reversibles cuando sea práctico, con índices por `tenant_id`, `status`, `owner_id`, claves foráneas y `due_date`.
- Pruebas acompañan cada cambio: unitarias (scoring, transiciones, permisos), integración (BD, API), E2E (Playwright), seguridad (acceso cross-tenant, escalada de privilegios). Fixtures de dominio derivados de `resumenes/`.
- Seguridad: secretos solo vía abstracción de gestor de secretos, nunca en columnas normales ni en el frontend; validación con Zod en toda entrada; cabeceras seguras; rate limiting.
- Commits convencionales (`feat:`, `fix:`, `docs:`, `test:`, `chore:`, `security:`). Ramas `feature/<capacidad>`. No reescribir historia publicada. No commitear secretos.

## Aprobación humana obligatoria antes de
Aceptar supuestos de producto mayores; bloquear la arquitectura; migraciones irreversibles; cambios de arquitectura de seguridad; credenciales de producción; integraciones contra cuentas reales; cambiar interpretación normativa; declarar un requisito implementado; borrar o alterar evidencia; publicar (PR, push a rama compartida, despliegue); añadir dependencias con licencia copyleft.

## Cómo trabajar un cambio
1. `openspec list` para ver el estado. Leer el PRD y solo los documentos de arquitectura necesarios (no cargar el prompt maestro completo).
2. `/opsx:propose <nombre-kebab>` → revisar `proposal.md`, `specs/`, `design.md`, `tasks.md`. Revisión humana.
3. `/opsx:apply` → implementar tarea por tarea; una casilla `[x]` solo tras pruebas, lint y typecheck en verde y comportamiento verificado.
4. `openspec validate --all`; revisión BMAD (`bmad-review`, `bmad-code-review`) en capacidades significativas.
5. `/opsx:archive` → actualizar `docs/compliance/TRACEABILITY_MATRIX.md`. Siguiente cambio.
Sesión nueva para cada cambio grande.

## Prohibido
Codificar antes del checkpoint de planificación aceptado; un cambio "build-entire-platform"; UI con botones falsos; afirmar que un conector funciona sin validarlo contra el proveedor real; borrar registros de cumplimiento en silencio; pegar el prompt maestro completo en el contexto de OpenSpec o de cada tarea.
