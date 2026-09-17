---
titulo: "Revisión de los prompts maestros y ajustes adoptados"
fecha: "2026-09-17"
autor: "Claude (orquestador) para Andrés Mauricio Pardo"
estado: "vigente"
documentos_revisados:
  - docs/product/MASTER_ORCHESTRATION_BMAD_OPENSPEC.md (Prompt 1: BMAD + OpenSpec + Claude Code)
  - docs/product/MASTER_ISO42001_PLATFORM_REQUIREMENTS.md (Prompt 2: plataforma ISO/IEC 42001)
---

# Revisión de los prompts maestros y ajustes adoptados

Este documento registra la opinión técnica sobre los dos prompts maestros y los ajustes que se aplican al
ejecutarlos. Tiene rango de **decisión organizacional** (nivel 1.5 en la jerarquía de fuentes de verdad):
complementa al Prompt 2 y lo corrige donde se indica; no altera ningún requisito normativo de ISO/IEC 42001.

## 1. Valoración general

| Prompt | Valoración | Resumen |
|---|---|---|
| Prompt 1 (orquestación BMAD + OpenSpec) | 8/10 | Método sólido: separa nivel producto (BMAD) de nivel cambio (OpenSpec), impone planificación antes de código, jerarquía de fuentes de verdad, puntos de aprobación humana y checkpoint antes de implementar. Bien escrito para tolerar cambios de versión de las herramientas. |
| Prompt 2 (requisitos de la plataforma) | 7/10 | Visión de producto excelente y completa ("sistema operativo de gobernanza de IA", la SoA gobierna los flujos, evidencia con procedencia, trazabilidad extremo a extremo). Su debilidad es el tamaño: 73 secciones, 25 informes, 3 integraciones, agentes, madurez, paquetes sectoriales, sin priorizar. Ejecutado literalmente es un programa de 12 a 24 meses para un equipo, no una tarea de una sesión. |

**Conclusión**: los prompts son compatibles y se implementan tal como piden, con Prompt 1 como procedimiento
de arranque y Prompt 2 como línea base de requisitos. Los ajustes siguientes resuelven contradicciones,
cubren vacíos y acotan el primer incremento.

## 2. Fortalezas que se conservan sin cambios

1. Separación de niveles: BMAD decide qué y cómo a nivel producto; OpenSpec contrata cada cambio; Claude Code ejecuta; Git preserva.
2. Jerarquía de fuentes de verdad (nivel 0 normas → nivel 6 pruebas) y regla "el código nunca sobreescribe la especificación en silencio".
3. Prohibición de fabricar requisitos, identificadores de controles o texto normativo; placeholders `SOURCE_DETAIL_REQUIRED`.
4. Puntos de aprobación humana (arquitectura, migraciones irreversibles, credenciales reales, interpretación normativa, declarar un requisito implementado).
5. Reglas de dominio de cumplimiento: la SoA activa o desactiva flujos; el riesgo no está tratado por tener plan; la evidencia no se acepta por subirse; la plataforma nunca declara "certificado".
6. Puntuaciones transparentes (fórmula, numerador, denominador, exclusiones) y vocabulario "preparación, cobertura, implementación, eficacia, exposición".
7. La IA sugiere, el humano aprueba; cada artefacto generado por IA lleva modelo, hora, fuente, revisor.

## 3. Problemas detectados y ajustes adoptados

Cada ajuste indica dónde se materializa (documento, ADR o configuración).

### A1. Contradicción sobre alcance: "no reduzcas el alcance" (P2 §73) frente a "no planifiques para siempre" (P1 §36)
- **Ajuste**: se mantiene la **arquitectura de dominio completa** desde el inicio (todos los bounded contexts, modelo de datos y eventos) pero se define un **MVP vertical** que llegue a una organización real capaz de operar un SGIA mínimo: fundación multi-tenant → motor de normas y requisitos (cláusulas 4-10 y 38 controles) → contexto y alcance → inventario de IA → riesgo e impacto → SoA y controles → evidencia → auditoría interna, hallazgos y CAPA → revisión por la dirección → informes esenciales (SoA, cumplimiento por cláusula, registro de riesgos, preparación para auditoría).
- Se difieren a fases posteriores: conectores Azure AI Foundry / OpenAI / Anthropic (se implementa primero el SDK de conectores con adaptador mock), constructor dinámico de informes, búsqueda semántica, paquetes sectoriales, modelo de madurez, gobierno de agentes de IA y los 25 informes completos.
- **Dónde**: `docs/product/PRODUCT_SCOPE.md`, PRD (sección de fuera de alcance del MVP), roadmap unificado.

### A2. Dos listas de fases incompatibles (P1 §17 con 16 fases; P2 §64 con 13 fases)
- **Ajuste**: una única hoja de ruta en el PRD y en `docs/product/ROADMAP.md`. Las fases de P1 son la referencia; las de P2 se mapean a ellas.

### A3. Dos layouts de documentación incompatibles (P1 §9 `docs/…`; P2 §58 quince archivos en raíz más `docs/requirements`, `docs/database`, `docs/api`)
- **Ajuste**: **un solo árbol `docs/`** (product, architecture, decisions, compliance, domain, integrations, security, operations, testing). En la raíz solo `README.md` y `CLAUDE.md`. Los archivos que P2 pide en raíz (ARCHITECTURE.md, SECURITY.md, DATA_MODEL.md, API.md, …) viven en la subcarpeta correspondiente de `docs/`.

### A4. Ubicación de la plataforma respecto al repositorio actual
- El repositorio es hoy una base de conocimiento (`resumenes/` con 13 resúmenes de las normas y plantillas). P2 §1 asume que la plataforma vive en el mismo repositorio y usa esos resúmenes como contexto.
- **Ajuste**: monorepo con dos capas claramente separadas: `resumenes/` (conocimiento normativo, nivel 0/1, solo lectura para el código) y `apps/` + `packages/` (plataforma). Ningún módulo de la aplicación importa texto de `resumenes/` en tiempo de ejecución; la semilla (`seed`) se genera desde un dataset estructurado versionado (`packages/standards-seed/`) construido a partir de los resúmenes y revisado por humanos.
- La carpeta `Material de estudio Complementario  ISO 42001/` sigue fuera de Git (licencias ISO/UNE de usuario único).

### A5. Idioma y bilingüismo (ambos prompts en inglés, usuario, resúmenes y equipo en español; la norma se usa en ES y EN)
- **Ajuste**: documentos de producto, arquitectura y OpenSpec en **español**; identificadores de código, nombres de entidades, enumeraciones canónicas, mensajes de commit y API en **inglés**. La UI es **bilingüe ES/EN desde la fundación** (i18n es NFR de fase 1, no mejora posterior). El glosario bilingüe (`resumenes/13_…`) es la fuente de la terminología oficial: `safety` = protección, `security` = seguridad, `accountability` = rendición de cuentas, `statement of applicability` = declaración de aplicabilidad.
- **Dónde**: `CLAUDE.md`, `openspec/config.yaml`, configuración BMAD (`document_output_language: Spanish`), NFR de i18n en el PRD, `docs/product/GLOSSARY.md`.

### A6. Derechos de autor del texto normativo
- P2 §1.8 lo menciona, pero no dice cómo. **Ajuste**: la semilla de normas contiene solo `identificador`, `título`, `resumen_del_requisito` (paráfrasis propia, tomada de `resumenes/`), `tipo` (`normative_requirement`, `implementation_guidance`, `recommended_practice`, `evidence_example`), `evidencia_esperada`, `referencias`. Nunca texto literal de ISO, IEC o UNE. Cada registro cita el archivo de `resumenes/` y la sección de la que proviene (trazabilidad nivel 0 → nivel 1 → semilla).

### A7. Versionado normativo insuficiente en P2
- **Ajuste**: el modelo `StandardVersion` debe soportar desde el inicio: ISO/IEC 42001:2023 + Amd 1:2024 (frases sobre cambio climático en 4.1 y 4.2, presentes en las tres versiones del corpus), UNE-ISO/IEC 42001:2025 como adopción idéntica, ISO 19011:2026 (cuarta edición, sustituye a 2018), ISO/IEC 23894:2023 (en el corpus solo está disponible el 23 % del cuerpo técnico: cláusulas 1-4, 5.1-5.3; el resto lleva `SOURCE_DETAIL_REQUIRED`). ISO/IEC 42006 (requisitos para organismos que auditan SGIA) no está en el corpus; se registra como dependencia pendiente para el módulo de auditoría de tercera parte.

### A8. Decisiones técnicas que P2 deja abiertas y conviene fijar por ADR antes de codificar
- Multi-tenencia: PostgreSQL compartido con `tenant_id` en toda tabla y **Row Level Security** obligatoria, más aislamiento de almacenamiento de objetos por prefijo de tenant (ADR-002). Se descarta esquema-por-tenant en el MVP.
- Estilo arquitectónico: **monolito modular** con bounded contexts como módulos con interfaces explícitas y **eventos de dominio internos con patrón outbox** (ADR-003). No microservicios en el MVP.
- Colas y trabajos: `pg-boss` (colas sobre PostgreSQL) en el MVP para evitar Redis como dependencia temprana; interfaz de jobs abstracta para migrar a BullMQ/Redis si hace falta (ADR-004).
- Registro de auditoría: tabla append-only con **cadena de hashes** por tenant (hash del registro anterior incluido en el siguiente) para evidencia inviolable (ADR-005). "Immutable" en P2 se interpreta así.
- Stack por defecto (si el repositorio sigue vacío de código, como ahora): TypeScript, Next.js (App Router), React, Tailwind, shadcn/ui, PostgreSQL, Prisma, Zod, pg-boss, OpenTelemetry, Docker, Vitest + Playwright, GitHub Actions (ADR-001). La arquitectura puede modificarlo con justificación.

### A9. Vacíos de P2 que se añaden como requisitos
- **La plataforma es a su vez un sistema de IA**: sus funciones asistidas por LLM (generación de políticas, sugerencia de riesgos, análisis de evidencia) deben registrarse en el propio inventario de IA del tenant plataforma y quedar sujetas a los controles A.6 (ciclo de vida), A.8 (información a partes interesadas) y A.9 (uso responsable). Es el mejor demo posible del producto.
- Accesibilidad **WCAG 2.2 AA** explícita.
- Residencia de datos por tenant (región), copias de seguridad y recuperación ante desastres, retención legal (`legal hold`) ya nombrada en §42 pero sin RPO/RPO; el PRD los fija.
- Coste y límites de uso de los proveedores de LLM (presupuesto por tenant, caché, registro de tokens) y abstracción de proveedor (sin acoplar el dominio a un proveedor).
- Pruebas de dominio de cumplimiento con fixtures derivados de `resumenes/`: la semilla debe pasar pruebas como "existen exactamente 38 controles con la distribución A.2:3, A.3:2, A.4:5, A.5:4, A.6:9, A.7:5, A.8:4, A.9:3, A.10:3" y "la cláusula 3 tiene 26 términos".
- Pruebas de contrato para conectores con adaptador mock etiquetado; nunca se afirma integración validada sin credenciales reales.

### A10. Trazabilidad: añadir el nodo "resumen fuente"
- La matriz de P1 §16 va de "ISO Requirement" a "Evidence". **Ajuste**: se antepone el nodo `Documento fuente (resumenes/NN, sección)` para que cada requisito sembrado sea auditable hasta el material original sin distribuir la norma.

### A11. Herramientas: verificación frente a lo instalado (2026-09-17)
- BMAD v6.12.0 instala 29 skills en `.claude/skills/bmad-*` (no existe `.bmad-core`); los flujos clave (`bmad-product-brief`, `bmad-prd`, `bmad-architecture`, `bmad-create-epics-and-stories`) tienen **modo headless** y registran decisiones en `.memlog.md`. Se configuró `communication_language` y `document_output_language` en español, `project_knowledge = docs/`, salidas en `_bmad-output/`.
- OpenSpec 1.13.1 crea `openspec/{specs,changes,config.yaml}` y 6 skills `openspec-*` con comandos `/opsx:*`. El esquema por defecto es `spec-driven` (proposal → specs → design → tasks). Límite documentado del campo `context`: 51.200 bytes (confirma P1 §8).
- El flag `--yes` de BMAD exige `--tools`; ambas instalaciones se hicieron de forma no interactiva. Node 22.16 cumple el mínimo 20.19.

### A12. Puntos de aprobación humana: se añaden dos
- Antes de **publicar** (crear PR, push a rama compartida, desplegar) y antes de **añadir dependencias con licencia copyleft o de terceros no auditados**.

### A13. Coste de ejecución del propio método
- El procedimiento de arranque de P1 (brief, PRD, arquitectura, ADRs, épicas, historias, cambios OpenSpec) genera decenas de miles de palabras. Se ejecuta con agentes especializados en paralelo donde no hay dependencia, y se detiene en el **checkpoint I** (informe de estado) antes de implementar, tal como exige P1 §40. La implementación del primer cambio (`foundation-project-bootstrap`) solo empieza tras aceptación humana del checkpoint.

## 4. Lo que NO se cambia aunque podría discutirse

- Se mantiene BMAD + OpenSpec aunque para un equipo pequeño añade ceremonia: el producto es en sí un sistema de gobernanza y la trazabilidad del proceso de desarrollo es un argumento de venta ante auditores.
- Se mantienen las enumeraciones de estado que propone P2 (por ejemplo, ciclo de vida de control: Not Applicable → … → Retired) como valores canónicos en inglés; la traducción es cosa de la capa i18n.

## 5. Efecto sobre la jerarquía de fuentes de verdad

```
Nivel 0   Normas (PDF con licencia, fuera de Git) → representadas por resumenes/01-13 (paráfrasis)
Nivel 1   docs/product/MASTER_ISO42001_PLATFORM_REQUIREMENTS.md  +  este documento (ajustes A1-A13)
Nivel 2   BMAD: product brief, PRD, análisis de requisitos, alcance, glosario, mapa de dominio
Nivel 3   BMAD: arquitectura + ADRs
Nivel 4   OpenSpec: specs y cambios
Nivel 5   Código
Nivel 6   Pruebas y evidencia generada
```
