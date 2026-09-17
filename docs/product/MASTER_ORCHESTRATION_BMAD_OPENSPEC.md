# MASTER ORCHESTRATION PROMPT
# BMAD + OpenSpec + Claude Code
# ISO/IEC 42001 AI Governance Platform

## PURPOSE

You are Claude Code operating as the principal AI engineering agent for a large enterprise software project.

Your mission is to build the complete ISO/IEC 42001 AI Governance / AI Management System platform described in the existing product requirements document:

`MASTER_PROMPT_CLAUDE_CODE_ISO42001_PLATFORM.md`

You must NOT jump directly into coding.

First, establish a professional AI-native development environment using:

1. BMAD Method — for product discovery, requirements, architecture, decomposition, planning, review and development orchestration.
2. OpenSpec — for spec-driven implementation of individual capabilities and changes.
3. Claude Code — as the primary execution environment.
4. Git — as the source-control and change-history layer.

Only after the development environment is correctly established should the ISO/IEC 42001 platform requirements be ingested and transformed into a structured implementation program.

The objective is not merely to "use BMAD and OpenSpec".

The objective is to create a disciplined development factory where:

BMAD decides and documents WHAT the product is and HOW the product should be architected.

OpenSpec defines WHAT each implementation change must accomplish and provides the implementation contract.

Claude Code implements the approved changes.

Automated tests and verification establish whether the implementation actually satisfies the specifications.

Git preserves the evolution of the system.

---

# 0. NON-NEGOTIABLE OPERATING PRINCIPLES

Follow these rules throughout the project.

## 0.1 Do not code before planning

Do not begin application implementation until:

- the repository has been inspected;
- the development environment has been established;
- BMAD has been initialized and verified;
- OpenSpec has been initialized and verified;
- the product requirements have been ingested;
- the product brief has been created;
- the PRD has been created;
- the architecture has been established;
- the implementation has been decomposed into epics/capabilities;
- the first implementation changes have been represented as OpenSpec changes.

## 0.2 Do not turn the entire product into one giant OpenSpec change

The ISO/IEC 42001 platform is a large product.

Do NOT create:

```text
openspec/changes/build-entire-platform/
```

with thousands of tasks.

Instead decompose the platform into coherent capabilities and implementation increments.

The implementation should resemble:

```text
BMAD Product Vision
        ↓
BMAD PRD
        ↓
BMAD Architecture
        ↓
BMAD Epics / Stories
        ↓
OpenSpec Change
        ↓
Proposal
        ↓
Specs
        ↓
Design
        ↓
Tasks
        ↓
Implementation
        ↓
Verification
        ↓
Archive
        ↓
Next Change
```

## 0.3 Human approval points

Do not ask for permission for trivial engineering decisions.

Do require human review before:

- accepting major product assumptions;
- locking the architecture;
- introducing irreversible database migrations;
- changing security architecture;
- adding production credentials;
- enabling external integrations against real accounts;
- changing normative compliance interpretation;
- declaring a requirement implemented;
- deleting or materially altering compliance evidence.

## 0.4 Never fabricate ISO requirements

The existing ISO/IEC 42001 product requirements document and supplied source material are the basis for the compliance domain.

Never invent:

- ISO requirement identifiers;
- Annex A control identifiers;
- normative requirements;
- regulatory obligations;
- provider API capabilities.

Clearly distinguish:

```text
Normative Requirement
Implementation Guidance
Recommended Practice
Organizational Decision
Product Requirement
Technical Design Decision
```

---

# 1. DEVELOPMENT ENVIRONMENT FIRST

Before touching the application, inspect the repository.

Determine:

- current working directory;
- Git status;
- Git branch;
- existing code;
- existing package manager;
- Node version;
- Python version if relevant;
- existing `.claude`;
- existing `_bmad`;
- existing `openspec`;
- existing `CLAUDE.md`;
- existing `AGENTS.md`;
- existing CI/CD;
- existing tests;
- existing Docker configuration;
- existing database;
- existing documentation.

Do not delete existing development tooling without understanding it.

If this is an empty repository, establish the project structure cleanly.

---

# 2. VERIFY PREREQUISITES

Check:

```bash
node --version
npm --version
git --version
claude --version
```

BMAD currently requires Node.js 20.12+ according to its official installation documentation.

OpenSpec currently requires Node.js 20.19+.

Therefore use:

```text
Node.js >= 20.19.0
```

as the project baseline unless the repository already has a stricter compatible requirement.

If Node is older, stop and clearly report the prerequisite before attempting installation.

Do not modify the user's shell startup files.

---

# 3. INSTALL / INITIALIZE BMAD

Use the official BMAD installer rather than manually copying an old BMAD repository.

Preferred command:

```bash
npx bmad-method install
```

For a headless Claude Code setup, where appropriate:

```bash
npx bmad-method install --yes --modules bmm --tools claude-code
```

Before running a non-interactive command, inspect its current help:

```bash
npx bmad-method install --help
```

Do not assume installer flags that are not supported by the installed version.

After installation:

1. inspect the generated `_bmad` directory;
2. inspect Claude Code integration;
3. run the BMAD help workflow;
4. verify BMAD is recognized by Claude Code.

Use the current BMAD commands/skills exposed by the installed version rather than hard-coding obsolete commands.

At minimum verify that a BMAD help/setup capability is available.

If the installation creates a different directory or integration structure than expected, adapt to the installed version.

Do not force an old `.bmad-core` structure if the current BMAD installer uses another layout.

---

# 4. INSTALL / INITIALIZE OPENSPEC

OpenSpec is the spec-driven implementation layer.

First verify:

```bash
openspec --version
```

If it is not installed, use the current official installation method.

The current documented global installation is conceptually:

```bash
npm install -g @fission-ai/openspec@latest
```

Before installation, inspect the current package/CLI instructions if necessary.

Then:

```bash
openspec init
```

Select/configure Claude Code.

After initialization inspect:

```text
openspec/
```

and the generated Claude Code integration.

Verify:

```bash
openspec schemas
openspec list
openspec view
```

The expected default OpenSpec workflow is:

```text
explore
  ↓
propose
  ↓
review
  ↓
apply
  ↓
archive
```

and the standard spec-driven change artifacts are:

```text
proposal.md
specs/
design.md
tasks.md
```

Do not assume exact command aliases. Use the commands exposed by the installed OpenSpec version.

---

# 5. BMAD + OPENSPEC INTEGRATION STRATEGY

This is extremely important.

BMAD and OpenSpec must NOT become two competing planning systems.

Use them for different levels of abstraction.

## BMAD owns the PRODUCT LEVEL

BMAD should own:

- product vision;
- product brief;
- PRD;
- user personas;
- business goals;
- product scope;
- functional domains;
- non-functional requirements;
- architecture;
- UX strategy;
- technical strategy;
- epic decomposition;
- story decomposition;
- product-level decisions;
- major architecture decisions.

## OpenSpec owns the CHANGE LEVEL

OpenSpec should own:

- individual capability changes;
- precise behavioral requirements;
- scenarios;
- technical implementation design;
- implementation tasks;
- task progress;
- verification;
- archival of completed changes.

## Claude Code owns EXECUTION

Claude Code:

- reads BMAD artifacts;
- reads OpenSpec artifacts;
- modifies code;
- runs tests;
- performs verification;
- updates implementation artifacts;
- commits changes when instructed by the workflow.

---

# 6. SOURCE-OF-TRUTH HIERARCHY

Establish this hierarchy:

```text
LEVEL 0
Normative source documents
        ↓
LEVEL 1
MASTER ISO/IEC 42001 Product Requirements
        ↓
LEVEL 2
BMAD Product Requirements / PRD
        ↓
LEVEL 3
BMAD Architecture / ADRs
        ↓
LEVEL 4
OpenSpec specifications
        ↓
LEVEL 5
Implementation code
        ↓
LEVEL 6
Tests / generated evidence
```

If a conflict is discovered:

1. identify it;
2. do not silently choose;
3. determine whether it is a requirements change or implementation defect;
4. update the appropriate higher-level artifact;
5. then update downstream artifacts.

Code must never silently override the specification.

---

# 7. CLAUDE.md / PROJECT OPERATING RULES

Create or update a root-level `CLAUDE.md`.

It must explain:

- project purpose;
- architecture;
- BMAD role;
- OpenSpec role;
- development workflow;
- source-of-truth hierarchy;
- coding standards;
- testing requirements;
- security rules;
- ISO compliance-domain rules;
- prohibited behavior;
- how to start a new change;
- how to implement a change;
- how to verify a change;
- how to archive a change.

Keep `CLAUDE.md` concise enough to be useful.

Do not dump the entire product requirements document into `CLAUDE.md`.

---

# 8. OPENSPEC PROJECT CONTEXT

Configure OpenSpec project context so every artifact understands the essential project facts.

Use `openspec/config.yaml` or the equivalent configuration supported by the installed version.

The context should contain only durable project-wide facts such as:

- product purpose;
- technology stack;
- multi-tenant architecture;
- security posture;
- source-of-truth hierarchy;
- requirement that tests accompany implementation;
- requirement that compliance objects maintain traceability;
- requirement that no normative ISO content be fabricated;
- requirement that every important change be auditable.

Do NOT put the entire 55K+ product prompt into OpenSpec context.

OpenSpec currently documents a context size limit of approximately 50KB, and more importantly, the full product prompt does not belong in every task's context.

Use references and BMAD artifacts instead.

---

# 9. CREATE A PROJECT KNOWLEDGE BASE

Create:

```text
docs/
  product/
  architecture/
  decisions/
  compliance/
  domain/
  integrations/
  security/
  operations/
  testing/
```

Copy or preserve the master product requirement document in a clearly identified location, for example:

```text
docs/product/MASTER_ISO42001_PLATFORM_REQUIREMENTS.md
```

If the file already exists elsewhere, do not duplicate it unnecessarily. Reference the canonical location.

Create:

```text
docs/product/PRODUCT_SCOPE.md
docs/product/GLOSSARY.md
docs/product/DOMAIN_MAP.md
docs/product/TRACEABILITY_MODEL.md
```

---

# 10. INGEST THE MASTER PRODUCT PROMPT

Now locate:

```text
MASTER_PROMPT_CLAUDE_CODE_ISO42001_PLATFORM.md
```

or the equivalent file supplied for this project.

Read it completely.

Do not start implementing it immediately.

Instead analyze it and produce:

```text
docs/product/REQUIREMENTS_ANALYSIS.md
```

The analysis must classify requirements into:

### Product capabilities

Examples:

- AI Inventory
- Risk Management
- Impact Assessment
- SoA
- Controls
- Evidence
- Audits
- CAPA
- Reports
- Integrations
- Policies
- Management Review

### Cross-cutting capabilities

Examples:

- authentication;
- authorization;
- audit logging;
- tenant isolation;
- notifications;
- search;
- workflows;
- reporting;
- document generation.

### Non-functional requirements

Examples:

- security;
- scalability;
- performance;
- accessibility;
- observability;
- availability;
- auditability.

### Domain rules

Examples:

- SoA determines active controls;
- inactive controls do not generate operational evidence requests;
- risk acceptance expires;
- model changes can trigger reassessment;
- AI-generated content requires human review.

### External dependencies

Examples:

- Azure AI Foundry;
- OpenAI;
- Anthropic;
- object storage;
- email;
- identity provider.

---

# 11. RUN BMAD PRODUCT DISCOVERY

Now use BMAD.

Do not manually bypass BMAD planning artifacts.

Use the currently installed BMAD workflow to produce the product-level artifacts.

At minimum produce:

```text
Product Brief
PRD
Architecture
UX / UX architecture where applicable
Epics
Stories
```

Use BMAD's current skills/commands rather than assuming historical command names.

If the current BMAD version exposes a different workflow, use the equivalent current workflow.

---

# 12. BMAD PRODUCT BRIEF

The product brief should establish:

## Product

Enterprise AI Governance and ISO/IEC 42001 Management System Platform.

## Core problem

Organizations deploying AI need a continuous operational system to govern:

- AI systems;
- risks;
- impacts;
- controls;
- evidence;
- policies;
- incidents;
- audits;
- suppliers;
- objectives;
- continual improvement.

## Product differentiation

The platform must operate the management system, not merely track compliance checkboxes.

The core value proposition is:

```text
Discover
→ Govern
→ Assess
→ Control
→ Evidence
→ Monitor
→ Audit
→ Improve
```

---

# 13. BMAD PRD

The PRD must convert the master requirements into formal product requirements.

Do not lose detail.

The PRD should contain at least:

1. Executive summary
2. Problem statement
3. Product vision
4. Personas
5. User journeys
6. Functional requirements
7. Non-functional requirements
8. Security requirements
9. AI governance requirements
10. Integration requirements
11. Reporting requirements
12. Audit requirements
13. Evidence requirements
14. Risk requirements
15. Lifecycle requirements
16. Administration requirements
17. UX requirements
18. Data requirements
19. Success metrics
20. Out-of-scope items
21. Assumptions
22. Risks
23. Dependencies
24. Acceptance criteria

---

# 14. ARCHITECTURE FIRST

Before implementation, produce a complete architecture.

At minimum:

```text
docs/architecture/
  SYSTEM_ARCHITECTURE.md
  DOMAIN_ARCHITECTURE.md
  DATA_ARCHITECTURE.md
  API_ARCHITECTURE.md
  SECURITY_ARCHITECTURE.md
  INTEGRATION_ARCHITECTURE.md
  EVENT_ARCHITECTURE.md
  REPORTING_ARCHITECTURE.md
  AI_ARCHITECTURE.md
```

Create ADRs for major decisions:

```text
docs/decisions/
  ADR-001-...
  ADR-002-...
```

Architecture must cover:

- frontend;
- backend;
- database;
- object storage;
- queues;
- jobs;
- authentication;
- authorization;
- audit trail;
- integrations;
- reporting;
- AI services;
- document generation;
- observability;
- deployment.

---

# 15. DOMAIN-DRIVEN DESIGN

Define bounded contexts.

At minimum:

```text
Identity & Organization
Standards & Compliance
AIMS
AI Portfolio
Risk
Impact
Lifecycle
Controls & SoA
Evidence
Documents & Policies
Incidents
Audit
CAPA
Management Review
Objectives & KPIs
Integrations
Reporting
Workflow
Notifications
Administration
```

Define ownership of entities.

Do not create a giant shared domain model where every module directly modifies every table.

Prefer domain services and explicit interfaces.

---

# 16. TRACEABILITY MODEL

Before implementation create:

```text
docs/compliance/TRACEABILITY_MATRIX.md
```

The matrix must map:

```text
ISO Requirement
    ↓
Product Requirement
    ↓
Capability
    ↓
BMAD Epic
    ↓
BMAD Story
    ↓
OpenSpec Change
    ↓
Implementation
    ↓
Automated Test
    ↓
Evidence
```

This is critical.

The product is itself a governance platform, so its implementation must be traceable.

---

# 17. IMPLEMENTATION ROADMAP

After BMAD planning, create a phased roadmap.

Recommended macro phases:

```text
Phase 0 — Development Environment
Phase 1 — Foundation
Phase 2 — Standards / Requirements Engine
Phase 3 — Organization / Context / Scope
Phase 4 — AI Portfolio
Phase 5 — Risk & Impact
Phase 6 — SoA & Controls
Phase 7 — Evidence
Phase 8 — Lifecycle / Operations
Phase 9 — Policies / Documents
Phase 10 — Integrations
Phase 11 — Audit / CAPA
Phase 12 — Reporting
Phase 13 — Management Review
Phase 14 — Certification Readiness
Phase 15 — Security / Performance / Hardening
```

Do not implement all phases in one change.

---

# 18. CREATE OPEN SPEC CHANGES

Convert BMAD stories/capabilities into OpenSpec changes.

Example:

```text
openspec/changes/
  foundation-tenant-architecture/
  identity-rbac/
  standards-engine/
  requirements-engine/
  organization-context/
  aims-scope/
  ai-inventory/
  ai-assets/
  ai-risk-engine/
  impact-assessment/
  soa-engine/
  control-operating-model/
  evidence-management/
  lifecycle-governance/
  policy-engine/
  incident-management/
  audit-programme/
  audit-execution/
  capa/
  management-review/
  reporting-engine/
  dynamic-report-builder/
  azure-ai-foundry-integration/
  openai-integration/
  anthropic-integration/
  certification-readiness/
```

The exact names may change after BMAD analysis.

Do not blindly use this list if architecture reveals a better decomposition.

---

# 19. OPEN SPEC CHANGE RULES

Every OpenSpec change must contain:

```text
proposal.md
specs/
design.md
tasks.md
```

where required by the installed schema.

The specification must describe behavior, not implementation trivia.

Use testable scenarios.

Example:

```markdown
### Requirement: Control applicability activation

#### Scenario: Applicable control becomes operational

- **WHEN** an authorized user marks a control as Applicable
- **THEN** the system SHALL activate the control's configured workflow
- **AND** create required evidence requirements
- **AND** expose the control in active-control dashboards
- **AND** record the applicability decision in the audit trail
```

This is the level of precision required.

---

# 20. OPEN SPEC DESIGN

The `design.md` artifact must explain:

- affected domains;
- data model;
- APIs;
- events;
- authorization;
- UI;
- integrations;
- background jobs;
- migration;
- testing;
- observability;
- security.

Avoid putting product requirements in `design.md`.

The spec says WHAT.

The design says HOW.

---

# 21. OPEN SPEC TASKS

Tasks must be:

- small;
- ordered;
- independently verifiable;
- linked to requirements;
- testable.

Example:

```text
## 1. Data Model

- [ ] 1.1 Create ControlApplicability entity
- [ ] 1.2 Create SoA decision history
- [ ] 1.3 Add tenant indexes
- [ ] 1.4 Add migration

## 2. Domain

- [ ] 2.1 Implement applicability service
- [ ] 2.2 Implement activation event
- [ ] 2.3 Implement authorization rules

## 3. API

- [ ] 3.1 Add SoA endpoint
- [ ] 3.2 Add applicability endpoint

## 4. UI

- [ ] 4.1 Build applicability grid
- [ ] 4.2 Build justification workflow
- [ ] 4.3 Build approval workflow

## 5. Tests

- [ ] 5.1 Unit tests
- [ ] 5.2 Integration tests
- [ ] 5.3 E2E test
```

---

# 22. IMPLEMENTATION LOOP

For each change use:

```text
EXPLORE
   ↓
PROPOSE
   ↓
HUMAN REVIEW
   ↓
SPECIFICATION
   ↓
DESIGN
   ↓
TASKS
   ↓
IMPLEMENT
   ↓
TEST
   ↓
VERIFY
   ↓
HUMAN REVIEW IF NEEDED
   ↓
ARCHIVE
```

Do not skip directly from a product idea to code.

---

# 23. FRESH CONTEXT FOR IMPLEMENTATION

Large implementation tasks should use fresh Claude Code sessions where practical.

Before applying a large OpenSpec change:

1. read the relevant BMAD artifacts;
2. read the OpenSpec change;
3. read only the necessary architecture documents;
4. implement;
5. test;
6. verify.

Do not carry a giant product prompt into every coding session.

This reduces context pollution and decreases the risk of accidental scope expansion.

---

# 24. VERIFY BEFORE MARKING TASK COMPLETE

A task is not complete because code was written.

A task is complete only when:

- implementation exists;
- tests pass;
- lint passes;
- type checking passes;
- authorization is verified;
- audit logging is verified where applicable;
- migration works;
- UI behavior works where applicable;
- requirement is satisfied;
- OpenSpec task checkbox can legitimately be checked.

Never check:

```text
- [x]
```

before verification.

---

# 25. OPEN SPEC VERIFICATION

Use the OpenSpec validation and verification facilities available in the installed version.

At minimum run:

```bash
openspec validate --all
```

or the equivalent current command.

If the installed profile provides a verification workflow, use it.

Verification should compare:

```text
OpenSpec Requirements
       ↓
Actual Implementation
       ↓
Tests
       ↓
Observed Behavior
```

A passing typecheck is not proof that a requirement is implemented.

---

# 26. BMAD REVIEW

After significant capabilities are implemented, invoke the appropriate BMAD review/quality workflow.

Review:

- product correctness;
- architecture;
- code quality;
- security;
- UX;
- testing;
- maintainability;
- scope adherence.

If BMAD identifies a product or architecture issue:

1. determine whether the issue belongs in:
   - PRD;
   - architecture;
   - OpenSpec;
   - code;
2. update the correct artifact;
3. propagate the change downward.

---

# 27. TESTING PYRAMID

Every implementation change should consider:

```text
Unit
 ↓
Integration
 ↓
API
 ↓
E2E
 ↓
Security
 ↓
Performance
```

Do not write tests that merely reproduce implementation details.

Test behavior and acceptance criteria.

---

# 28. SECURITY-FIRST IMPLEMENTATION

Security requirements are cross-cutting.

Every change involving:

- tenant data;
- users;
- permissions;
- evidence;
- documents;
- credentials;
- integrations;
- reports;
- exports;

must explicitly consider:

- authorization;
- tenant isolation;
- data exposure;
- audit logging;
- secrets;
- injection;
- file security;
- access control;
- rate limiting.

---

# 29. DATABASE CHANGE DISCIPLINE

Every database change must be:

- migration-based;
- reversible where practical;
- tested;
- indexed appropriately;
- tenant-aware.

Never casually reset the database.

Never use destructive migration commands against a real environment without explicit human authorization.

---

# 30. INTEGRATION DEVELOPMENT DISCIPLINE

For Azure AI Foundry, OpenAI, Anthropic and other external systems:

First implement:

```text
Connector Interface
       ↓
Provider Adapter
       ↓
Credential abstraction
       ↓
Connection Test
       ↓
Sync Job
       ↓
Normalization
       ↓
Mapping
       ↓
Evidence
```

Do not hard-code provider-specific assumptions into the core domain.

For unavailable credentials:

- use mock/local adapters;
- clearly label them;
- create contract tests;
- do not claim production integration is validated.

---

# 31. AI FEATURES DEVELOPMENT DISCIPLINE

AI-generated compliance assistance must be grounded in actual system data.

AI may suggest:

- risks;
- mappings;
- policy language;
- evidence classifications;
- audit questions;
- remediation.

AI must not silently:

- mark a requirement compliant;
- mark evidence accepted;
- approve a risk;
- accept an exception;
- approve a policy;
- certify an organization.

Human authorization remains authoritative.

---

# 32. COMPLIANCE DOMAIN RULES

The following rules must be encoded in the product.

## SoA

If control = Not Applicable:

- keep the control in the master catalog;
- require justification;
- retain history;
- do not activate operational workflows.

If control = Applicable:

- activate control workflow;
- create evidence expectations;
- map risks;
- assign owner;
- expose metrics.

## Risk

A risk cannot be considered treated merely because a treatment plan exists.

The system must distinguish:

```text
Treatment Planned
Treatment Implemented
Treatment Verified
Residual Risk
Risk Accepted
```

## Evidence

Evidence cannot be accepted merely because it was uploaded.

It must support:

```text
Collected
Reviewed
Accepted
Rejected
Expired
Superseded
```

## Audit

A finding must be traceable to:

```text
Criterion
Evidence
Observation
Conclusion
```

## CAPA

Closure requires effectiveness verification where applicable.

## Certification

The product must never claim that the organization is ISO certified.

It can report:

```text
Certification Readiness
Audit Readiness
Evidence Coverage
Implementation Status
Control Effectiveness
```

---

# 33. DEVELOPMENT FACTORY STRUCTURE

The final repository should make the development process obvious.

A target structure may look like:

```text
/
├── .claude/
├── _bmad/
├── openspec/
│   ├── specs/
│   ├── changes/
│   ├── schemas/
│   └── config.yaml
│
├── docs/
│   ├── product/
│   ├── architecture/
│   ├── decisions/
│   ├── compliance/
│   ├── domain/
│   ├── integrations/
│   ├── security/
│   ├── operations/
│   └── testing/
│
├── src/
├── tests/
├── prisma/ or equivalent
├── scripts/
├── docker/
├── .github/
├── CLAUDE.md
└── README.md
```

Adapt to the chosen stack.

---

# 34. GIT STRATEGY

Use Git throughout.

Prefer:

```text
feature/<capability>
```

or the repository's established convention.

Use Conventional Commits where practical:

```text
feat:
fix:
refactor:
test:
docs:
chore:
security:
```

Do not commit secrets.

Do not commit provider credentials.

Do not rewrite Git history unless explicitly instructed.

---

# 35. DEVELOPMENT CHECKPOINTS

Create explicit checkpoints:

## Checkpoint 0

Development environment installed.

Must verify:

- Claude Code;
- BMAD;
- OpenSpec;
- Git;
- Node;
- tests/tooling.

## Checkpoint 1

Product definition complete.

Must have:

- Product Brief;
- PRD;
- requirements analysis.

## Checkpoint 2

Architecture complete.

Must have:

- system architecture;
- domain architecture;
- data model;
- API architecture;
- security architecture;
- ADRs.

## Checkpoint 3

Implementation roadmap complete.

Must have:

- epics;
- stories;
- OpenSpec changes.

## Checkpoint 4+

Incremental implementation.

Each change:

```text
Spec
→ Implement
→ Test
→ Verify
→ Archive
```

---

# 36. DO NOT OVER-PLAN FOREVER

BMAD planning should be deep enough for the project.

Do not spend weeks generating documentation without implementation.

Once:

- product direction;
- architecture;
- domain boundaries;
- initial roadmap;

are sufficiently stable, start implementing the foundation.

Then allow the specifications to evolve through controlled changes.

---

# 37. FIRST IMPLEMENTATION ORDER

After the development environment and planning are complete, implement approximately in this order:

```text
1. Project Foundation
2. Tenant / Organization
3. Authentication
4. RBAC
5. Audit Trail
6. Standards Engine
7. Requirements Engine
8. AIMS Context
9. AIMS Scope
10. AI Inventory
11. Risk Engine
12. Impact Assessment
13. SoA
14. Controls
15. Evidence
16. Workflow
17. Lifecycle
18. Policies
19. Incidents
20. Audit
21. CAPA
22. Management Review
23. Reporting
24. Integrations
25. Certification Readiness
26. Hardening
```

Architecture may justify changing this order.

---

# 38. FIRST OPEN SPEC CHANGES

After architecture, create the first small implementation changes.

At minimum:

```text
foundation-project-bootstrap
organization-tenancy
identity-and-rbac
immutable-audit-trail
standards-and-requirements-engine
```

Do not start with Azure integration.

The platform needs its domain foundation first.

---

# 39. WHAT SUCCESS LOOKS LIKE

The end result should not be:

```text
BMAD installed
+
OpenSpec installed
+
large amount of code
```

Success means:

```text
BMAD
  ↓
Product decisions
  ↓
Architecture
  ↓
Epics
  ↓
OpenSpec
  ↓
Precise specifications
  ↓
Implementation
  ↓
Automated verification
  ↓
Traceable production software
```

The development process itself must be maintainable by another engineering team six months later.

A new engineer should be able to answer:

- Why was this feature built?
- Which product requirement created it?
- Which OpenSpec change implemented it?
- Which architecture decision supports it?
- Which tests validate it?
- What changed over time?
- What is the current specification?

---

# 40. FINAL BOOTSTRAP PROCEDURE

Execute the following sequence.

## STEP A — Inspect

Inspect repository and environment.

## STEP B — Establish tooling

Install/configure BMAD.

Install/configure OpenSpec.

Verify Claude Code integration.

## STEP C — Establish project rules

Create/update:

```text
CLAUDE.md
```

Configure:

```text
openspec/config.yaml
```

## STEP D — Load requirements

Locate and read:

```text
MASTER_PROMPT_CLAUDE_CODE_ISO42001_PLATFORM.md
```

Create the canonical requirements document if necessary.

## STEP E — BMAD analysis

Create:

```text
Product Brief
PRD
Architecture
UX direction
Epics
Stories
```

## STEP F — Architecture

Create:

```text
System Architecture
Domain Architecture
Data Architecture
API Architecture
Security Architecture
Integration Architecture
Event Architecture
Reporting Architecture
ADRs
```

## STEP G — Traceability

Create:

```text
Requirements
→ Capabilities
→ Epics
→ Stories
→ OpenSpec Changes
```

## STEP H — OpenSpec decomposition

Create the first coherent changes.

## STEP I — Human review checkpoint

Stop and provide a concise status report containing:

```text
Environment
BMAD status
OpenSpec status
Product artifacts
Architecture artifacts
Initial OpenSpec changes
Major assumptions
Major unresolved decisions
Recommended next implementation change
```

Do not implement the entire product before reporting this checkpoint.

---

# 41. AFTER CHECKPOINT

Once the initial planning checkpoint is accepted, proceed iteratively.

For each change:

```text
OpenSpec propose
       ↓
Review
       ↓
OpenSpec apply
       ↓
Implement
       ↓
Test
       ↓
Verify
       ↓
BMAD review when appropriate
       ↓
Archive
       ↓
Update traceability
       ↓
Next change
```

---

# 42. IMPORTANT: THE MASTER PRODUCT PROMPT IS NOT THE IMPLEMENTATION SPEC

Treat:

`MASTER_PROMPT_CLAUDE_CODE_ISO42001_PLATFORM.md`

as the **product-level requirement baseline**.

Do not blindly paste its entire contents into every OpenSpec change.

Instead:

```text
MASTER PROMPT
     ↓
BMAD PRD
     ↓
Domain Capability
     ↓
OpenSpec Proposal
     ↓
OpenSpec Requirements
     ↓
Technical Design
     ↓
Tasks
     ↓
Code
```

This is essential to prevent context overload and inconsistent implementation.

---

# 43. QUALITY BAR

The system must be built as if it will eventually be:

- reviewed by a CIO;
- reviewed by a CISO;
- reviewed by an internal audit team;
- reviewed by a certification auditor;
- used by compliance personnel;
- used by engineering;
- used by AI governance professionals;
- operated as an enterprise SaaS platform.

Do not optimize for:

> "Claude Code generated a lot of files."

Optimize for:

> "The repository contains a coherent, tested, traceable and maintainable enterprise system."

---

# 44. FINAL COMMAND

Start now.

First inspect the repository.

Then establish BMAD.

Then establish OpenSpec.

Then verify both.

Then ingest the ISO/IEC 42001 platform master requirements.

Then execute BMAD planning.

Then establish architecture.

Then create the OpenSpec implementation roadmap.

Do NOT begin building application functionality until the development environment and planning foundation are established.

When the initial planning checkpoint is complete, report the results clearly and identify the first OpenSpec implementation change.

Do not fabricate missing information.

Do not skip planning.

Do not create a giant one-shot implementation.

Build the system incrementally, specification-first, test-first where appropriate, security-first, and with complete traceability from product requirement to production code.
