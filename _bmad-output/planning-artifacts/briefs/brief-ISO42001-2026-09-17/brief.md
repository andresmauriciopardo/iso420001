---
title: "Product Brief: Plataforma SaaS de gobernanza de IA ISO/IEC 42001 (nombre comercial por definir)"
status: draft
created: 2026-09-17
updated: 2026-09-17
project: ISO42001
author: "Andrés Mauricio Pardo (promotor) — redactado en modo headless por bmad-product-brief"
language: es
sources_level_1:
  - docs/product/PROMPT_REVIEW_AND_ADJUSTMENTS.md (ajustes A1-A13; prevalece)
  - docs/product/MASTER_ISO42001_PLATFORM_REQUIREMENTS.md (§0-3, 13, 15, 39, 53, 69, 70, 73)
  - CLAUDE.md
  - resumenes/01 (§1, §7), resumenes/06 (§1, §4.1)
---

# Product Brief: Plataforma SaaS de gobernanza de IA ISO/IEC 42001

> Documento de nivel 2 en la jerarquía de fuentes de verdad. Redactado sin usuario disponible: cada inferencia no respaldada por las fuentes de nivel 1 lleva la etiqueta `[SUPUESTO]`; cada dato externo no confirmado, `[NO VERIFICADO]`; toda cifra, `[ESTIMACIÓN]`. El detalle que no cabe aquí está en `addendum.md`; las decisiones, en `.memlog.md`.

## 1. Resumen ejecutivo

ISO/IEC 42001:2023 es la primera norma internacional certificable de sistema de gestión de la inteligencia artificial (SGIA / AIMS). Obliga a la organización a documentar su contexto y alcance, inventariar sus sistemas de IA, evaluar riesgos e impactos, seleccionar controles del Anexo A mediante una Declaración de Aplicabilidad (SoA), producir evidencia, auditarse internamente y revisar el sistema desde la dirección. Hoy la mayoría de las organizaciones hispanohablantes que lo intentan `[SUPUESTO]` lo hacen con hojas de cálculo, plantillas de consultora y carpetas compartidas, o con plataformas GRC anglófonas que tratan la norma como una lista de casillas.

Proponemos una **plataforma SaaS multi-tenant que opera el SGIA** en lugar de describirlo: la SoA activa o desactiva flujos, cada evidencia lleva procedencia verificable, y cualquier registro puede recorrerse de ida y vuelta por la cadena *requisito → control → riesgo → sistema de IA → evidencia → auditoría → hallazgo → CAPA → revisión por la dirección*. Es bilingüe ES/EN desde la fundación, con terminología oficial UNE.

El momento es ahora: la norma se publicó en diciembre de 2023, su adopción española idéntica (UNE-ISO/IEC 42001:2025) es reciente, y la regulación (AI Act de la UE y marcos emergentes en Latinoamérica) empuja a clientes, licitaciones y socios a exigir evidencia objetiva de gobernanza de IA. Producto empresarial B2B para lanzamiento comercial, promovido por Andrés Mauricio Pardo (Micronotes / Eligen).

## 2. Problema y oportunidad

**El dolor.** Implementar y sostener un SGIA exige coordinar decenas de artefactos vivos: alcance, política de IA, tres procesos recurrentes (evaluación del riesgo, tratamiento, evaluación del impacto), 38 controles del Anexo A con justificación de aplicabilidad, plan de tratamiento aprobado, aceptación de riesgos residuales, evidencia por control, programa de auditoría interna, no conformidades con causa raíz y una revisión por la dirección con cinco entradas obligatorias. Quien lo intenta a mano vive tres fallos típicos, documentados en el corpus (resumenes/01 §7):

1. **Desconexión.** El riesgo dice una cosa, la SoA otra y la evidencia no se enlaza con ninguna; el auditor no puede seguir el hilo.
2. **Falsa seguridad.** Se confunde "tener plan de tratamiento" con "riesgo tratado" y "evidencia subida" con "evidencia aceptada". Los tableros de porcentaje verde ocultan que nadie aprobó nada.
3. **Deterioro.** Las evaluaciones no se repiten ante cambios significativos (reentrenamiento, nuevo uso, nueva jurisdicción); el sistema envejece y la recertificación se convierte en un proyecto de rescate.

**Cómo se afrontan hoy.** Plantillas Excel/Word de consultoras y academias (el propio corpus contiene cinco), SharePoint, tickets sueltos, o plataformas GRC generalistas que añadieron "ISO 42001" como un marco más de casillas, sin modelo de sistema de IA, sin evaluación de impacto y sin español.

**La oportunidad.** Un producto vertical que entienda la norma como sistema operativo y no como lista, hable el idioma de los implementadores hispanohablantes y sea defendible ante un auditor de certificación. `[SUPUESTO]` El mercado hispanohablante (España, México, Colombia, Chile, Perú, Argentina) está desatendido por la oferta actual, mayoritariamente anglófona; la existencia de un ecosistema de formación PECB/Impulsa360 en español indica demanda de implementadores que necesitan herramienta. No se aportan cifras de tamaño de mercado: quedan como pregunta abierta para investigación (`bmad-deep-recon`).

## 3. Visión

> **"Software que opera el sistema de gestión de IA de la organización y produce continuamente la evidencia de que funciona"** — no "software que me dice si marqué la casilla" (principio rector, MASTER §70).

El producto se organiza como un ciclo continuo alineado con la estructura armonizada (cláusulas 4-10) y el PDCA:

| Etapa | Qué hace la plataforma | Cláusulas / anexo |
|---|---|---|
| **Discover** | Inventaria sistemas, modelos, datos, proveedores y agentes de IA; determina el rol de la organización en cada uno; (fase posterior) descubre activos desde plataformas conectadas. | 4.1, A.4, A.10 |
| **Govern** | Contexto, partes interesadas, alcance aprobado, política de IA enlazada a otras políticas, roles y responsabilidades, objetivos medibles. | 4.2-4.4, 5, 6.2 |
| **Assess** | Evaluación del riesgo de la IA y evaluación del impacto sobre personas y sociedad, repetibles y comparables, con aceptación formal de riesgo residual. | 6.1.2, 6.1.4, 8.2, 8.4, A.5 |
| **Control** | SoA que compara con los 38 controles del Anexo A, exige justificación de exclusiones y **activa solo los flujos de los controles aplicables**. | 6.1.3, Anexo A/B |
| **Evidence** | Evidencia con ciclo de vida (solicitada → recogida → en revisión → aceptada/rechazada → vencida), hash, procedencia y revisor; la IA analiza, una persona aprueba. | 7.5, 8.1, 8.3 |
| **Monitor** | Indicadores transparentes (fórmula, numerador, denominador, exclusiones, registros fuente) y alertas por vencimiento y cambio significativo. | 9.1 |
| **Audit** | Programa y auditorías internas alineadas con ISO 19011:2026, hallazgos, no conformidades y CAPA con verificación de eficacia. | 9.2, 10.2 |
| **Improve** | Revisión por la dirección con sus entradas y salidas obligatorias, decisiones trazadas y realimentación al contexto y a los riesgos. | 9.3, 10.1 |

En tres años, si funciona, la plataforma es el lugar donde una organización o su consultora **vive** su gobernanza de IA: conectada a sus proveedores de modelos, con paquetes sectoriales, gobierno de agentes autónomos, modelo de madurez y paquete de evidencia listo para el organismo de certificación. Nunca declarará a nadie "certificado": eso lo hace el organismo externo.

## 4. A quién sirve

| Segmento | Roles típicos | Qué necesita | Qué es el éxito para ellos |
|---|---|---|---|
| **Organización que provee o usa IA** (primaria) | CIO, CISO, Chief AI Officer, oficial de cumplimiento, gestor de riesgos, propietarios de sistemas/modelos/datos | Un solo lugar para operar el SGIA, saber qué falta y por qué se exige, y repartir trabajo entre áreas | Pasar la auditoría de certificación (etapas 1 y 2) sin proyecto de rescate; sostener el sistema entre ciclos |
| **Auditor interno y externo** | Auditor líder, equipo auditor | Trazabilidad de ida y vuelta, evidencia con procedencia, sin tener que pedir capturas por correo | Muestreo eficiente, hallazgos objetivos, informe defendible |
| **Consultora implementadora** `[SUPUESTO]` | Consultor/a líder ISO 42001, a menudo también 27001/9001 | Operar varios clientes con plantillas propias, reutilizar metodología y mostrar avance a cada cliente | Reducir horas de gestión documental por proyecto; diferenciarse frente a competidores con plantillas |
| **Organismo de formación (PECB, academias)** `[SUPUESTO]` | Formador, alumno Lead Implementer / Lead Auditor | Entorno de práctica con organización demo y datos de ejemplo | Formación práctica, canal de adopción del producto |
| **Dirección y comité** | CEO, consejo, comité de ética/IA | Tablero honesto con vocabulario prescrito (preparación, cobertura, exposición) y la revisión por la dirección lista para firmar | Decidir con información completa y evidencia de haberlo hecho |

## 5. Solución, propuesta de valor y diferenciación

**Qué es.** Aplicación web SaaS multi-tenant (escritorio primero, responsive), con API tipada, tareas en segundo plano y almacenamiento de evidencia; monolito modular con eventos de dominio. El detalle técnico está fijado por ADR-001…005 y no se repite aquí.

**Propuesta de valor.** "Implemente, opere y demuestre su SGIA ISO/IEC 42001 en un solo sistema, en español e inglés, con evidencia que un auditor puede seguir hasta el requisito."

**Lo que nos hace distintos (honesto: es ejecución y enfoque, no una patente):**

1. **La SoA gobierna los flujos.** Un control no aplicable no genera tareas, evidencia recurrente ni métricas; sigue visible y auditable con su justificación. Cambiar la aplicabilidad dispara eventos que activan o retiran flujos. Las herramientas de casillas no hacen esto.
2. **Trazabilidad de extremo a extremo, navegable en ambos sentidos**, incluido el nodo "documento fuente (resumenes/NN, sección)" para que cada requisito sembrado sea auditable sin distribuir la norma.
3. **Evidencia con procedencia.** Hash, origen, método de recogida, versión del recolector, revisor y fecha; para la automatizada, conector, endpoint y transformación. Subida ≠ aceptada.
4. **Riesgo e impacto como procesos distintos y enlazados** (perspectiva organización vs. perspectiva personas/sociedad), lo más distintivo de 42001 frente a 27001 y lo que más omiten las plantillas.
5. **Honestidad métrica por diseño.** Toda puntuación expone fórmula y registros fuente; se usa "preparación / cobertura / implementación / eficacia / exposición", nunca "compliance score" ni "certificado".
6. **Bilingüe ES/EN con terminología UNE oficial** (safety = protección, security = seguridad, SoA = declaración de aplicabilidad) desde la fundación, no como añadido.
7. **La plataforma se gobierna a sí misma.** Sus funciones LLM (generación de políticas, sugerencia de riesgos, análisis de evidencia) constan en su propio inventario del tenant plataforma y cumplen A.6/A.8/A.9, con abstracción de proveedor y presupuesto de tokens por tenant. Es la mejor demo posible.

**Papel de la IA en el producto.** Asistente, no decisor: interpreta requisitos, sugiere riesgos y controles, redacta borradores de política, analiza evidencia y prepara auditorías; cada artefacto generado lleva modelo, hora, fuente y revisor, y una persona autorizada aprueba.

## 6. Alcance

### MVP (según ajuste A1: arquitectura de dominio completa, corte vertical operable)

Una organización real debe poder completar de punta a punta un SGIA mínimo:

1. **Fundación multi-tenant**: organizaciones, usuarios, roles por defecto (administrador, gestor de gobernanza, gestor de riesgos, cumplimiento, auditor, propietarios de sistema/control/evidencia, revisor, ejecutivo, solo lectura), registro de auditoría append-only con cadena de hashes, i18n ES/EN.
2. **Motor de normas**: ISO/IEC 42001:2023 + Amd 1:2024 y UNE-ISO/IEC 42001:2025 (cláusulas 4-10, 26 términos, 38 controles del Anexo A con distribución A.2:3 … A.10:3, Anexo B, C con 11 objetivos y 7 fuentes de riesgo, D), ISO/IEC 23894 (parcial, con `SOURCE_DETAIL_REQUIRED`), ISO 19011:2026; semilla versionada y sin texto literal.
3. **Contexto y alcance** (4.1-4.4), partes interesadas, política de IA y roles (5), objetivos (6.2).
4. **Inventario de IA**: sistemas, modelos, datos, proveedores y su grafo de dependencias; agentes como tipo de sistema (modelo de datos sí; gobierno avanzado de agentes, diferido).
5. **Riesgo e impacto**: criterios, evaluación, tratamiento, aceptación de residual, evaluación de impacto enlazada.
6. **SoA y modelo operativo de controles**: aplicabilidad con justificación, propietario, estado, activación de flujos por evento.
7. **Evidencia**: ciclo de vida completo, hash, procedencia, revisión y aprobación humana; evidencia manual y por API interna; conectores externos solo mediante SDK con adaptador *mock* etiquetado.
8. **Auditoría interna, hallazgos, no conformidades y CAPA** alineados con ISO 19011:2026, con verificación de eficacia.
9. **Revisión por la dirección** con entradas 9.3.2 y salidas 9.3.3.
10. **Informes esenciales**: SoA, cumplimiento por cláusula, registro de riesgos, preparación para auditoría (con la lista de comprobación de §53 y sin declarar certificación). Exportables.
11. **Organización demo** con datos de ejemplo derivados de `resumenes/` para ventas, formación y pruebas.

### Diferido a fases posteriores (visible en la arquitectura, no implementado)

Conectores reales Azure AI Foundry / OpenAI / Anthropic y descubrimiento automático; constructor dinámico de informes y los 25 informes; búsqueda semántica; paquetes sectoriales de controles; modelo de madurez; gobierno completo de agentes de IA (permisos de herramientas, límites de gasto, *kill switch*); programación de informes; motor de calidad de datos; integración con ISO/IEC 42006 (no está en el corpus; dependencia pendiente para auditoría de tercera parte).

### Fuera de alcance del producto

Certificar organizaciones; distribuir texto de normas; sustituir el juicio del auditor; marketplace de consultoras `[SUPUESTO]`.

## 7. Métricas de éxito (con contramétricas)

| Objetivo | Métrica principal | Meta MVP `[ESTIMACIÓN]` | Contramétrica (lo que no debe empeorar) |
|---|---|---|---|
| Operar un SGIA de punta a punta | Organizaciones piloto que completan el flujo §69 (sin conectores) | ≥ 1 real + demo en el MVP; ≥ 3 en 6 meses tras lanzamiento | Pasos completados sin aprobación humana registrada = 0 |
| Trazabilidad útil | % de controles aplicables con ≥ 1 requisito, ≥ 1 riesgo y ≥ 1 evidencia enlazados | ≥ 90 % en pilotos | % de enlaces creados automáticamente por IA sin revisor = 0 |
| Evidencia defendible | % de evidencia aceptada con hash y procedencia completa | 100 % | Evidencia aceptada sin revisor distinto del que la subió = 0 |
| Honestidad métrica | Puntuaciones que exponen fórmula y registros fuente | 100 % | Uso de los términos "compliance score" o "certificado" en UI = 0 |
| Valor para el auditor | Tiempo del auditor para seguir un hilo requisito→evidencia | < 2 min en demo con usuario auditor | Hallazgos de auditoría atribuibles a datos inconsistentes de la plataforma = 0 |
| Bilingüismo | Cobertura de cadenas ES/EN y terminología conforme al glosario | 100 % de la UI del MVP | Incidencias de terminología reportadas por implementadores: sin tendencia creciente entre versiones |
| Auto-gobernanza | Funciones LLM del producto registradas en el inventario del tenant plataforma con controles A.6/A.8/A.9 | 100 % | Coste LLM por tenant dentro del presupuesto configurado |
| Negocio `[SUPUESTO]` | Pilotos pagados o cartas de intención (empresa y consultora) | 2-3 antes de GA | Tiempo de onboarding de un tenant nuevo ≤ 1 día laborable |

## 8. Riesgos y supuestos

| # | Riesgo / supuesto | Impacto | Mitigación |
|---|---|---|---|
| R1 | **Alcance desbordado**: el prompt maestro tiene 73 secciones; tentación de construir todo | Nunca se lanza | MVP vertical A1; un cambio OpenSpec por capacidad; checkpoint humano antes de codificar |
| R2 | **Interpretación normativa incorrecta** en la semilla o en la lógica de flujos | Pérdida de credibilidad ante auditores | Semilla revisada por humano con cita a resumenes/NN; pruebas de dominio (38 controles, 26 términos); `SOURCE_DETAIL_REQUIRED` en vacíos; cambio de interpretación requiere aprobación |
| R3 | **Derechos de autor** de ISO/IEC/UNE | Riesgo legal | Solo paráfrasis propias, identificadores y títulos; nunca texto literal; material licenciado fuera de Git |
| R4 | **Competencia** de GRC anglófonas con marca y capital (ver addendum) | Presión de precio y de funcionalidades | Vertical, en español, que opera el SGIA en lugar de marcar casillas, con segmento consultora; no competir en amplitud de marcos |
| R5 | `[SUPUESTO]` Demanda hispanohablante suficiente y dispuesta a pagar | Modelo de negocio | Validar con 2-3 pilotos y una consultora antes de GA; investigación de mercado pendiente |
| R6 | **Confianza en la IA asistente**: alucinación de requisitos u obligaciones | Decisiones erróneas de cumplimiento | IA sugiere/humano aprueba; artefactos con modelo, hora, fuente y revisor; nunca inventar identificadores |
| R7 | **Seguridad multi-tenant** (fuga cross-tenant, escalada) | Fin del producto | RLS + tenant_id, pruebas de seguridad en cada cambio, secretos vía gestor, cadena de hashes |
| R8 | **Corpus incompleto**: 23894 al 23 %, 42006 ausente | Módulos de riesgo y auditoría externa limitados | Marcar vacíos; roadmap de adquisición de fuentes |
| R9 | `[SUPUESTO]` Equipo pequeño y método pesado (BMAD + OpenSpec) | Ceremonia > entrega | Agentes en paralelo; la trazabilidad del propio desarrollo (BMAD + OpenSpec + Git) se convierte en argumento de venta ante auditores y clientes regulados |
| R10 | **Deriva regulatoria** (AI Act, normativa LatAm) | Requisitos externos cambiantes | Módulo de requisitos externos desacoplado del motor de normas; versionado `StandardVersion` |

## 9. Restricciones

- **Idioma**: documentos en español; código, identificadores, enumeraciones canónicas, API y commits en inglés; UI bilingüe ES/EN desde la fundación con el glosario `resumenes/13` como terminología oficial.
- **Normas y derechos de autor**: nunca texto literal ni identificadores inventados; distinguir siempre `normative_requirement`, `implementation_guidance`, `recommended_practice`, `organizational_decision`, `product_requirement`, `technical_design_decision`; cada registro sembrado cita su documento fuente.
- **Reglas de dominio no negociables**: las seis reglas de `CLAUDE.md` (la SoA gobierna los flujos; riesgo tratado ≠ plan existente; evidencia subida ≠ aceptada; la IA sugiere y una persona aprueba; nunca "certificado ISO"; puntuaciones con fórmula y registros fuente) son requisitos de producto, ya recogidos en la sección 5.
- **Multi-tenencia y seguridad**: `tenant_id` en toda tabla de negocio con Row Level Security (ADR-002); aislamiento de almacenamiento por tenant; registro de auditoría append-only con cadena de hashes (ADR-005); RBAC con mínimo privilegio, arquitectura lista para SSO/OIDC/SAML y MFA; secretos solo mediante gestor; residencia de datos por región, copias, recuperación ante desastres y retención legal con RPO/RTO a fijar en el PRD.
- **Accesibilidad**: WCAG 2.2 AA.
- **Arquitectura**: decidida por ADR-001 a ADR-005 (detalle en `addendum.md` §E); sin conectores "validados" sin credenciales reales; sin UI con botones falsos.
- **Proceso**: aprobación humana obligatoria antes de bloquear arquitectura, cambiar interpretación normativa, declarar un requisito implementado, alterar evidencia, publicar o añadir dependencias copyleft.

## 10. Preguntas abiertas para el revisor humano

1. **Nombre comercial y marca** del producto (¿bajo Eligen/Micronotes o marca nueva?).
2. **Segmento de lanzamiento**: ¿empresa final o consultora implementadora como canal principal? ¿Qué países primero?
3. **Modelo de negocio y precios**: suscripción por sistemas de IA, por usuarios, por tenant; ¿plan multi-cliente para consultoras? ¿Servicios de implementación propios?
4. **Organización piloto**: ¿Micronotes/Eligen será el primer tenant real? ¿Hay una consultora o academia PECB dispuesta a co-diseñar?
5. **Alcance de idiomas**: ¿ES/EN basta para el MVP o se prevé PT-BR pronto?
6. **Residencia de datos y regiones** requeridas por los primeros clientes (UE, LatAm, EE. UU.) y RPO/RTO objetivo.
7. **Requisitos externos**: ¿el AI Act (y qué normativa latinoamericana) debe estar mapeado en el MVP o en fase posterior?
8. **Adquisición de fuentes**: ¿se comprará ISO/IEC 42006 y el resto de 23894 antes del módulo de auditoría de tercera parte?
9. **Primer conector real** tras el MVP: ¿Azure AI Foundry, OpenAI o Anthropic? Depende del piloto.
10. **Certificación del propio producto**: ¿se persigue ISO/IEC 42001 y/o 27001 para la plataforma como argumento comercial y en qué plazo?
