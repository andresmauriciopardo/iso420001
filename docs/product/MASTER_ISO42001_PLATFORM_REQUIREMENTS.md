# MASTER PROMPT — BUILD A COMPLETE ISO/IEC 42001 AI GOVERNANCE PLATFORM

## ROLE

Act as a principal software architect, senior full-stack engineer, AI governance architect, GRC product architect, DevSecOps engineer and ISO/IEC 42001 implementation specialist.

You are working inside a Claude Code repository. Your job is to **design and implement a production-grade, enterprise SaaS platform for implementing, operating, monitoring and auditing an ISO/IEC 42001 Artificial Intelligence Management System (AIMS/SGIA)**.

Do not build a simple compliance checklist.

Build a complete operational system in which an organization can:

- establish its AI management system;
- understand and document its organizational context;
- inventory all AI systems;
- manage AI assets and dependencies;
- define scope and interested parties;
- manage every ISO/IEC 42001 requirement;
- select and manage applicable Annex A controls through a Statement of Applicability (SoA);
- automatically activate only the workflows associated with selected controls;
- generate and maintain AI policies and procedures;
- identify, assess, treat, accept and continuously monitor AI risks;
- perform AI impact assessments;
- manage AI system lifecycle governance;
- govern AI data;
- manage transparency and stakeholder information;
- manage human oversight;
- manage AI incidents;
- manage suppliers and third parties;
- manage objectives, KPIs and evidence;
- collect evidence automatically from connected AI platforms;
- conduct internal audits;
- manage findings, nonconformities and corrective actions;
- conduct management reviews;
- generate executive, auditor, operational and domain-specific reports;
- maintain a complete audit trail;
- demonstrate objective evidence for certification readiness.

The platform must be designed as an **AI Governance Operating System**, not merely as a GRC database.

---

# 1. SOURCE OF TRUTH

The repository contains Markdown summaries derived from the following source documents:

- ISO/IEC 42001:2023
- UNE-ISO/IEC 42001:2025
- ISO/IEC 23894:2023
- ISO 19011:2026
- ISO/IEC 42001 implementation guidance
- PECB training/certification material

Use those documents as the primary contextual basis.

Important:

1. ISO/IEC 42001 is the primary source for AIMS requirements.
2. ISO/IEC 23894 is supporting guidance for AI risk management.
3. ISO 19011:2026 is supporting guidance for auditing management systems.
4. Implementation guides are practical/non-normative guidance.
5. Training catalogs are not normative requirements.
6. Never present implementation advice as if it were an ISO requirement.
7. Never invent requirements, controls, control identifiers or normative wording.
8. Do not reproduce copyrighted standards verbatim. Store concise requirement summaries, identifiers, interpretations, evidence expectations and references, not the full standard text.
9. Where exact normative wording is unavailable, clearly distinguish:
   - `normative_requirement`
   - `implementation_guidance`
   - `recommended_practice`
   - `evidence_example`
10. The system must be architected so a future standard revision can be added without rewriting the application.

Create a versioned `standards` knowledge model.

At minimum:

```text
Standard
StandardVersion
Clause
SubClause
Requirement
Annex
ControlDomain
Control
ControlGuidance
RequirementControlMapping
StandardRelationship
```

Example relationships:

```text
ISO/IEC 42001:2023
        |
        +-- Clause 4
        +-- Clause 5
        +-- Clause 6
        +-- Clause 7
        +-- Clause 8
        +-- Clause 9
        +-- Clause 10
        |
        +-- Annex A
        +-- Annex B
        +-- Annex C
        +-- Annex D

ISO/IEC 23894:2023
        |
        +-- AI Risk Management
        +-- Risk Framework
        +-- Risk Process
        +-- AI Risk Sources
        +-- AI Lifecycle Mapping

ISO 19011:2026
        |
        +-- Audit Principles
        +-- Audit Programme
        +-- Audit Planning
        +-- Audit Execution
        +-- Evidence
        +-- Findings
        +-- Conclusions
        +-- Auditor Competence
```

---

# 2. FIRST ACTION — ANALYZE THE EXISTING REPOSITORY

Before writing substantial code:

1. Inspect the repository.
2. Identify the existing stack.
3. Identify package manager.
4. Identify frontend/backend architecture.
5. Identify database.
6. Identify authentication.
7. Identify testing framework.
8. Identify deployment configuration.
9. Identify existing design system.
10. Identify reusable components.
11. Identify existing API conventions.
12. Identify existing CI/CD.
13. Identify environment variables.
14. Identify security controls.
15. Identify existing documentation.

Do not unnecessarily replace an existing architecture.

If the repository is empty or lacks a reasonable production stack, use a modern enterprise architecture such as:

- TypeScript
- Next.js
- React
- Tailwind CSS
- shadcn/ui or equivalent accessible component system
- PostgreSQL
- Prisma or equivalent ORM
- REST and/or typed API layer
- Zod or equivalent schema validation
- background jobs/queues
- object storage for evidence/documents
- Redis where useful
- OpenTelemetry
- structured logging
- Docker
- automated tests
- CI/CD

Use the existing stack when it is already established and sound.

---

# 3. PRODUCT VISION

The application should feel like a combination of:

- enterprise GRC platform;
- AI inventory/catalog;
- AI risk-management platform;
- compliance operating system;
- audit management platform;
- evidence management platform;
- AI observability/control plane;
- executive reporting platform.

It must be suitable for:

- CIO
- CISO
- Chief AI Officer
- AI Governance Officer
- Compliance Officer
- Risk Manager
- Internal Auditor
- AI System Owner
- Model Owner
- Data Owner
- Security Officer
- Privacy Officer
- Legal
- Executive Management
- External Auditor

---

# 4. MULTI-TENANT ARCHITECTURE

Design the application as multi-tenant from the beginning.

Entities:

```text
Organization
Tenant
BusinessUnit
Department
Location
User
Role
Permission
Team
Membership
```

All tenant-owned data must be isolated.

Support:

- organization-level roles;
- business-unit roles;
- project/system-level access;
- least privilege;
- RBAC;
- optional ABAC where justified;
- SSO/OIDC/SAML-ready architecture;
- MFA-compatible architecture;
- session management;
- invitation workflow;
- user lifecycle;
- deactivation;
- access reviews.

Default roles should include:

- Platform Administrator
- Organization Administrator
- AI Governance Manager
- Risk Manager
- Compliance Manager
- Auditor
- AI System Owner
- Control Owner
- Evidence Owner
- Reviewer
- Executive
- Read Only

Implement granular permissions such as:

```text
organization.read
organization.manage

users.read
users.manage
roles.manage

standards.read
standards.manage

requirements.read
requirements.manage

controls.read
controls.manage
controls.activate
controls.deactivate

soa.read
soa.manage
soa.approve

risks.read
risks.manage
risks.approve
risks.accept

assets.read
assets.manage

ai_systems.read
ai_systems.manage

assessments.read
assessments.manage
assessments.approve

evidence.read
evidence.upload
evidence.approve
evidence.delete

audits.read
audits.manage
audits.execute

reports.read
reports.generate
reports.export

integrations.read
integrations.manage
integrations.sync

policies.read
policies.generate
policies.approve

management_review.read
management_review.manage
```

---

# 5. CORE INFORMATION MODEL

Design a normalized relational model with strong relationships and history.

Core entities:

```text
Organization
BusinessUnit
Department
Location
InterestedParty
Requirement
Control
ControlDomain
StatementOfApplicability
SoAItem
AIObjective
AIObjectiveMetric
AIAsset
AISystem
AIModel
AIModelVersion
AIProvider
AIDeployment
AIUseCase
AIProject
AIDataset
AIDataSource
AIIntegration
Risk
RiskSource
Threat
Vulnerability
RiskAssessment
RiskTreatment
RiskTreatmentPlan
RiskAcceptance
ResidualRisk
ImpactAssessment
ImpactAssessmentFinding
LifecycleStage
LifecycleGate
LifecycleReview
Policy
PolicyVersion
Procedure
Evidence
EvidenceRequest
EvidenceLink
EvidenceReview
Document
DocumentVersion
Incident
IncidentEvent
Finding
Nonconformity
CorrectiveAction
Audit
AuditProgramme
AuditPlan
AuditEvidence
AuditFinding
AuditConclusion
ManagementReview
ManagementReviewInput
ManagementReviewDecision
Task
Workflow
Approval
Comment
Notification
KPI
Metric
Dashboard
ReportDefinition
ReportRun
ReportSchedule
Integration
IntegrationCredential
IntegrationSync
IntegrationFinding
ChangeRequest
Exception
RegulatoryRequirement
Supplier
Customer
Contract
TrainingRequirement
TrainingRecord
Competency
```

Every important entity should have:

```text
id
tenant_id
created_at
created_by
updated_at
updated_by
status
version
deleted_at where appropriate
```

Use immutable audit records for important compliance decisions.

---

# 6. ISO/IEC 42001 REQUIREMENT MANAGEMENT

This is one of the most important modules.

The application must represent **every requirement in Clauses 4–10**, not just the clause titles.

For each requirement create:

```text
Requirement ID
Standard
Version
Clause
Subclause
Title
Requirement Summary
Purpose
Applicability
Implementation Guidance
Required/Expected Evidence
Responsible Role
Control Relationships
Risk Relationships
Policy Relationships
Procedure Relationships
Task Relationships
KPI Relationships
Audit Questions
Evidence Status
Implementation Status
Owner
Reviewer
Due Date
Notes
```

Statuses:

```text
Not Started
Planned
In Progress
Implemented
Partially Implemented
Not Applicable
Needs Review
Evidence Pending
Ready for Audit
Nonconforming
```

For every requirement provide a dedicated detail page.

The page must answer:

1. What does the requirement address?
2. Why does it matter?
3. What does the organization need to do?
4. Who owns it?
5. What evidence is expected?
6. Which controls support it?
7. Which risks relate to it?
8. Which policies/procedures support it?
9. Which AI systems are affected?
10. Which tasks are open?
11. What is the implementation status?
12. What evidence has been collected?
13. What audit questions apply?
14. What findings exist?
15. What corrective actions exist?
16. What is the audit readiness?

Create an implementation workspace for every requirement.

---

# 7. CLAUSES 4–10 — DEDICATED FUNCTIONALITY

Do not implement Clauses 4–10 as static text.

Each clause needs functional workflows.

## Clause 4 — Context

Implement:

### 4.1 Organization and context
- internal issues;
- external issues;
- strategic context;
- technological context;
- regulatory context;
- AI landscape;
- dependencies;
- strengths/weaknesses;
- opportunities/threats;
- review history.

### 4.2 Interested parties
Manage:
- employees;
- customers;
- regulators;
- suppliers;
- technology partners;
- investors;
- affected groups;
- other stakeholders.

For each:
- needs;
- expectations;
- influence;
- relevant requirements;
- communication needs;
- review date.

### 4.3 AIMS scope
Build a scope designer.

Scope should be composable from:
- business units;
- locations;
- AI systems;
- processes;
- products;
- services;
- lifecycle activities;
- suppliers.

Support:
- exclusions;
- exclusion justification;
- approval;
- scope versioning;
- scope change history.

### 4.4 AIMS
Provide an AIMS control center showing:
- status;
- objectives;
- risks;
- controls;
- evidence;
- audits;
- improvements.

---

# 8. CLAUSE 5 — LEADERSHIP

Implement:

## AI Policy
A policy lifecycle:

```text
Draft
AI Generated
Under Review
Pending Approval
Approved
Published
Superseded
Archived
```

Policy generator must use:

- organization context;
- AIMS scope;
- AI objectives;
- applicable controls;
- selected risk posture;
- applicable regulations;
- organizational principles.

Never invent facts.

AI-generated content must be explicitly marked as generated and require human review/approval.

## Roles and responsibilities

Support:

- RACI matrix;
- AI Governance Committee;
- AI System Owner;
- Model Owner;
- Data Owner;
- Human Supervisor;
- Risk Owner;
- Control Owner;
- Evidence Owner;
- Auditor.

---

# 9. CLAUSE 6 — PLANNING

This should be one of the strongest modules.

## 6.1 Risk and opportunity management

Build an enterprise AI risk engine.

Support:

```text
Risk Source
Risk Event
Cause
Consequence
Affected Stakeholders
Affected AI System
Affected Asset
Existing Controls
Likelihood
Impact
Inherent Risk
Treatment
Residual Risk
Risk Owner
Target Date
Acceptance
Monitoring
Review
```

Use configurable scoring.

Do not hard-code one universal scoring method.

Allow:

- 3x3;
- 4x4;
- 5x5;
- custom scales.

Support:

- likelihood;
- impact;
- severity;
- velocity;
- detectability where useful;
- confidence;
- uncertainty.

The platform should calculate:

```text
Inherent Risk
Control Effectiveness
Residual Risk
Risk Trend
Risk Appetite Status
```

Risk treatment options:

- Avoid
- Mitigate
- Transfer/Share
- Accept/Retain

Include risk acceptance workflow.

Acceptance must require:
- authorized role;
- rationale;
- expiration/review date;
- residual risk;
- approval history.

## Dynamic risk

Use ISO/IEC 23894 principles.

Risk should be capable of changing when:
- model changes;
- model version changes;
- dataset changes;
- provider changes;
- use case changes;
- deployment changes;
- incident occurs;
- regulatory requirement changes;
- monitoring detects degradation;
- threat landscape changes.

Create risk reassessment triggers.

---

# 10. AI IMPACT ASSESSMENT

Create a dedicated Impact Assessment engine.

Assess:

- individuals;
- groups;
- society;
- privacy;
- fairness;
- transparency;
- safety;
- security;
- human rights;
- accessibility;
- environmental considerations;
- economic/social impact;
- automated decision-making;
- human oversight;
- foreseeable misuse.

Support:

```text
Assessment Template
Assessment Instance
Question
Answer
Evidence
Finding
Severity
Mitigation
Owner
Approval
Review
```

Impact assessments should be versioned and linked to the AI system lifecycle.

---

# 11. AI INVENTORY / AI ASSET MANAGEMENT

This must be a major module.

Create an enterprise AI inventory.

## AI system record

Capture:

- name;
- description;
- business purpose;
- owner;
- technical owner;
- business owner;
- provider;
- model;
- model version;
- deployment;
- environment;
- geography;
- data;
- data classification;
- personal data;
- sensitive data;
- users;
- affected stakeholders;
- automation level;
- decision type;
- human oversight;
- intended use;
- prohibited use;
- foreseeable misuse;
- risk classification;
- impact classification;
- lifecycle status;
- dependencies;
- contracts;
- controls;
- incidents;
- monitoring;
- evidence.

## AI asset relationship graph

Show:

```text
Business Process
      |
      v
AI System
      |
      +---- Model
      +---- Model Version
      +---- Dataset
      +---- Provider
      +---- API
      +---- Infrastructure
      +---- Users
      +---- Business Owner
      +---- Risks
      +---- Controls
      +---- Evidence
      +---- Incidents
      +---- Contracts
```

Allow graph visualization.

---

# 12. AI LIFECYCLE GOVERNANCE

Implement lifecycle stages:

```text
Idea
Assessment
Design
Development
Training
Validation
Approval
Deployment
Production
Monitoring
Change
Retirement
```

Each stage must support:

- entry criteria;
- required evidence;
- required controls;
- risk review;
- impact review;
- approval;
- exit criteria;
- audit trail.

Create configurable lifecycle gates.

Example:

```text
Deployment Gate
    |
    +-- Risk assessment complete
    +-- Impact assessment complete
    +-- Required controls implemented
    +-- Validation complete
    +-- Human oversight defined
    +-- Documentation complete
    +-- Owner approval
```

A system must not be allowed to progress if mandatory gates are incomplete, unless an authorized exception exists.

---

# 13. STATEMENT OF APPLICABILITY — SoA

This must be a first-class module.

Load the complete Annex A control catalog from the source material available in the repository.

Group controls by:

- A.2 AI policies
- A.3 Internal organization
- A.4 Resources for AI systems
- A.5 Impact assessment
- A.6 AI system lifecycle
- A.7 Data for AI systems
- A.8 Information for interested parties
- A.9 Use of AI systems
- A.10 Third-party/customer relationships

Do not simply display controls.

For each control:

```text
Control ID
Domain
Title
Purpose
Applicability
Selected? 
Justification
Risk(s)
AI System(s)
Requirement(s)
Implementation Status
Control Owner
Evidence Requirements
Evidence Status
Exceptions
Residual Risk
Approval
Effective Date
Review Date
```

Applicability options:

```text
Applicable
Not Applicable
Applicable with Alternative Control
Pending Assessment
```

For `Not Applicable`, require documented justification.

For `Applicable`, require:
- owner;
- implementation status;
- evidence expectations;
- mapped risks;
- mapped requirements.

## Critical behavior

The SoA controls the rest of the application.

If a control is **not selected as applicable**:

- do not create unnecessary operational tasks for it;
- do not show it as an active control requirement in control dashboards;
- do not request recurring evidence for it;
- do not include it in active-control performance metrics.

However:

- it must remain visible in the complete Annex A catalog;
- its applicability decision and justification must remain auditable;
- the system must include it in SoA completeness reports.

If a control becomes applicable:

- activate its workflows;
- create required tasks;
- activate evidence requirements;
- activate monitoring;
- activate relevant report metrics.

This should be event-driven.

---

# 14. CONTROL OPERATING MODEL

Each active control needs a real operational workspace.

For every active control show:

```text
Control Objective
Why It Exists
Mapped Requirements
Mapped Risks
Mapped AI Systems
Owner
Implementation Plan
Tasks
Evidence
Testing
Metrics
Exceptions
Findings
Corrective Actions
Audit History
Effectiveness
Next Review
```

Control lifecycle:

```text
Not Applicable
Applicable
Planned
Implementing
Implemented
Operating
Needs Improvement
Ineffective
Retired
```

Controls must be testable.

Create control testing workflows:

- design effectiveness;
- implementation;
- operating effectiveness.

Store:
- test procedure;
- tester;
- date;
- sample;
- evidence;
- result;
- findings;
- corrective actions.

---

# 15. EVIDENCE MANAGEMENT — MAKE THIS EXCELLENT

Evidence is central to certification readiness.

Create a complete evidence platform.

Evidence types:

- document;
- screenshot;
- API result;
- system record;
- configuration;
- log;
- report;
- meeting record;
- approval;
- training record;
- assessment;
- audit result;
- monitoring metric;
- source-code result;
- automated scan.

Every evidence record must contain:

```text
Evidence ID
Title
Description
Source
Collection Method
Collected At
Collected By
Hash
Version
Owner
Classification
Retention
Expiration
Related Requirement(s)
Related Control(s)
Related Risk(s)
Related AI System(s)
Related Audit
Review Status
Reviewer
Review Date
```

Support evidence:

```text
Requested
Missing
Collected
Under Review
Accepted
Rejected
Expired
Superseded
```

## Evidence provenance

For automated evidence, record:

```text
Connector
Endpoint
Collection Time
API Request Metadata
Source Object ID
Transformation
Collector Version
Hash
```

This is critical for audit defensibility.

---

# 16. INTEGRATION HUB

Build an extensible connector framework.

Do not hard-code integrations directly into business logic.

Architecture:

```text
Integration
   |
   +-- Provider
   +-- Credentials
   +-- Connection Test
   +-- Sync Configuration
   +-- Scheduler
   +-- Data Mapping
   +-- Evidence Mapping
   +-- Risk Mapping
   +-- Finding Mapping
   +-- Sync Logs
   +-- Errors
```

Initial integrations should include:

## Microsoft Azure / Azure AI Foundry

Where supported by available APIs/permissions, retrieve:

- AI projects;
- models;
- model deployments;
- model versions;
- endpoints;
- AI resources;
- configurations;
- monitoring metadata;
- evaluations;
- safety configuration;
- responsible AI artifacts;
- usage;
- logs/telemetry where accessible;
- owners/tags;
- resource relationships.

Do not assume that every piece of information is available from every Foundry/Azure API.

The UI must clearly show:

```text
Available
Not Available
Permission Required
Not Supported by Provider API
Not Configured
```

## OpenAI

Connector architecture should support retrieval of provider/account/model/deployment/usage/configuration information available through the authorized API.

## Anthropic

Support authorized retrieval of relevant organization/API/model information where the provider exposes it.

## Future connectors

Design the connector SDK so new integrations can be added without changing the core domain model.

Potential future connectors:

- AWS AI services
- Google Vertex AI
- GitHub
- Azure DevOps
- Jira
- ServiceNow
- Datadog
- Splunk
- Microsoft Purview
- Microsoft Entra ID
- SharePoint
- cloud storage
- SIEM
- ticketing systems
- model registries
- ML platforms

---

# 17. CONNECTOR SECURITY

Never store raw API secrets in normal database columns.

Use:

- secret manager abstraction;
- encryption at rest;
- secret references;
- credential rotation;
- minimal scopes;
- connection testing;
- access logging;
- revocation;
- tenant isolation.

Implement:

```text
CredentialCreated
CredentialUpdated
CredentialRotated
CredentialAccessed
CredentialRevoked
```

Never expose credentials to frontend clients.

---

# 18. AUTOMATED DISCOVERY ENGINE

The platform should be able to discover AI systems.

Pipeline:

```text
CONNECT
   ↓
DISCOVER
   ↓
NORMALIZE
   ↓
CLASSIFY
   ↓
MATCH
   ↓
CREATE/UPDATE ASSET
   ↓
MAP REQUIREMENTS
   ↓
MAP CONTROLS
   ↓
IDENTIFY RISKS
   ↓
REQUEST HUMAN VALIDATION
```

Never automatically mark compliance merely because data was discovered.

Example:

Azure detects a deployed model.

The system may create:

```text
Candidate AI System
```

Then request human confirmation:

- Is this actually in scope?
- Who owns it?
- What is the intended use?
- What data does it use?
- What stakeholders are affected?
- What is the risk?
- What level of human oversight exists?

---

# 19. AI-ASSISTED GOVERNANCE ENGINE

Use AI to accelerate work, not to fabricate compliance.

AI capabilities:

### Requirement interpretation
Explain requirements in plain language.

### Gap analysis
Compare current evidence against expected implementation.

### Risk suggestions
Suggest potential risks based on system characteristics.

### Impact assessment assistance
Suggest questions and possible impact areas.

### Control mapping
Suggest controls based on risks and systems.

### Policy generation
Generate policy drafts based on actual organization data.

### Evidence analysis
Analyze uploaded evidence and identify:
- which requirements it supports;
- which controls it supports;
- gaps;
- inconsistencies;
- stale information.

### Audit preparation
Generate audit questions.

### Corrective action
Suggest remediation plans.

### Reports
Generate narrative executive summaries.

Every AI-generated artifact must include:

```text
Generated by AI
Model
Generation Time
Prompt/Task ID
Source Data
Human Review Status
Reviewer
Approval
```

AI suggestions never automatically become "compliant."

---

# 20. POLICY AND DOCUMENT GENERATOR

Build a document-generation engine.

Templates:

- AI Policy
- AI Risk Management Policy
- AI Development Policy
- Responsible AI Policy
- AI Data Governance Policy
- AI Security Policy
- AI Incident Management Procedure
- AI Lifecycle Procedure
- Human Oversight Procedure
- AI Supplier Management Procedure
- AI Impact Assessment Procedure
- AI Monitoring Procedure
- AI Audit Procedure
- Evidence Management Procedure
- Exception Management Procedure

Each generated document should use actual organization data.

Version lifecycle:

```text
Draft
Review
Approval
Published
Superseded
Archived
```

Support:
- PDF;
- DOCX;
- Markdown;
- HTML.

---

# 21. TASK / WORKFLOW ENGINE

Create a workflow engine capable of turning compliance requirements into work.

Tasks can originate from:

- requirement;
- control;
- risk;
- impact assessment;
- audit finding;
- incident;
- corrective action;
- policy review;
- evidence request;
- lifecycle gate;
- management review;
- connector discovery.

Task fields:

```text
Title
Description
Source
Owner
Reviewer
Priority
Due Date
Status
Dependencies
Evidence Required
Approval Required
Escalation
SLA
```

Support recurring tasks.

---

# 22. INCIDENT MANAGEMENT

Create AI-specific incident management.

Incident categories:

- unexpected model behavior;
- incorrect output;
- bias/fairness concern;
- privacy issue;
- security incident;
- hallucination;
- data quality;
- model drift;
- unauthorized use;
- unsafe content;
- regulatory concern;
- supplier incident;
- human oversight failure.

Incident lifecycle:

```text
Reported
Triaged
Investigating
Contained
Remediating
Resolved
Closed
Lessons Learned
```

Every significant incident should be capable of triggering:

- risk reassessment;
- impact reassessment;
- control review;
- corrective action;
- management notification.

---

# 23. AUDIT MODULE — ISO 19011-ALIGNED

Build a complete audit platform.

## Audit Programme

Support:

- annual audit programme;
- risk-based audit planning;
- audit scope;
- objectives;
- criteria;
- resources;
- auditor competence;
- independence;
- audit methods;
- remote audits;
- combined/integrated audits.

## Audit

Lifecycle:

```text
Planned
Scheduled
Preparing
Opening
Fieldwork
Evidence Collection
Findings
Closing
Report
Follow-up
Closed
```

## Audit evidence

Link evidence to:

```text
Criterion
Requirement
Control
AI System
Finding
```

## Audit findings

Support:

```text
Conformity
Nonconformity
Observation
Opportunity for Improvement
```

Never force all organizations to use the same classification.

## Audit conclusion

Generate conclusion from actual findings and evidence.

The application must preserve the traceability:

```text
AUDIT CRITERION
      ↓
EVIDENCE
      ↓
FINDING
      ↓
CONCLUSION
      ↓
CORRECTIVE ACTION
      ↓
FOLLOW-UP
```

This follows the evidence-based and risk-based audit concepts emphasized by ISO 19011.

---

# 24. NONCONFORMITY / CAPA

Build a proper CAPA module.

For every nonconformity:

```text
Requirement
Control
Evidence
Finding
Immediate Correction
Root Cause
Corrective Action
Owner
Due Date
Effectiveness Test
Verification
Closure
```

Root cause methods:

- 5 Whys
- Fishbone
- custom

Support effectiveness verification.

---

# 25. MANAGEMENT REVIEW

Build a management-review module.

Inputs:

- previous decisions;
- objectives;
- KPIs;
- risk status;
- impact assessments;
- audit results;
- incidents;
- nonconformities;
- corrective actions;
- regulatory changes;
- AI portfolio changes;
- stakeholder feedback;
- supplier performance;
- resource needs.

Outputs:

- decisions;
- actions;
- resource changes;
- objective changes;
- policy changes;
- scope changes;
- risk decisions;
- improvement actions.

Generate a formal management-review record.

---

# 26. KPI / METRICS ENGINE

Build configurable metrics.

Examples:

### Compliance
- % requirements implemented
- % requirements with evidence
- % requirements audit-ready
- % active controls implemented
- % active controls effective

### Risk
- inherent risk
- residual risk
- high-risk AI systems
- overdue risk reviews
- risk trend
- accepted risks
- expired risk acceptances

### AI portfolio
- total AI systems
- systems by risk
- systems by provider
- systems by lifecycle
- systems with missing owners
- systems with missing assessments

### Evidence
- evidence completeness
- evidence freshness
- rejected evidence
- expired evidence
- automated evidence coverage

### Audit
- findings by severity
- overdue findings
- CAPA aging
- audit readiness

### Training
- completion;
- overdue training;
- competency gaps.

---

# 27. REPORTING PLATFORM — AT LEAST 20 HIGH-VALUE REPORTS

Reports must be dynamic, filterable and exportable.

Each report should support:

- date range;
- business unit;
- department;
- AI system;
- provider;
- risk;
- control domain;
- owner;
- lifecycle;
- status;
- custom filters.

Export:

- PDF
- XLSX
- CSV
- JSON
- HTML

Create at least these reports:

1. **Executive AI Governance Dashboard**
   - overall AIMS posture, risks, controls, evidence, incidents, audits.

2. **ISO/IEC 42001 Clause Compliance Report**
   - Clauses 4–10, status, owners, evidence, gaps.

3. **Statement of Applicability Report**
   - all Annex A controls, applicability, justification, implementation status.

4. **Control Effectiveness Report**
   - active controls, test results, effectiveness, evidence.

5. **AI Risk Register Report**
   - inherent/residual risk, treatment, owner, due dates.

6. **High-Risk AI Systems Report**
   - systems requiring management attention.

7. **AI Inventory Report**
   - complete portfolio of AI systems/assets.

8. **AI Lifecycle Compliance Report**
   - lifecycle stage and missing gates.

9. **AI Impact Assessment Report**
   - assessments, impacts, findings and mitigations.

10. **AI Data Governance Report**
    - datasets, provenance, quality, classification, owners, issues.

11. **AI Provider / Third-Party Risk Report**
    - providers, suppliers, contracts, risks, controls.

12. **AI Incident Report**
    - incidents, severity, root causes, corrective actions.

13. **Audit Readiness Report**
    - requirements/control/evidence readiness.

14. **Evidence Coverage Report**
    - requirements and controls with/without objective evidence.

15. **Evidence Freshness Report**
    - expiring/stale evidence.

16. **Nonconformity & CAPA Report**
    - findings, root causes, corrective actions and aging.

17. **Management Review Report**
    - inputs, decisions, open actions.

18. **AI Governance KPI Report**
    - configurable KPI trends.

19. **Connector / Discovery Report**
    - discovered systems, sync status, unmapped assets, errors.

20. **Regulatory / Requirement Impact Report**
    - external requirements mapped to AI systems and controls.

21. **Human Oversight Report**
    - AI systems requiring human oversight, assigned supervisors, exceptions.

22. **AI Model Change Report**
    - model versions, deployments, changes and triggered reassessments.

23. **Risk Treatment Effectiveness Report**
    - treatment plans, residual risk changes and effectiveness.

24. **Certification Readiness Pack**
    - executive summary plus detailed requirement/control/evidence index.

25. **Audit Evidence Pack**
    - structured export containing evidence references and traceability.

The report engine must be generic enough to create additional reports without creating a new code path for every report.

---

# 28. DYNAMIC REPORT BUILDER

Implement a report-builder UI.

Users can:

1. choose a data source;
2. choose dimensions;
3. choose metrics;
4. apply filters;
5. choose visualization;
6. save report;
7. schedule report;
8. share report;
9. export report.

Visualizations:

- KPI cards;
- tables;
- bar charts;
- line charts;
- heat maps;
- risk matrices;
- trend charts;
- status distributions;
- Sankey/relationship views where useful;
- graphs.

Create a query abstraction rather than allowing arbitrary SQL from users.

---

# 29. DOMAIN DASHBOARDS

Provide dashboards for:

- Executive
- ISO 42001
- AI Risk
- AI Inventory
- AI Lifecycle
- Data Governance
- Responsible AI
- AI Security
- Third Parties
- Audit
- Evidence
- Incidents
- CAPA
- Training
- Integrations

Dashboards should only show relevant active controls/workflows based on SoA.

---

# 30. NOTIFICATION ENGINE

Support:

- email;
- in-app;
- webhook-ready.

Notifications for:

- overdue tasks;
- expiring evidence;
- high risks;
- risk acceptance expiration;
- failed lifecycle gates;
- incidents;
- failed connector syncs;
- audit findings;
- upcoming audits;
- management review;
- policy expiration;
- control review;
- regulatory changes.

Notification rules must be configurable.

---

# 31. SEARCH

Implement enterprise search across:

- AI systems;
- assets;
- requirements;
- controls;
- risks;
- evidence;
- policies;
- audits;
- findings;
- incidents;
- reports.

Support semantic search architecture where appropriate.

Search result should show why the result matched.

---

# 32. TRACEABILITY GRAPH

This is a differentiating feature.

Create a visual relationship graph:

```text
Requirement
   ↓
Control
   ↓
Risk
   ↓
AI System
   ↓
Asset/Data/Model
   ↓
Evidence
   ↓
Audit
   ↓
Finding
   ↓
Corrective Action
```

Users should be able to start from any node and traverse the governance chain.

Example:

Click an AI system:

```text
AI System
 ├── Requirements
 ├── Controls
 ├── Risks
 ├── Impact Assessments
 ├── Data
 ├── Models
 ├── Providers
 ├── Incidents
 ├── Evidence
 ├── Audits
 └── Tasks
```

---

# 33. CHANGE MANAGEMENT

Changes to important objects must be versioned.

Examples:

- AI system changed;
- model changed;
- provider changed;
- dataset changed;
- scope changed;
- control applicability changed;
- policy changed;
- risk changed.

A significant change should be capable of triggering:

```text
Change Request
   ↓
Impact Analysis
   ↓
Risk Reassessment
   ↓
Impact Reassessment
   ↓
Control Review
   ↓
Approval
   ↓
Implementation
   ↓
Evidence
```

---

# 34. EXCEPTION MANAGEMENT

Create formal exceptions.

Fields:

- requirement/control;
- reason;
- business justification;
- risk;
- compensating control;
- owner;
- approver;
- expiration;
- review date.

Never allow permanent exceptions without explicit expiration/review governance.

---

# 35. REGULATORY / EXTERNAL REQUIREMENTS

Create a module to map external obligations to:

```text
Regulatory Requirement
      ↓
ISO Requirement
      ↓
Control
      ↓
AI System
      ↓
Evidence
```

Architecture should support future regulatory sources without hard-coding one jurisdiction.

---

# 36. TRAINING / COMPETENCE

Implement:

- roles;
- competency requirements;
- training requirements;
- courses;
- completion;
- evidence;
- expiration;
- competency gaps.

Map competency requirements to AI roles.

---

# 37. UI / UX

Build a professional enterprise UI.

Principles:

- clean;
- information-dense but readable;
- responsive;
- accessible;
- keyboard navigable;
- consistent;
- audit-friendly.

Navigation:

```text
Dashboard
AI Portfolio
 ├── AI Systems
 ├── Models
 ├── Datasets
 ├── Providers
 └── Lifecycle

ISO 42001
 ├── Requirements
 ├── Statement of Applicability
 ├── Controls
 ├── Policies
 └── Objectives

Risk
 ├── Risk Register
 ├── Assessments
 ├── Treatment
 └── Risk Acceptance

Impact
 ├── Assessments
 ├── Findings
 └── Mitigations

Evidence
 ├── Evidence Library
 ├── Requests
 └── Reviews

Audit
 ├── Audit Programme
 ├── Audits
 ├── Findings
 └── CAPA

Operations
 ├── Incidents
 ├── Tasks
 ├── Changes
 └── Exceptions

Reports
 ├── Dashboards
 ├── Reports
 └── Report Builder

Integrations
 ├── Providers
 ├── Connections
 ├── Discovery
 └── Sync History

Administration
 ├── Organization
 ├── Users
 ├── Roles
 ├── Teams
 ├── Notifications
 └── Settings
```

---

# 38. HOME DASHBOARD

The executive dashboard should immediately answer:

```text
AIMS Readiness
Risk Exposure
Control Effectiveness
Evidence Coverage
AI Portfolio
Open Findings
Open CAPAs
Incidents
Audit Readiness
Overdue Tasks
```

Use drill-down navigation.

Never display a single "ISO compliant: 87%" number without explaining the calculation.

Every score must be transparent.

---

# 39. SCORING ENGINE

Create a transparent scoring engine.

Examples:

```text
Requirement Readiness
Evidence Coverage
Control Implementation
Control Effectiveness
Risk Exposure
Audit Readiness
```

Every score must expose:

- formula;
- numerator;
- denominator;
- exclusions;
- source records;
- timestamp.

Avoid misleading "compliance scores."

Use terms such as:

- Readiness
- Coverage
- Implementation
- Effectiveness
- Exposure

unless a formal certification status is actually established.

---

# 40. SECURITY

Treat the platform itself as an enterprise security product.

Implement:

- strong authentication;
- authorization;
- tenant isolation;
- secure secret storage;
- encryption;
- audit logs;
- immutable compliance decisions;
- rate limiting;
- CSRF protection where relevant;
- secure headers;
- input validation;
- output encoding;
- file scanning;
- malware-safe document processing;
- secure download URLs;
- least privilege;
- secure API design;
- dependency scanning;
- SAST;
- secrets scanning;
- logging;
- monitoring.

Never expose:
- API keys;
- provider secrets;
- internal credentials;
- sensitive evidence;
- cross-tenant data.

---

# 41. AUDIT TRAIL

Every significant governance action needs a tamper-evident audit record.

Examples:

```text
RequirementStatusChanged
ControlApplicabilityChanged
RiskCreated
RiskScored
RiskTreatmentApproved
RiskAccepted
ImpactAssessmentApproved
EvidenceCollected
EvidenceApproved
PolicyApproved
AIModelChanged
AISystemChanged
IncidentCreated
FindingCreated
CAPAClosed
AuditStarted
AuditConcluded
ManagementReviewApproved
ConnectorSynced
UserPermissionChanged
```

Store:

```text
Actor
Timestamp
Action
Object
Previous State
New State
Reason
IP / session metadata where appropriate
Correlation ID
```

---

# 42. DATA RETENTION

Every evidence/document class should support configurable:

- retention period;
- expiration;
- legal hold;
- deletion workflow;
- archival.

Do not silently delete compliance records.

---

# 43. API DESIGN

Create a documented API.

Organize by domain:

```text
/api/organizations
/api/users
/api/roles
/api/standards
/api/requirements
/api/controls
/api/soa
/api/ai-systems
/api/assets
/api/models
/api/datasets
/api/providers
/api/risks
/api/impact-assessments
/api/lifecycle
/api/evidence
/api/policies
/api/audits
/api/findings
/api/capa
/api/incidents
/api/objectives
/api/kpis
/api/reports
/api/integrations
/api/tasks
/api/management-reviews
```

Use:
- typed request/response schemas;
- consistent errors;
- pagination;
- filtering;
- sorting;
- authorization;
- audit logging.

---

# 44. BACKGROUND PROCESSING

Use background jobs for:

- connector synchronization;
- AI discovery;
- evidence collection;
- report generation;
- document generation;
- notifications;
- scheduled assessments;
- KPI calculation;
- risk reassessment;
- stale evidence detection.

Jobs must be:

- retryable;
- idempotent;
- observable;
- auditable.

---

# 45. OBSERVABILITY

Implement:

- structured logs;
- metrics;
- traces;
- job monitoring;
- connector health;
- application health;
- error tracking.

Every asynchronous operation should have a correlation ID.

---

# 46. TESTING

Do not stop at compiling.

Create:

## Unit tests
- scoring;
- risk calculations;
- applicability logic;
- permission logic;
- status transitions;
- report calculations.

## Integration tests
- database;
- API;
- connectors;
- evidence ingestion;
- workflow engine.

## End-to-end tests
At minimum:

1. Create organization.
2. Create users.
3. Configure roles.
4. Define context.
5. Define scope.
6. Create AI system.
7. Perform risk assessment.
8. Perform impact assessment.
9. Configure SoA.
10. Activate controls.
11. Create evidence.
12. Generate policy.
13. Run audit.
14. Create finding.
15. Create CAPA.
16. Close CAPA.
17. Generate management review.
18. Generate reports.

## Security tests

Test:
- cross-tenant access;
- privilege escalation;
- unauthorized evidence access;
- secret exposure;
- broken object authorization.

---

# 47. SEED DATA

Create a deterministic seed process.

The seed must include:

- ISO/IEC 42001 standard metadata;
- Clauses 4–10;
- every requirement represented by the source material;
- Annex A domains;
- complete control catalog available from the supplied source context;
- ISO/IEC 23894 risk structure;
- ISO 19011 audit structure;
- example roles;
- example risk scales;
- example lifecycle;
- example report definitions;
- example dashboard definitions.

Do not fake normative text.

Where source detail is insufficient, create a clearly labeled placeholder such as:

```text
SOURCE_DETAIL_REQUIRED
```

rather than inventing it.

---

# 48. DEMO ORGANIZATION

Create a realistic demo tenant.

Example:

```text
Demo Financial Services
```

Include:

- multiple AI systems;
- multiple providers;
- high/medium/low risks;
- multiple business units;
- example controls;
- evidence;
- incidents;
- audit findings;
- policies;
- reports;
- management review.

The demo must make the product immediately understandable.

---

# 49. AI SYSTEM EXAMPLE DATA

Create examples such as:

```text
Customer Support AI
Fraud Detection Model
Credit Decisioning Model
Marketing Propensity Model
Internal Coding Assistant
Document Classification Model
Generative AI Assistant
AI Agent
```

Do not imply that demo data represents actual organizational compliance.

---

# 50. AI AGENTS AS FIRST-CLASS AI SYSTEMS

Because modern organizations increasingly deploy AI agents, the AI System model must support:

- agent;
- sub-agent;
- tool;
- tool permissions;
- memory;
- autonomy level;
- actions;
- external systems;
- human approval;
- spending limits;
- execution limits;
- prohibited actions;
- kill switch;
- logs;
- model;
- prompts/instructions;
- versions.

Represent:

```text
Agent
 ├── Model
 ├── Tools
 ├── Permissions
 ├── Data
 ├── Memory
 ├── Users
 ├── External Systems
 ├── Human Supervisor
 ├── Risks
 └── Controls
```

---

# 51. AI GOVERNANCE MATURITY

Create an optional maturity model.

Dimensions:

- Governance
- Risk
- Lifecycle
- Data
- Security
- Transparency
- Human Oversight
- Monitoring
- Incident Management
- Third Parties
- Audit
- Continual Improvement

Maturity:

```text
Initial
Developing
Defined
Managed
Optimized
```

This is a management metric, not an ISO certification score.

---

# 52. REPORT SCHEDULING

Allow users to schedule reports:

```text
Daily
Weekly
Monthly
Quarterly
Custom
```

Recipients can be:

- users;
- teams;
- roles.

Delivery:

- in-app;
- email;
- downloadable package.

---

# 53. CERTIFICATION READINESS MODE

Create a dedicated "Certification Readiness" workspace.

It should calculate:

```text
Requirements
Controls
Evidence
Risks
Audits
Findings
CAPA
Management Review
```

Provide a checklist:

```text
Context ready?
Scope approved?
Policy approved?
Risk methodology approved?
Risk assessments complete?
Impact assessments complete?
SoA approved?
Controls implemented?
Evidence sufficient?
Monitoring active?
Internal audit completed?
Management review completed?
CAPA closed?
```

But never declare that an organization is "ISO certified."

The system can say:

- Ready for internal review
- Audit-ready
- Certification preparation status

Actual certification is determined by the external certification process.

---

# 54. IMPORT / EXPORT

Support:

- CSV;
- XLSX;
- JSON;
- PDF;
- DOCX;
- Markdown.

Import workflows must include:

```text
Upload
Validate
Preview
Map Fields
Detect Duplicates
Confirm
Import
Report
```

Never overwrite compliance data silently.

---

# 55. DATA QUALITY ENGINE

Create quality checks for:

- missing owners;
- orphaned controls;
- risks without treatment;
- controls without evidence;
- AI systems without assessments;
- expired evidence;
- missing scope relationships;
- duplicate AI systems;
- duplicate providers;
- stale assessments.

Create a Data Quality Dashboard.

---

# 56. CONTINUAL IMPROVEMENT ENGINE

Create improvement records.

Sources:

- audit findings;
- incidents;
- risk changes;
- stakeholder feedback;
- regulatory changes;
- monitoring;
- management review;
- lessons learned.

Workflow:

```text
Improvement Opportunity
    ↓
Analysis
    ↓
Action
    ↓
Owner
    ↓
Implementation
    ↓
Effectiveness
    ↓
Closure
```

---

# 57. DOMAIN-SPECIFIC CONTROL PACKAGES

Create architecture for domain packages.

Examples:

- Financial Services AI
- Healthcare AI
- HR AI
- Marketing AI
- Customer Service AI
- Software Development AI
- Generative AI
- AI Agents

Packages may contain:

- additional risks;
- questions;
- evidence templates;
- recommended controls;
- report definitions;
- assessment templates.

Clearly label these as organizational/domain guidance rather than ISO requirements.

---

# 58. DOCUMENTATION TO PRODUCE

Claude Code must create:

```text
README.md
ARCHITECTURE.md
SECURITY.md
DATA_MODEL.md
API.md
INTEGRATIONS.md
COMPLIANCE_MODEL.md
REPORTING.md
AUDIT_MODEL.md
RISK_MODEL.md
AI_SYSTEM_MODEL.md
DEPLOYMENT.md
OPERATIONS.md
TESTING.md
THREAT_MODEL.md
```

Also create:

```text
docs/
  requirements/
  architecture/
  database/
  api/
  security/
  integrations/
  operations/
  testing/
  compliance/
```

---

# 59. DATABASE / MIGRATION RULES

Use migrations.

Never modify production schemas manually.

Every schema change must include:

- migration;
- seed compatibility;
- rollback consideration;
- tests.

Use foreign keys and indexes.

Index heavily queried fields:

- tenant_id;
- status;
- owner_id;
- requirement_id;
- control_id;
- risk_id;
- ai_system_id;
- created_at;
- due_date.

---

# 60. PERFORMANCE

The system must be designed for:

- thousands of AI systems;
- tens of thousands of risks;
- hundreds of thousands of evidence records;
- large audit histories;
- many concurrent users;
- scheduled connector synchronization.

Use:
- pagination;
- server-side filtering;
- background processing;
- caching where justified;
- asynchronous report generation;
- indexed queries.

---

# 61. NO MOCK-ONLY IMPLEMENTATION

Do not build a UI full of fake buttons.

Every important action must be wired to:

```text
UI
 ↓
API
 ↓
Domain Service
 ↓
Database
 ↓
Audit Log
```

For external integrations where credentials are unavailable:

- implement the connector architecture;
- implement configuration UI;
- implement credential validation interface;
- implement sync job;
- implement provider adapter;
- use a safe mock adapter only for local/demo mode;
- clearly mark demo/mock data.

Do not claim that a provider integration works if it has not been validated against the actual provider API.

---

# 62. ERROR HANDLING

All operations must provide useful errors.

For connector errors, distinguish:

```text
Authentication Failed
Authorization Failed
Rate Limited
Provider Unavailable
Invalid Configuration
Unsupported Endpoint
Partial Sync
Data Mapping Error
Timeout
Unknown Error
```

Provide remediation guidance.

---

# 63. AUDITABILITY OF THE APPLICATION ITSELF

The platform is itself a compliance system.

Therefore every important governance operation must be:

- attributable;
- timestamped;
- traceable;
- reviewable;
- exportable.

A user must be able to answer:

> Who changed this control's applicability, when, why, what was the previous value, and who approved it?

The same principle must apply to:
- risk acceptance;
- policy approval;
- evidence approval;
- scope;
- audit findings;
- CAPA closure;
- management review.

---

# 64. IMPLEMENTATION STRATEGY

Do not attempt to write everything blindly in one pass.

Use an iterative engineering plan.

## Phase 0 — Architecture

Produce:

- repository assessment;
- architecture;
- domain model;
- ERD;
- permission model;
- module map;
- API map;
- event map.

## Phase 1 — Foundation

Implement:

- authentication;
- tenants;
- users;
- RBAC;
- audit trail;
- database;
- design system.

## Phase 2 — Standards Engine

Implement:

- standards;
- versions;
- clauses;
- requirements;
- Annex A;
- controls;
- mappings.

## Phase 3 — AIMS Core

Implement:

- context;
- interested parties;
- scope;
- policy;
- objectives;
- roles.

## Phase 4 — Risk / Impact

Implement:

- AI inventory;
- assets;
- risk engine;
- assessments;
- treatments;
- residual risk;
- impact assessments.

## Phase 5 — SoA / Controls

Implement:

- SoA;
- applicability;
- control activation;
- control workflows;
- evidence mapping.

## Phase 6 — Lifecycle / Operations

Implement:

- lifecycle;
- incidents;
- changes;
- exceptions;
- tasks;
- monitoring.

## Phase 7 — Evidence / Documents

Implement:

- evidence;
- documents;
- policies;
- document generation.

## Phase 8 — Integrations

Implement:

- connector framework;
- Azure AI Foundry;
- OpenAI;
- Anthropic;
- discovery;
- sync.

## Phase 9 — Audit / CAPA

Implement:

- audit programme;
- audit;
- evidence;
- findings;
- CAPA;
- follow-up.

## Phase 10 — Reporting

Implement:

- dashboards;
- report engine;
- 20+ report definitions;
- dynamic builder;
- exports;
- scheduling.

## Phase 11 — Management Review / Certification Readiness

Implement:

- management review;
- certification readiness;
- evidence pack;
- executive reporting.

## Phase 12 — Hardening

Implement:

- security testing;
- performance;
- observability;
- accessibility;
- documentation;
- deployment.

---

# 65. EVENT-DRIVEN BEHAVIOR

Use domain events.

Examples:

```text
AI_SYSTEM_CREATED
AI_SYSTEM_CHANGED
MODEL_VERSION_CHANGED
DATASET_CHANGED
CONTROL_ACTIVATED
CONTROL_DEACTIVATED
RISK_CREATED
RISK_ESCALATED
RISK_ACCEPTED
RISK_ACCEPTANCE_EXPIRING
IMPACT_ASSESSMENT_COMPLETED
EVIDENCE_COLLECTED
EVIDENCE_EXPIRED
INCIDENT_CREATED
AUDIT_FINDING_CREATED
CAPA_CREATED
CAPA_OVERDUE
POLICY_APPROVED
SCOPE_CHANGED
CONNECTOR_SYNC_COMPLETED
REGULATORY_REQUIREMENT_CHANGED
```

Example:

```text
MODEL_VERSION_CHANGED
        ↓
Change Request
        ↓
Risk Reassessment Required
        ↓
Impact Assessment Review
        ↓
Control Review
        ↓
Evidence Request
```

---

# 66. UX FOR REQUIREMENT IMPLEMENTATION

For every ISO requirement, provide a progress workspace:

```text
Requirement
 ├── Interpretation
 ├── Owner
 ├── Status
 ├── Tasks
 ├── Controls
 ├── Risks
 ├── AI Systems
 ├── Policies
 ├── Evidence
 ├── Audit Questions
 ├── Findings
 └── Corrective Actions
```

The user should never need to hunt through five modules to determine whether a requirement is implemented.

---

# 67. "WHY IS THIS REQUIRED?" EXPLANATION

Every compliance object should provide contextual explanation.

Examples:

- Why this requirement exists.
- Why this control was activated.
- Which risk caused the control to be selected.
- Which AI systems are affected.
- Which evidence demonstrates implementation.
- What happens if evidence expires.
- Which audit criteria will test it.

This is where AI assistance can provide substantial value.

---

# 68. QUALITY GATES

Before declaring a module complete, verify:

### Functional
- CRUD works.
- validation works.
- authorization works.
- audit trail works.
- relationships work.
- filters work.
- search works.

### Compliance
- requirement traceability works.
- control traceability works.
- evidence traceability works.
- risk traceability works.
- audit traceability works.

### UX
- empty states;
- loading;
- errors;
- pagination;
- accessibility;
- mobile/responsive behavior.

### Security
- tenant isolation;
- permission checks;
- secret handling;
- file security.

### Testing
- unit;
- integration;
- E2E.

---

# 69. FINAL ACCEPTANCE CRITERIA

The implementation is not complete until a user can perform this complete workflow:

```text
Create Organization
      ↓
Define Context
      ↓
Define Interested Parties
      ↓
Define AIMS Scope
      ↓
Inventory AI Systems
      ↓
Connect Azure/OpenAI/Anthropic
      ↓
Discover AI Assets
      ↓
Validate AI Systems
      ↓
Perform AI Risk Assessment
      ↓
Perform AI Impact Assessment
      ↓
Define AI Objectives
      ↓
Review Annex A
      ↓
Complete Statement of Applicability
      ↓
Activate Applicable Controls
      ↓
Assign Control Owners
      ↓
Generate AI Policy
      ↓
Create Procedures
      ↓
Create Implementation Tasks
      ↓
Collect Evidence
      ↓
Test Controls
      ↓
Monitor Risks/Controls
      ↓
Manage Incidents
      ↓
Perform Internal Audit
      ↓
Generate Findings
      ↓
Create CAPA
      ↓
Verify CAPA Effectiveness
      ↓
Perform Management Review
      ↓
Generate Certification Readiness Report
      ↓
Generate Audit Evidence Pack
```

Every step must create persistent, traceable records.

---

# 70. MOST IMPORTANT PRODUCT PRINCIPLE

Do not build:

> "software that tells me whether I checked a box."

Build:

> **"software that operates the organization's AI Management System and continuously creates the evidence needed to demonstrate that it is functioning."**

The core data relationship should be:

```text
ORGANIZATION
      ↓
CONTEXT
      ↓
SCOPE
      ↓
AI PORTFOLIO
      ↓
RISKS + IMPACTS
      ↓
OBJECTIVES
      ↓
REQUIREMENTS
      ↓
SoA
      ↓
ACTIVE CONTROLS
      ↓
PROCESSES
      ↓
EVIDENCE
      ↓
MONITORING
      ↓
AUDIT
      ↓
FINDINGS
      ↓
CORRECTIVE ACTION
      ↓
MANAGEMENT REVIEW
      ↓
CONTINUAL IMPROVEMENT
```

This relationship must be reflected in the database, APIs, UI, workflows and reports.

---

# 71. IMPORTANT ENGINEERING RULE

Do not ask for confirmation after every small implementation step.

Make reasonable engineering decisions, document them and proceed.

Ask for clarification only when a decision would materially affect:

- security;
- compliance interpretation;
- irreversible data migration;
- production infrastructure;
- external system actions;
- legal/regulatory interpretation.

Otherwise implement the most maintainable enterprise solution.

---

# 72. DELIVERY EXPECTATION

At the end of the implementation, provide:

1. working application;
2. database schema;
3. migrations;
4. seed data;
5. API;
6. authentication/RBAC;
7. all core modules;
8. integrations framework;
9. implemented initial connectors;
10. evidence management;
11. report engine;
12. 20+ reports;
13. dashboards;
14. audit module;
15. risk/impact modules;
16. SoA;
17. policies;
18. management review;
19. certification-readiness workspace;
20. automated tests;
21. security documentation;
22. deployment documentation;
23. architecture documentation;
24. known limitations.

Also provide a final implementation matrix:

| Module | Status | Tests | Notes |
|---|---|---|---|
| Authentication | | | |
| RBAC | | | |
| Requirements | | | |
| SoA | | | |
| Controls | | | |
| AI Inventory | | | |
| Risk | | | |
| Impact | | | |
| Lifecycle | | | |
| Evidence | | | |
| Policies | | | |
| Incidents | | | |
| Audit | | | |
| CAPA | | | |
| Management Review | | | |
| Integrations | | | |
| Reports | | | |
| Dashboards | | | |
| Notifications | | | |
| Search | | | |
| Security | | | |

---

# 73. FINAL INSTRUCTION TO CLAUDE CODE

Start by inspecting the repository and the available source Markdown documents.

Then create the architecture and implementation plan.

Do not reduce scope simply because the system is large.

Implement the platform incrementally, but maintain the complete domain architecture from the beginning.

Prioritize correctness, traceability, security, auditability, maintainability and professional UX.

Whenever a requirement from ISO/IEC 42001 is represented in the application, it must be possible to navigate from:

**requirement → control → risk → AI system → evidence → audit → finding → corrective action → management review**

and back again.

The resulting application should be credible as the foundation of a real enterprise **ISO/IEC 42001 AI Management System implementation platform**.
