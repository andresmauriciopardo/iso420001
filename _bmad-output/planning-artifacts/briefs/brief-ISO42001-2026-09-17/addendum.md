---
title: "Addendum al Product Brief: Plataforma SaaS de gobernanza de IA ISO/IEC 42001"
status: draft
created: 2026-09-17
updated: 2026-09-17
project: ISO42001
purpose: "Profundidad que no cabe en el brief y que consumirán el PRD, la arquitectura y la investigación de mercado."
---

# Addendum al Product Brief

Este documento conserva el detalle que respalda el brief pero no pertenece a sus dos páginas: panorama competitivo, alternativas descartadas, mapa del ciclo de producto a las fases de implementación, personas ampliadas, restricciones técnicas ya decididas y contexto de hoja de ruta aparcado. No contiene decisiones de proceso ni auditoría: eso vive en `.memlog.md`.

## A. Panorama competitivo (digest web, septiembre de 2026)

Investigación breve realizada por subagente web (11 consultas). Toda afirmación sin fuente directa lleva `[NO VERIFICADO]`; las cifras, `[ESTIMACIÓN]`. Debe sustituirse por un `bmad-deep-recon` de tipo mercado antes de decisiones de precio o posicionamiento.

### A.1 Plataformas que declaran soporte a ISO/IEC 42001

| Categoría | Plataformas | Enfoque respecto a 42001 |
|---|---|---|
| GRC de automatización de cumplimiento (origen SOC 2 / ISO 27001) | Vanta, Drata, Secureframe, Sprinto; Scrut, Hyperproof, Thoropass `[NO VERIFICADO]` | Marco de casillas con recolección automatizada de evidencia y mapeo cruzado a 27001/SOC 2; plantillas de política; poca o ninguna noción de sistema de IA, impacto o SoA operativa |
| Gobernanza de IA especializada | OneTrust AI Governance, Credo AI, Holistic AI, Trustible, Enzai, Fairly AI, Saidot | Inventario de IA, clasificación de riesgo, plantillas alineadas a EU AI Act / NIST AI RMF / ISO 42001; Credo AI ofrece un "Policy Pack" ISO 42001 que, según el propio proveedor, cubre alrededor del 45 % de la norma `[afirmación del vendedor, no verificada por terceros]`; Fairly AI destaca despliegue en nube privada |
| Suites empresariales | IBM watsonx.governance (+ OpenPages), Microsoft Purview Compliance Manager, ServiceNow IRM `[NO VERIFICADO]` | Aceleradores/plantillas de evaluación con 42001 entre muchos marcos; posicionadas como capa de datos/GRC corporativa, no como gestor integral del SGIA |

Nota sobre idioma: ningún proveedor de las tres categorías confirmó UI en español específica de 42001 `[NO VERIFICADO]`; las suites empresariales ofrecen localización corporativa genérica.

### A.2 Tendencias

- **Adopción**: aproximadamente el 23 % de las pymes tecnológicas europeas planearía certificarse antes de 2027 `[ESTIMACIÓN, fuente terciaria]`.
- **EU AI Act**: obligaciones de transparencia (art. 50) aplicables desde agosto de 2026; obligaciones de alto riesgo del Anexo III pospuestas a diciembre de 2027 y del Anexo I a agosto de 2028 según el paquete "Digital Omnibus" `[NO VERIFICADO: confirmar con el texto consolidado]`. La norma 42001 no equivale al AI Act, pero es el marco de gestión que el mercado usa para demostrar gobernanza.
- **ISO/IEC 42006:2025** ya está publicada y fija requisitos para los organismos que auditan y certifican SGIA; hay organismos acreditados (ANAB y certificadores como Armanino o ControlCase citados) `[NO VERIFICADO]`. No está en el corpus del proyecto: dependencia pendiente.
- **Mercado hispanohablante**: grandes bancos (Bancolombia, BBVA, Santander) habrían iniciado auditorías de modelos de IA `[NO VERIFICADO]`; escasez de auditores 42001 formados en la región. La UNE-ISO/IEC 42001:2025 es la única traducción oficial; ningún proveedor confirmó UI completa en español.

### A.3 Vacíos observados (base de la diferenciación)

Ninguna plataforma revisada: (1) opera el SGIA activando/desactivando flujos desde la SoA con justificación de exclusiones; (2) automatiza la revisión por la dirección con sus entradas 9.3.2; (3) modela la auditoría interna conforme a ISO 19011; (4) ofrece trazabilidad navegable de ida y vuelta entre cláusulas 4-10, controles, riesgos, sistemas, evidencia, hallazgos y CAPA; (5) ofrece modo consultora multi-cliente; (6) tiene UI bilingüe ES/EN nativa confirmada. Lo que sí hacen bien y debemos igualar en fases posteriores: recolección automatizada de evidencia vía conectores y mapeo cruzado con ISO 27001.

### A.4 Fuentes principales

credo.ai/responsible-ai/iso-42001 · onetrust.com/solutions/ai-governance · trustible.ai/iso-iec-42001 · iso.org/standard/42006 · consilium.europa.eu (comunicado de mayo de 2026 sobre el Digital Omnibus) · ibm.com/products/watsonx-governance · learn.microsoft.com/compliance/regulatory/offering-iso-42001 · gcerti.org/iso-42001.

## B. Alternativas consideradas y descartadas

| Alternativa | Por qué se descarta (fuente) |
|---|---|
| Construir la plataforma completa de 73 secciones en un solo programa | Programa de 12-24 meses sin valor entregable intermedio; contradice P1 §36 (ajuste A1) |
| Un "checklist ISO 42001" ligero para llegar antes al mercado | Contradice el principio rector §70 y elimina la diferenciación; ya lo ofrecen las GRC generalistas |
| Esquema de base de datos por tenant | Descartado en el MVP a favor de `tenant_id` + Row Level Security (ADR-002, ajuste A8) |
| Microservicios desde el inicio | Monolito modular con eventos de dominio y outbox (ADR-003) |
| Redis/BullMQ como cola inicial | pg-boss sobre PostgreSQL para evitar dependencia temprana; interfaz de jobs abstracta (ADR-004) |
| Importar `resumenes/` en tiempo de ejecución | Semilla estructurada versionada (`packages/standards-seed/`) revisada por humanos; ningún módulo lee `resumenes/` (ajuste A4) |
| Conectores reales Azure/OpenAI/Anthropic en el MVP | SDK de conectores + adaptador mock etiquetado; nunca afirmar integración validada sin credenciales reales (A1, A9) |
| UI solo en inglés con traducción posterior | i18n ES/EN es NFR de fase 1 (A5) |
| Marketplace de consultoras `[SUPUESTO]` | Fuera del núcleo; se revisita si el canal consultora se confirma |

## C. Mapa del ciclo de producto a las fases de implementación

Las fases de P1 son la referencia (ajuste A2); aquí se muestra la correspondencia indicativa con las fases de P2 §64 y el ciclo del brief.

| Etapa del ciclo | Fases P2 §64 | Capacidades OpenSpec previstas (indicativo) | MVP |
|---|---|---|---|
| (Fundación) | 0 Arquitectura, 1 Fundación, 2 Motor de normas | foundation-project-bootstrap, tenancy-and-auth, audit-trail-hash-chain, standards-seed | Sí |
| Discover | 3 AIMS Core (inventario), 8 Integraciones | ai-inventory, ai-asset-graph; connectors-sdk (mock) | Inventario sí; descubrimiento automático no |
| Govern | 3 AIMS Core | organization-context, interested-parties, aims-scope, ai-policy-and-roles, ai-objectives | Sí |
| Assess | 4 Riesgo / Impacto | risk-criteria-and-assessment, risk-treatment-and-acceptance, ai-impact-assessment | Sí |
| Control | 5 SoA / Controles | statement-of-applicability, control-operating-model, soa-event-activation | Sí |
| Evidence | 7 Evidencia / Documentos | evidence-lifecycle-and-provenance, document-control | Sí (manual + API interna) |
| Monitor | 6 Operaciones, 10 Reporting | transparent-scoring, essential-reports, notifications | Puntuaciones e informes esenciales sí; constructor dinámico no |
| Audit | 9 Auditoría / CAPA | audit-programme-19011, findings-nonconformity-capa | Sí |
| Improve | 11 Revisión por la dirección / Preparación certificación | management-review, certification-readiness-workspace | Sí |
| (Endurecimiento) | 12 Hardening | security-tests, performance, observability | Sí |

## D. Personas ampliadas

- **Oficial de gobernanza de IA / Compliance Manager (usuaria diaria).** Coordina a propietarios de sistemas y controles, persigue evidencia vencida, prepara la auditoría interna y la revisión por la dirección. Necesita "por qué se exige esto" (P2 §67) en cada requisito, tareas generadas por la SoA y un panel honesto. Dolor actual: reconciliar Excel de riesgos, SoA en Word y evidencia en SharePoint.
- **Propietario de sistema de IA / modelo / datos (usuario ocasional).** Responde a tareas concretas: ficha del sistema, evaluación de impacto, evidencia de su control. Necesita formularios guiados, bilingües, sin jerga normativa innecesaria.
- **Auditor interno (usuario por ciclos).** Planifica el programa 9.2, muestrea evidencia, registra hallazgos con criterio, evidencia y conclusión (ISO 19011), y verifica eficacia de CAPA. Necesita independencia de rol y trazabilidad.
- **Auditor externo (lector).** Acceso de solo lectura al paquete de evidencia y a la SoA; nunca ve datos de otros tenants. Valora hash y procedencia.
- **Consultor implementador `[SUPUESTO]`.** Opera varios clientes; quiere plantillas propias, comparar avance y exportar entregables con su marca. Riesgo: si el producto le sustituye, no lo adopta; el posicionamiento debe ser "herramienta del consultor".
- **Formador / alumno PECB `[SUPUESTO]`.** Organización demo con datos de ejemplo (P2 §48-49) como entorno de práctica Lead Implementer / Lead Auditor.
- **Dirección.** Firma política, alcance, aceptación de riesgo residual y revisión por la dirección; quiere tablero con vocabulario prescrito y un botón "aprobar" que deje huella.

## E. Restricciones técnicas ya decididas (referencia para arquitectura)

Aviso para quien redacte cambios OpenSpec: el campo `context` admite como máximo 51.200 bytes; nunca se pega el prompt maestro completo, se enlaza esta sección o el ADR correspondiente.

### E.1 Decisiones de arquitectura (ADR)

- ADR-001 stack por defecto: TypeScript estricto, Next.js App Router, React, Tailwind, shadcn/ui, PostgreSQL + Prisma, Zod, pg-boss, OpenTelemetry, Vitest, Playwright, Docker, GitHub Actions.
- ADR-002 multi-tenencia: `tenant_id` en toda tabla de negocio + RLS; almacenamiento de objetos por prefijo de tenant.
- ADR-003 monolito modular con bounded contexts, eventos de dominio internos y patrón outbox.
- ADR-004 colas sobre PostgreSQL (pg-boss) con interfaz de jobs abstracta.
- ADR-005 registro de auditoría append-only con cadena de hashes por tenant.

### E.2 Modelo de datos

- Entidades importantes: `id, tenant_id, created_at, created_by, updated_at, updated_by, status, version, deleted_at`.
- Enumeraciones canónicas en inglés (p. ej. aplicabilidad: Applicable, Not Applicable, Applicable with Alternative Control, Pending Assessment; evidencia: Requested, Missing, Collected, Under Review, Accepted, Rejected, Expired, Superseded).

### E.3 Motor de normas y semilla

- Modelo `StandardVersion` con ISO/IEC 42001:2023 + Amd 1:2024, UNE-ISO/IEC 42001:2025 (adopción idéntica), ISO 19011:2026, ISO/IEC 23894:2023 (parcial: cláusulas 1-4 y 5.1-5.3 en el corpus, resto `SOURCE_DETAIL_REQUIRED`); ISO/IEC 42006 como dependencia pendiente.
- Semilla: campos `identificador, título, resumen_del_requisito, tipo, evidencia_esperada, referencias (resumenes/NN, sección)`.
- Pruebas de dominio: 38 controles con distribución A.2:3, A.3:2, A.4:5, A.5:4, A.6:9, A.7:5, A.8:4, A.9:3, A.10:3; 26 términos en cláusula 3; Anexo C con 11 objetivos y 7 fuentes de riesgo.

### E.4 Requisitos no funcionales

- WCAG 2.2 AA; residencia de datos por tenant; RPO/RTO, copias y retención legal a fijar en el PRD.

### E.5 IA del propio producto

- Abstracción de proveedor LLM; presupuesto y registro de tokens por tenant; funciones LLM inventariadas en el tenant plataforma y sujetas a A.6/A.8/A.9.

## F. Medición: definiciones operativas

- **Flujo §69 completado**: todos los pasos de la cadena de aceptación final salvo "Connect Azure/OpenAI/Anthropic" y "Discover AI Assets" (diferidos), con registro persistente y aprobación humana en cada punto de decisión (alcance, política, aceptación de riesgo, SoA, evidencia, cierre de CAPA, revisión por la dirección).
- **Procedencia completa**: origen, método de recogida, recogido por, fecha, hash, versión, revisor y fecha de revisión; para evidencia automatizada, además conector, endpoint, ID de objeto fuente, transformación y versión del recolector.
- **Puntuación transparente**: la UI y la API exponen fórmula, numerador, denominador, exclusiones, registros fuente y marca de tiempo; vocabulario permitido: preparación, cobertura, implementación, eficacia, exposición.

## G. Contexto de hoja de ruta aparcado (post-MVP)

Cada elemento entra como capacidad OpenSpec independiente tras aceptación del checkpoint:

- Conectores reales y descubrimiento automático (P2 §16-18).
- Gobierno de agentes como sistemas de IA de primera clase: permisos de herramientas, límites de gasto, *kill switch* (§50).
- Modelo de madurez (§51).
- Programación de informes (§52).
- Importación/exportación masiva (§54).
- Motor de calidad de datos (§55).
- Motor de mejora continua (§56).
- Paquetes sectoriales (§57).
- Búsqueda semántica (§31).
- Constructor dinámico de informes y catálogo de 25 informes (§27-28).
- Requisitos regulatorios externos mapeados (§35), con el AI Act como primer candidato.
- Formación y competencia (§36).
