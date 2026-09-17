# ISO/IEC 42001 — Base de conocimiento y plataforma de gobernanza de IA

Monorepo con dos capas:

| Capa | Ubicación | Estado |
|---|---|---|
| Base de conocimiento normativa (13 resúmenes en español de ISO/IEC 42001, ISO/IEC 23894, ISO 19011:2026, guía de implementación, catálogo PECB y 5 plantillas Excel), pensada para personas y para IA (RAG) | `resumenes/` (empieza por `00_INDICE.md`) | Completa, en revisión de calidad |
| Plataforma SaaS multi-tenant para implementar, operar y auditar un Sistema de Gestión de IA (SGIA / AIMS) | `docs/`, `_bmad-output/`, `openspec/`; código futuro en `apps/` y `packages/` | Fase de planificación (BMAD + OpenSpec) |

## Método de desarrollo
- **BMAD** (`_bmad/`, skills `bmad-*`): nivel producto (brief, PRD, arquitectura, ADRs, épicas, historias).
- **OpenSpec** (`openspec/`, comandos `/opsx:*`): nivel cambio (proposal → specs → design → tasks → apply → archive).
- **Claude Code** ejecuta; **Git** preserva. Reglas completas en [CLAUDE.md](CLAUDE.md).

## Documentos clave
- Línea base de requisitos: [docs/product/MASTER_ISO42001_PLATFORM_REQUIREMENTS.md](docs/product/MASTER_ISO42001_PLATFORM_REQUIREMENTS.md)
- Procedimiento de orquestación: [docs/product/MASTER_ORCHESTRATION_BMAD_OPENSPEC.md](docs/product/MASTER_ORCHESTRATION_BMAD_OPENSPEC.md)
- Revisión de los prompts y ajustes adoptados (A1-A13): [docs/product/PROMPT_REVIEW_AND_ADJUSTMENTS.md](docs/product/PROMPT_REVIEW_AND_ADJUSTMENTS.md)
- Análisis de requisitos, alcance, glosario, mapa de dominio y modelo de trazabilidad: `docs/product/`
- Artefactos BMAD (brief, PRD, arquitectura, épicas): `_bmad-output/planning-artifacts/`

## Material fuente
La carpeta `Material de estudio Complementario  ISO 42001/` contiene normas ISO/UNE con licencia de usuario único y está excluida del repositorio. Los resúmenes son paráfrasis; no reproducen texto normativo.

## Requisitos de entorno
Node.js >= 20.19 (probado con 22.16), `uv` (scripts de BMAD), Git. BMAD v6.12.0 y OpenSpec 1.13.1 se instalan con `npx bmad-method install` y `npx @fission-ai/openspec init`.
