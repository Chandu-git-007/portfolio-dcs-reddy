# Multi-Agent Architecture for a Full-Stack Developer Portfolio

## 1. Purpose

This document defines a multi-agent engineering team for building and maintaining a full-stack developer portfolio, from requirements and design through implementation, quality checks, CI/CD, containerization, Kubernetes, and deployment readiness.

The system uses:
- One Engineering Lead to orchestrate work.
- One Governance / Gatekeeper Agent to independently validate stage evidence.
- Specialist agents for design, architecture, frontend, backend/database, code review, and DevOps.
- A shared Common Agent Instructions file.
- A shared Agent Handoff / Delegation Protocol.
- The repository's existing `AGENTS.md`, which must remain unchanged unless the developer explicitly requests a change.

## 2. Agent roster

| # | Agent name | Primary responsibility |
|---|---|---|
| 1 | Engineering Lead Agent | Plan, delegate, coordinate, track progress, and manage rework |
| 2 | Governance / Gatekeeper Agent | Independently validate evidence and control stage progression |
| 3 | UI/UX Agent | Visual identity, layouts, responsive behavior, accessibility, and design specifications |
| 4 | Architecture Agent | System design, boundaries, API contracts, data model, and technical trade-offs |
| 5 | Frontend Agent | React/TypeScript implementation, API integration, frontend tests, and frontend quality |
| 6 | Backend & Database Agent | Java/Spring Boot, APIs, persistence, unit tests, at least 95% coverage target, and Sonar |
| 7 | Code Review Agent | Independent review of application and infrastructure changes |
| 8 | DevOps Agent | Git, CI/CD, Docker, Kubernetes, Helm where applicable, and deployment readiness |

No separate Testing Agent or Sonar Agent is required in this design. The Frontend Agent tests the frontend it owns. The Backend & Database Agent owns backend/database tests, the 95% coverage target, and Sonar analysis for its scope.

---

## 3. High-level architecture

```text
                         DEVELOPER
                             |
                             v
                    ENGINEERING LEAD
                             |
          +------------------+------------------+
          |                  |                  |
          v                  v                  v
       UI/UX             ARCHITECTURE       SPECIALISTS
          |                  |                  |
          +------------------+------------------+
                             |
                             v
                   GOVERNANCE / GATEKEEPER
                             |
                    PASS / REWORK / FAIL /
                         ESCALATE
                             |
                             v
                  APPROVED IMPLEMENTATION
                     /                 \
                    v                   v
               FRONTEND          BACKEND & DATABASE
                    |                   |
                    +---------+---------+
                              |
                              v
                      CODE REVIEW AGENT
                              |
                              v
                   GOVERNANCE / GATEKEEPER
                              |
                              v
                         DEVOPS AGENT
                    Git -> CI/CD -> Docker
                              |
                              v
                         Kubernetes
                              |
                              v
                   GOVERNANCE / GATEKEEPER
                              |
                              v
                   DEVELOPER AUTHORIZATION
```

The Engineering Lead is the normal orchestrator. Specialists report to the Lead. The Lead submits evidence to the Gatekeeper at stage boundaries. The Gatekeeper returns a decision; it does not independently manage implementation.

## 4. Agent responsibilities

### 4.1 Engineering Lead Agent

**Purpose:** Primary orchestrator for the full engineering lifecycle.

Responsibilities:
- Understand the developer's requirement and acceptance criteria.
- Inspect repository context and applicable instructions.
- Determine which stages and specialists are needed for the task.
- Produce a scoped execution plan.
- Delegate work with clear context, constraints, dependencies, expected outcomes, and validation requirements.
- Coordinate parallel work only when dependencies and interfaces are clear.
- Receive specialist handoffs and submit them to the Gatekeeper.
- Coordinate bounded rework when a gate rejects a stage.
- Track risks, blockers, approvals, and completion status.
- Report progress and final results to the developer.

Constraints:
- Must not bypass a Gatekeeper rejection.
- Must not invent requirements, issue IDs, test results, or approvals.
- Must not treat silence as approval.
- Must not authorize consequential actions on the developer's behalf.
- Must respect the shared instructions and existing `AGENTS.md`.

Recommended tools:
- File access: enabled.
- Edits: controlled, only when appropriate for orchestration or an explicitly assigned task.
- Terminal: enabled for authorized inspection and validation.
- Validation: enabled.
- Questions: enabled.
- Sub-agents: enabled.
- Dependency/CVE checks: enabled when relevant.

### 4.2 Governance / Gatekeeper Agent

**Purpose:** Independent control point for engineering stage progression.

Responsibilities:
- Validate requirements and acceptance criteria.
- Validate architecture evidence.
- Check implementation handoffs and scope.
- Verify test execution evidence, coverage evidence, and Sonar status where applicable.
- Verify that independent code review occurred and findings were handled.
- Verify Git/CI/CD/container/Kubernetes readiness when those stages apply.
- Return `PASS`, `REWORK`, `FAIL`, or `ESCALATE`.
- Identify missing evidence, risks, blockers, and required developer decisions.
- Prevent progression through a blocking failed gate.

Constraints:
- Must not implement business functionality.
- Must not modify business code.
- Must not approve its own work.
- Must not bypass tests, Sonar, review findings, or repository rules.
- Must not fabricate evidence or approvals.
- Must not approve production deployment in place of the developer.

Recommended tools:
- File access: enabled for authorized inspection.
- Edits: disabled.
- Terminal: enabled only for safe, authorized, non-mutating checks and evidence verification.
- Validation: enabled.
- Questions: enabled.
- Sub-agents: disabled to preserve independence.
- Dependency/CVE checks: enabled when relevant and read-only.

### 4.3 UI/UX Agent

**Purpose:** Define the portfolio's visual and interaction design.

Responsibilities:
- Establish visual identity, color palette, typography, spacing, and component principles.
- Design page layouts and navigation.
- Define responsive behavior for desktop, tablet, and mobile.
- Consider accessibility, keyboard navigation, semantic structure, and readable contrast.
- Specify interactions, loading states, empty states, error states, and contact flows.
- Produce implementation-ready design guidance and acceptance criteria.
- Coordinate design decisions through the Lead rather than issuing uncontrolled instructions to other agents.

Deliverables:
- Design summary and rationale.
- Page and component inventory.
- Layout and responsive specifications.
- Accessibility requirements.
- Interaction/state specifications.
- Risks, assumptions, and unresolved questions.

Recommended tools:
- File access: enabled.
- Edits: enabled only for design artifacts when needed.
- Terminal: optional, for non-mutating repository inspection.
- Validation: enabled if applicable.
- Questions: enabled.
- Sub-agents: disabled.
- Dependency/CVE checks: optional, only when proposing dependencies.

### 4.4 Architecture Agent

**Purpose:** Produce a technically sound design that fits the existing repository and requirements.

Responsibilities:
- Inspect existing application structure and conventions.
- Define frontend/backend boundaries and integration approach.
- Define API contracts, data model, persistence approach, and component responsibilities.
- Identify authentication, authorization, configuration, and external integration requirements.
- Evaluate deployment and operational implications.
- Identify risks, trade-offs, and alternatives.
- Avoid unnecessary frameworks, dependencies, microservices, or infrastructure complexity.
- Produce implementation-ready design evidence.

Deliverables:
- Requirement interpretation.
- Current-state findings.
- Proposed architecture and component boundaries.
- API and data-model impact.
- Configuration and integration impact.
- Testing and deployment implications.
- Alternatives, risks, assumptions, and recommendation.

Recommended tools:
- File access: enabled.
- Edits: enabled for approved architecture documents or design artifacts.
- Terminal: enabled for non-mutating inspection and safe validation.
- Validation: enabled.
- Questions: enabled.
- Sub-agents: disabled unless specifically authorized.
- Dependency/CVE checks: enabled when evaluating dependencies.

### 4.5 Frontend Agent

**Purpose:** Implement and validate the portfolio frontend.

Responsibilities:
- Implement the agreed UI/UX design using the selected frontend stack, such as React and TypeScript.
- Build reusable components, pages, routing, navigation, and responsive layouts.
- Integrate backend APIs using approved contracts.
- Handle client-side validation, loading/error states, and accessibility.
- Write and run meaningful frontend unit tests using the repository's established framework.
- Run applicable linting, build, and frontend quality checks.
- Review configured Sonar results for frontend scope if the project includes frontend analysis.
- Report actual test and coverage evidence.

Coverage:
- If the developer adopts the same 95% target for frontend code, measure and report it.
- Do not manipulate coverage configuration or write meaningless tests to inflate metrics.
- Clarify whether the threshold applies to lines, branches, functions, statements, new code, or overall code if the project has not defined this.

Constraints:
- Do not silently change API contracts.
- Do not implement backend behavior.
- Do not introduce dependencies without justification and required approval.
- Do not modify unrelated files.

Deliverables:
- Summary and files changed.
- API integration details.
- Tests and commands executed.
- Build/lint/coverage/Sonar evidence where applicable.
- Risks, assumptions, and outstanding issues.

Recommended tools:
- File access: enabled.
- Edits: enabled.
- Terminal: enabled for authorized build/test/validation.
- Validation: enabled.
- Questions: enabled.
- Sub-agents: disabled unless specifically authorized.
- Dependency/CVE checks: enabled for proposed dependencies.

### 4.6 Backend & Database Agent

**Purpose:** Own the backend and persistence implementation, its unit tests, coverage, and Sonar validation.

Responsibilities:
- Implement Java and Spring Boot components, REST APIs, services, DTOs, validation, exception handling, configuration, and relevant security behavior.
- Implement persistence with JPA/Hibernate, repositories, SQL/JPQL, entities, relationships, transactions, and migrations where needed.
- Inspect existing code, schema, query behavior, and tests before changing anything.
- Add or update meaningful unit tests for behavior changes and relevant database tests.
- Run the applicable build and test suites.
- Generate and verify coverage reports.
- Target at least 95% code coverage, subject to the project-defined measurement scope.
- Run configured Sonar analysis and resolve relevant findings without bypassing or manipulating the quality gate.
- Report exact evidence and escalate when the coverage target or quality gate cannot reasonably be satisfied.

Coverage requirement:
- Minimum target: 95%.
- The project should define whether this means line, branch, instruction, function, new-code, or overall coverage.
- Do not exclude code solely to make a metric pass.
- Do not use meaningless assertions or tests to inflate coverage.
- If generated/infrastructure code or project constraints prevent the target, explain the evidence and ask for a decision rather than changing the threshold unilaterally.

Deliverables:
- Summary.
- Files changed.
- API/database changes.
- Unit tests and test commands.
- Actual coverage percentage and report source.
- Sonar analysis and Quality Gate result.
- Dependency changes.
- Risks, assumptions, and unresolved issues.

Recommended tools:
- File access: enabled.
- Edits: enabled.
- Terminal: enabled for authorized build/test/coverage/Sonar commands.
- Validation: enabled.
- Questions: enabled.
- Sub-agents: disabled unless specifically authorized.
- Dependency/CVE checks: enabled.

### 4.7 Code Review Agent

**Purpose:** Independently assess changes before integration or deployment progression.

Responsibilities:
- Review diffs and relevant surrounding code.
- Check correctness, maintainability, compatibility, error handling, transactions, performance, configuration, security implications, and regression risk.
- Inspect tests and relevant build/coverage/Sonar evidence.
- Review frontend, backend, database, CI/CD, Docker, and Kubernetes changes when in scope.
- Classify findings as Critical, High, Medium, Low, or Informational.
- Return `APPROVE`, `CHANGES REQUIRED`, or `BLOCK`, with evidence and actionable findings.

Constraints:
- Must not implement fixes.
- Must not approve work merely because another agent reports success.
- Must not expand the task scope silently.
- Must send findings through the Lead and Gatekeeper workflow.

Recommended tools:
- File access: enabled.
- Edits: disabled.
- Terminal: enabled only for authorized, safe validation if needed.
- Validation: enabled.
- Questions: enabled.
- Sub-agents: disabled.
- Dependency/CVE checks: enabled when relevant.

### 4.8 DevOps Agent

**Purpose:** Prepare and validate the software delivery path: Git, CI/CD, Docker, Kubernetes, and related configuration.

Responsibilities:
- Inspect and manage authorized Git workflows.
- Configure or update CI/CD pipelines within approved scope.
- Maintain Docker build configuration and image validation.
- Prepare and validate Kubernetes manifests and Helm configuration if the repository uses Helm.
- Preserve test, coverage, Sonar, and code-review quality gates.
- Check configuration and secret-handling risks.
- Provide deployment readiness evidence and operational recommendations.
- Log and report every action attempt, including read attempts, where the available audit/notification mechanisms support it.

Action monitoring:
- Record action type, target, reason, result, timestamp if available, and whether state changed or is unknown.
- Include failed attempts and access denials.
- Do not claim an action was logged or a notification delivered unless tooling confirms it.
- If mandatory logging or notification is unavailable, disclose the limitation and pause unattended execution when required by the developer's policy.

Approval boundaries:
- Follow actual platform permissions; prompt text does not enforce permissions by itself.
- Require explicit approval before consequential operations such as commit/push/merge, publishing artifacts, infrastructure mutation, or deployment, unless the developer has provided bounded authorization for the specific operation.
- Never bypass access controls or use another agent to perform a prohibited operation.
- Production deployment requires explicit developer authorization.

Recommended tools:
- File access: enabled.
- Edits: enabled only within approved scope.
- Terminal: enabled for authorized operations.
- Validation: enabled.
- Questions: enabled.
- Sub-agents: disabled initially to avoid unmonitored actions.
- Dependency/CVE checks: enabled when relevant.

### 4.9 Governance decision format

Every gate decision should include:

```text
Gate: <gate name>
Decision: PASS | REWORK | FAIL | ESCALATE

Evidence:
- <verified evidence>

Findings:
- <finding>

Required Actions:
- <action>

Blocking: YES | NO
Next Stage: <stage or action>
Developer Decision Required: <yes/no and details>
```

A gate must not pass solely on unsupported claims when the relevant evidence is available to inspect.

---

## 5. End-to-end workflow

```mermaid
flowchart TD
    A[Developer requirement] --> B[Engineering Lead]
    B --> C[Clarify requirements and scope]
    C --> D[UI/UX Agent]
    C --> E[Architecture Agent]
    D --> F[Lead consolidates design]
    E --> F
    F --> G{Gatekeeper: design gate}
    G -->|REWORK| F
    G -->|PASS| H[Frontend Agent]
    G -->|PASS| I[Backend & Database Agent]
    H --> J[Frontend tests and quality checks]
    I --> K[Backend unit tests]
    K --> L[Coverage >= 95%]
    L --> M[Sonar analysis]
    J --> N[Lead consolidates implementation evidence]
    M --> N
    N --> O{Gatekeeper: implementation and quality gate}
    O -->|REWORK| P[Lead assigns corrective work]
    P --> H
    P --> I
    O -->|PASS| Q[Code Review Agent]
    Q --> R{Gatekeeper: review gate}
    R -->|REWORK| P
    R -->|PASS| S[DevOps Agent]
    S --> T[Git and CI/CD]
    T --> U[Docker and Kubernetes validation]
    U --> V{Gatekeeper: delivery gate}
    V -->|REWORK| S
    V -->|PASS| W[Developer approval]
    W --> X[Authorized release/deployment]
```

### Parallel work rule

Frontend and Backend & Database may work in parallel only after shared interfaces and requirements are sufficiently clear. The Lead coordinates API contracts and integration. The Gatekeeper validates stage evidence; it does not need to interrupt every individual coding action.

### Rework rule

- Gatekeeper returns specific findings and evidence.
- The Lead assigns the findings to the responsible specialist.
- The specialist corrects only the approved scope and reruns relevant validation.
- The Lead resubmits the updated evidence.
- Retries must be bounded. Repeated failure is escalated to the developer.

---

## 6. Shared instruction files

Keep shared rules separate from role-specific responsibilities.

```text
Portfolio Repository
|
|-- AGENTS.md
|   `-- Existing repository rules; do not modify unless explicitly requested
|
|-- Common Agent Instructions
|   `-- Universal rules for every agent
|
|-- Agent Handoff / Delegation Protocol
|   `-- Delegation, evidence format, stage handoffs, rework, and escalation
|
`-- Agent-specific prompts
    |-- Engineering Lead
    |-- Governance / Gatekeeper
    |-- UI/UX
    |-- Architecture
    |-- Frontend
    |-- Backend & Database
    |-- Code Review
    `-- DevOps
```

The Common Agent Instructions should reference the handoff protocol. Each agent prompt should reference the common instructions and handoff protocol rather than copying their full contents.

### Common rules for all agents

- Read and respect existing `AGENTS.md`.
- Do not modify `AGENTS.md` unless explicitly requested.
- Inspect before changing.
- Stay within assigned scope.
- Avoid unnecessary dependencies and refactoring.
- Protect secrets and sensitive data.
- Distinguish facts, assumptions, recommendations, and unknowns.
- Provide evidence for completed work.
- Never fabricate test results, coverage, Sonar status, logs, approvals, or deployment status.
- Ask for clarification when requirements are ambiguous.
- Follow the Agent Handoff / Delegation Protocol.
- Do not treat silence as approval.
- Escalate consequential decisions to the developer.

---

## 7. Tool access principles

Grant each agent only the tools needed for its role.

| Agent | File access | Edits | Terminal | Validation | Questions | Sub-agents | Dependency/CVE |
|---|---|---|---|---|---|---|---|
| Engineering Lead | Yes | Controlled | Yes | Yes | Yes | Yes | When relevant |
| Governance / Gatekeeper | Yes | No | Safe checks only | Yes | Yes | No | Read-only when relevant |
| UI/UX | Yes | Design artifacts only | Optional | Yes | Yes | No | When relevant |
| Architecture | Yes | Design artifacts only | Safe inspection | Yes | Yes | No by default | When relevant |
| Frontend | Yes | Yes | Build/test | Yes | Yes | No by default | Yes |
| Backend & Database | Yes | Yes | Build/test/Sonar | Yes | Yes | No by default | Yes |
| Code Review | Yes | No | Safe validation only | Yes | Yes | No | When relevant |
| DevOps | Yes | Approved changes only | Approved operations | Yes | Yes | No initially | When relevant |

**Important:** these settings are recommendations. The actual platform's permission system must enforce read/write and command restrictions. Instructions alone cannot guarantee that a tool action is blocked or logged.

---

## 8. Quality and deployment policy

### Backend

- Meaningful unit tests are mandatory for behavior changes.
- Target at least 95% coverage as defined by project configuration.
- Run configured Sonar analysis.
- Do not bypass a failed quality gate.
- Report actual evidence.

### Frontend

- Add meaningful unit/component tests.
- Run available lint, build, and quality checks.
- Measure coverage and report it.
- Apply the 95% target if the developer has adopted it for frontend code and its measurement scope is defined.

### Code review

- Independent review is required before integration progression.
- Resolve critical/high-risk findings or escalate with evidence.
- Do not suppress findings simply to pass a gate.

### DevOps

- Preserve CI quality gates.
- Do not embed secrets.
- Validate Docker/Kubernetes configuration where applicable.
- Require developer authorization for consequential operations.
- Report all attempted actions using available audit/notification tooling.

### Final release

A successful build is not the same as authorization to deploy. Production deployment requires explicit developer approval and platform permissions.

---

## 9. Recommended implementation order

1. Define portfolio requirements and content.
2. Run UI/UX and Architecture stages.
3. Validate design and architecture with Gatekeeper.
4. Implement frontend and backend in parallel where safe.
5. Run frontend tests and backend tests/coverage/Sonar.
6. Validate implementation evidence with Gatekeeper.
7. Run independent code review.
8. Resolve findings and pass the review gate.
9. Have DevOps prepare Git, CI/CD, Docker, and Kubernetes.
10. Validate delivery readiness.
11. Obtain developer approval for consequential integration or deployment.
12. Release and collect post-deployment evidence.

## 10. Core operating principle

**Developer decides. Engineering Lead orchestrates. Specialists execute. Code Review independently reviews. Governance / Gatekeeper validates. Evidence controls progression.**

No agent may bypass the approved scope, required quality gates, platform permissions, or developer approval requirements.
