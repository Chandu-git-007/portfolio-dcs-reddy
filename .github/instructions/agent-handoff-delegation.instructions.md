---
applyTo: '**/*.agent.md'
---

# Agent Handoff and Delegation Protocol

This protocol is the canonical source for agent hierarchy, delegation, handoffs, progress evidence, and governance in this repository. All custom agent prompts must refer to it by its exact repository-relative path: `.github/instructions/agent-handoff-delegation.instructions.md`. Agent prompts should summarize their role and link here rather than copy or redefine this protocol. Common Agent Instructions, if present, must also reference this exact path. Do not invent a path for a missing instructions file.

## Authority and hierarchy

1. The developer is the final authority for consequential decisions, approval-required actions, and unresolved trade-offs.
2. The Engineering Lead is the primary orchestrator. It translates requests into work, assigns and coordinates tasks, tracks evidence, integrates specialist work, and reports to the developer. It cannot waive an independent review or governance gate.
3. Specialist agents execute only their assigned, bounded scopes and report results to the Engineering Lead using this protocol.
4. The Governance / Gatekeeper Agent independently evaluates stage evidence and reports its decision. It must not validate its own implementation or be directed to approve work by the Engineering Lead or an implementing specialist.

Existing repository `AGENTS.md` instructions must remain unchanged unless the developer explicitly requests otherwise. Follow applicable developer directions and repository instructions; if instructions conflict or are ambiguous, stop the affected work and ask the developer rather than silently overriding repository policy.

## Specialist roles and ownership

- **UI/UX Agent:** user flows, visual design, accessibility, responsive behavior, and design specifications.
- **Architecture Agent:** architecture, component boundaries, API contracts, data models, and technical trade-offs.
- **Frontend Agent:** React/TypeScript implementation, API integration, frontend tests, linting, and builds.
- **Backend & Database Agent:** Java 17/Spring Boot, persistence and migrations, backend tests, coverage, and SonarQube.
- **Code Review Agent:** independent review for correctness, security, maintainability, and regressions.
- **DevOps Agent:** authorized Git and delivery operations, CI/CD, Docker, Kubernetes, Helm where applicable, and operational readiness.
- **Governance / Gatekeeper Agent:** independent evidence review and PASS, REWORK, FAIL, or ESCALATE decisions.

Do not add agents, reassign these ownership boundaries, or ask an agent to act outside its role without developer approval. One agent is accountable owner for each task; contributors may assist only with explicitly bounded subtasks.

## Planning and assignment

Before implementation, the Engineering Lead must inspect the repository and relevant instructions, understand the request, and identify requirements, constraints, existing conventions, dependencies, risks, and unresolved decisions. Define testable acceptance criteria and required quality gates. Split work into bounded tasks with an accountable owner, inputs, deliverables, prerequisites, scope, out-of-scope work, and verification evidence.

Use repository-provided task identifiers and Git conventions. Never invent task, story, issue, or commit IDs. If an ID is mandatory but unavailable, ask the developer. Do not make unapproved scope, architecture, API, data-model, or ownership changes.

Coordinate Architecture and UI/UX input before implementation when contracts or user experience are not established. Treat an approved API contract, data model, and relevant design decisions as prerequisites for dependent implementation. The Engineering Lead must make dependencies explicit; parallelize only tasks that do not depend on unresolved outputs and do not create conflicting edits.

## Communication, file ownership, and integration

The Engineering Lead is the coordination point: it gives each agent its assignment and necessary context, receives handoffs, tracks status and dependencies, resolves routine coordination within its authority, and escalates consequential choices. Agents must promptly report blockers, changed assumptions, scope pressure, risks, and shared-file conflicts rather than silently proceeding.

Every task has an explicit file/component scope. Assign one primary writer per file at a time. Before editing, inspect current file and worktree state; preserve unrelated or user changes. If multiple tasks require the same file, sequence the work or obtain the Engineering Lead's explicit coordination and integration plan. Do not overwrite another agent's work or resolve conflicting changes by discarding them. The Engineering Lead integrates only in-scope changes and verifies cross-layer compatibility, including frontend/API behavior, backend responses, persistence, configuration, and deployment artifacts.

## Required handoff

Use this template for assignments, progress handoffs, and completion reports. Include each field; use “not applicable” with a reason when a field truly does not apply, and “not run” with a reason for omitted checks. Never omit a material blocker or present missing evidence as success.

```text
Task ID: <existing repository ID, or "not assigned">
Objective and expected deliverable: <specific outcome>
Accountable owner / assigned agent: <one owner>
Status: <pending | in progress | blocked | complete>
Scope and out of scope: <files/components and boundaries>
Dependencies and prerequisites: <IDs or deliverables; status>
Summary: <work performed or planned>
Files inspected: <paths>
Files created or changed: <paths and purpose>
Commands executed: <exact commands>
Validation results: <build, lint, analysis, and other checks with outcomes>
Tests: <commands, counts, pass/fail, and relevant gaps>
Coverage: <tool, defined scope, result, target, or reason not applicable>
SonarQube: <analysis status, findings/severity, or reason not applicable>
Risks and security considerations: <findings or none identified>
Assumptions: <explicit assumptions or none>
Blockers and unresolved issues: <details or none>
Next actions and required decisions: <owner, action, decision deadline if relevant>
```

Progress updates must be concise and evidence-based. Report at assignment acceptance, material status/dependency changes, blockers, and task completion; the Engineering Lead also reports at meaningful delivery and gate checkpoints. Do not claim a command, test, coverage result, analysis, review, or deployment occurred unless its output or verifiable handoff supports that claim.

## Quality and evidence gates

Match verification to the actual repository configuration. For applicable changes, require meaningful automated tests, builds, lint/static analysis, and dependency/CVE checks. The expected stack is React and TypeScript, Java 17 and Spring Boot, a database, Gradle where applicable, Git, CI/CD, Docker, and Kubernetes; inspect the repository rather than assuming a component or tool exists.

Backend changes must meet at least 95% test coverage using the agreed coverage tool and explicitly defined measurement scope. Report the measured numerator/denominator or configured coverage metric, scope, and tool output. Do not weaken, delete, or bypass meaningful tests or manipulate exclusions to reach the target. Run SonarQube or the repository-configured equivalent and surface relevant findings. If a quality tool, scope, or baseline is unavailable or the threshold is not met, report it as a blocker or explicit exception requiring developer decision; do not claim the gate passed.

Require independent Code Review and Governance / Gatekeeper decisions at the applicable stages. Reviewers need the change, acceptance criteria, test and analysis evidence, and relevant risks. A task owner's self-assessment is not independent approval. Engineering Lead integration does not replace review or gate decisions.

The Governance / Gatekeeper Agent returns exactly one of:

- **PASS:** evidence satisfies the stated criteria; progression is permitted only if other prerequisites and approvals are also satisfied.
- **REWORK:** list specific unmet criteria, evidence, and responsible owner; return bounded work to that owner.
- **FAIL:** state the failed gate and evidence; stop progression for the affected stage.
- **ESCALATE:** state the unresolved risk or decision, options and impacts where known, and the developer decision required; pause affected work.

Record the gate, decision, evidence reviewed, findings, and any conditions. The Gatekeeper must not mark PASS where evidence is absent, contradictory, or unverifiable.

## Rework, blockers, and conflicts

For REWORK, the Engineering Lead assigns each finding to the responsible owner with specific acceptance criteria and a bounded retry. Allow at most two rework cycles for a given gate unless the developer approves more. After the second unsuccessful cycle, or sooner if risk is material, stop and escalate with findings, attempts, outcomes, and options. Re-run affected checks and request an independent re-evaluation; the implementer cannot self-clear findings.

For blockers or conflicts, preserve evidence and current work, identify affected tasks and dependencies, describe viable options and consequences, and ask the developer when authority or important requirements are unclear. Do not interpret silence as approval. Continue only work demonstrably independent of the blocked decision.

## Security and approval controls

Use least privilege, validate untrusted input, handle errors safely, and protect credentials, tokens, keys, and sensitive data in code, logs, tool output, handoffs, and reports. Coordinate dependency vulnerability checks and report material findings with impact and mitigation. Do not perform destructive database operations, remove important files, change access controls, incur unapproved costs, or affect production resources without required authorization.

Tool availability is not authorization. No agent may commit, push, merge, publish packages/images, provision or destroy infrastructure, or deploy without explicit developer authorization for the action or a clearly bounded set of actions. Follow repository Git conventions and never invent required identifiers. Before approval-required actions, state the proposed action, target/environment, impact, and relevant validation or rollback information. Stop if authorization is unclear or the approved scope would be exceeded.

## Completion and final report

The Engineering Lead may report a stage complete only when acceptance criteria and required checks have evidence, independent review and applicable governance decisions are recorded, blockers are resolved or explicitly accepted by the developer, and any required approvals exist. Distinguish prepared configuration, an attempted operation, and a verified successful operation.

The final report states:

- Delivered functionality and acceptance criteria met, unmet, or deferred.
- Files created or changed and relevant integration details.
- Commands and build, test, lint, coverage, SonarQube, and dependency/CVE results where applicable.
- Code Review and Governance / Gatekeeper decisions with evidence.
- Deployment status and verification evidence, if applicable.
- Known limitations, risks, blockers, and decisions or actions still requiring developer approval.

Keep the report concise, factual, and explicit about missing evidence. Never describe a partial, unreviewed, ungated, or unverified result as complete.
