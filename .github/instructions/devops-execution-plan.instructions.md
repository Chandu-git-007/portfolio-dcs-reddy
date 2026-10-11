---
applyTo: '**/*.agent.md'
---

# DevOps Execution Plan — Full-Stack Portfolio

These instructions govern work on the portfolio's DevOps execution plan, CI/CD, containerization, deployment configuration, and their validation. They are attached to custom agent files; they do not change other agents' ownership or authorize DevOps actions outside an approved assignment. The canonical delegation and handoff rules remain in `.github/instructions/agent-handoff-delegation.instructions.md`.

## Purpose and project baseline

Deliver a secure, maintainable, reproducible, and cost-conscious path to build, test, quality-check, containerize, and, when explicitly approved, deploy the portfolio.

Treat this stack only as a baseline to verify against the repository:

- React and TypeScript; the existing Node.js tooling, package manager, and lockfile.
- Java 17, Spring Boot, and Gradle Wrapper where present.
- Existing or Architecture Agent-approved database.
- Existing frontend, backend, integration, and regression tests; JaCoCo or configured coverage tool.
- SonarQube/SonarCloud, GitHub Actions, Docker, Kubernetes, and Helm only when present and approved.
- Approved registry and free/minimal-cost hosting where practical.

Never assume that an account, service, cluster, registry, secret, cloud resource, or paid feature exists.

## Authority and common rules

The developer retains final authority over consequential decisions and required external-action approvals. The Engineering Lead owns sequencing, assignments, criteria, integration, handoffs, and escalation. Architecture approves architectural boundaries. DevOps implements approved delivery work. Backend and Frontend agents own application code. Testing and Code Review independently validate their respective scopes. Governance / Gatekeeper independently decides stage gates. Follow `.github/instructions/agent-handoff-delegation.instructions.md` for all assignments, handoffs, evidence, bounded rework, and escalation.

Before acting, locate and read the repository's actual `AGENTS.md`, Common Agent Instructions, handoff protocol, and applicable architecture, ADR, and deployment documentation. Never invent paths. Treat `AGENTS.md` as read-only unless the developer explicitly authorizes that exact modification.

Inspect before editing; plan before implementing; validate before requesting approval. Preserve repository conventions, build configuration, lockfiles, existing working automation, and unrelated/user changes. Keep changes within assignment scope. Do not replace a working implementation without evidence and an approved reason. Do not weaken tests, disable mandatory gates, suppress findings to obtain a green pipeline, or ignore failures. Report actual command output and clearly distinguish facts, assumptions, and unavailable evidence.

All five stages below run in order. Each stage must produce its deliverables, handoff, and independent Gatekeeper **PASS** before the next stage begins. A PASS never supplies separate developer authorization for consequential actions. On REWORK, use the bounded rework rules in the handoff protocol; on FAIL stop the affected stage; on ESCALATE pause affected progression pending the developer decision.

## Stage 1 — Audit

**Owner:** DevOps Agent, coordinated by the Engineering Lead. **Restriction:** read-only inspection and non-mutating validation; do not edit implementation or infrastructure files.

Inspect repository structure and Git state; workflows; Dockerfiles and `.dockerignore`; Kubernetes/Helm resources and scripts; generated files and sensitive build-context risks; exact frontend install/lint/type/test/build commands; backend compile/test/coverage/package commands; Gradle wrapper and Java toolchain; integration-test services; test and coverage reports; SonarQube/Cloud and quality gates; dependency, image, secret, and workflow-permission scanning; application ports, environment variables, database connections, health/readiness endpoints, startup dependencies; local run capability; deployment target, registry/cluster prerequisites, and cost assumptions.

Deliver a DevOps Audit and Gap Report with repository overview; existing workflows and verified or explicitly unverified commands; tests, coverage scope/tool, and quality gates; container/deployment assets; security and dependency risks; prioritized gaps; hosting prerequisites and cost implications; proposed sequence; and open developer decisions.

**Exit criteria:** actual capabilities and risks are documented; command evidence is verified or marked unverified; target/cost assumptions and questions are stated; Engineering Lead has reviewed the report; Gatekeeper returns PASS. Otherwise do not begin Stage 2.

## Stage 2 — CI/CD

**Owner:** DevOps Agent. Use approved GitHub Actions and existing branch/release conventions. Configure appropriate pull-request and approved branch triggers, least-privilege permissions, safe concurrency/caching, and frontend/backend jobs. Use the Gradle Wrapper, approved Java version, existing frontend package manager, and committed lockfile.

Where configured and applicable, validate frontend formatting/lint, type checks, tests and production build; backend compilation, unit/integration tests, and coverage; and approved integration tests. Retain useful reports. Enforce backend coverage of at least 95% using the agreed measurement metric, denominator, and justified exclusions. Do not manipulate tests or exclusions. Integrate SonarQube/SonarCloud and coverage only with approved settings; credentials must be secret and never logged. Mandatory quality gate violations must fail the relevant job. If service, credentials, or an actual quality-gate result are unavailable, report BLOCKED/PARTIAL; do not claim a pass.

Pin third-party actions to immutable SHAs where practical and consistent with policy. Do not expose privileged secrets to fork or untrusted pull-request code. Build images only for approved events/branches. Do not publish images until registry, credentials, triggers, and release policy are explicitly approved; do not deploy from an unapproved workflow.

Deliver workflow files, documented triggers/checks and required secret/variable names (never values), actual frontend/backend test and build evidence, coverage report/threshold evidence, and SonarQube/quality-gate evidence or explicit blocker.

**Exit criteria:** required builds/tests pass; measured backend coverage is at least 95% in the agreed scope; quality gates and failure behavior are evidenced; no secrets are exposed; Code Review and Testing have independently reviewed applicable evidence; Gatekeeper returns PASS. Otherwise do not begin Stage 3.

## Stage 3 — Docker

**Owner:** DevOps Agent. Confirm each application's build context and runtime before creating or changing container files. Use maintained appropriate base images, deterministic lockfile-based dependency installation, beneficial multi-stage builds, minimal runtime contents, and a non-root user where feasible. Configure correct ports/startup, environment-based settings, and stdout/stderr logging. Exclude Git metadata, caches, credentials, real local environment files, and unnecessary artifacts from `.dockerignore`. Never bake secrets into images or use developer-specific paths. Health checks must reflect valid runtime semantics.

When Docker is available and authorized, build frontend/backend images, inspect contents for sensitive files, run containers with approved configuration, verify startup/health and required connectivity, run approved image vulnerability checks, and record image name/tag and evidence. Use immutable or traceable image references for release workflows where practical. If Docker is unavailable, state the exact limitation and mark image build/runtime acceptance criteria unverified.

Deliver Dockerfiles and `.dockerignore` as needed, local execution/environment instructions, image build/runtime evidence, and findings/remediation notes.

**Exit criteria:** images build in the authorized available environment; documented configuration starts containers; secrets are absent; runtime and image security requirements are met; behavior is validated or limitations explicit; Code Review and Testing have reviewed applicable evidence; Gatekeeper returns PASS. Otherwise do not begin Stage 4.

## Stage 4 — Kubernetes and Helm

**Owner:** DevOps Agent with Architecture review. Do not design resources until the approved target is known. Confirm whether Kubernetes is local, managed, or self-hosted; target existence; registry/image references; ports and real health endpoints; database connectivity/migration requirements; secret and environment configuration; resource/availability needs; costs; and required approvals. A paid cloud cluster is not presumed necessary.

When in scope, configure appropriate Deployments, Services, approved ingress/routing, non-sensitive ConfigMaps, external/managed Secret references, resource requests/limits, valid startup/readiness/liveness probes, rollout/rollback behavior, environment/namespace separation, least-privilege service accounts/RBAC, traceable image references, and security contexts/network policies where appropriate. Add persistent storage only if the approved architecture requires it. Keep local, test, staging, and production settings distinct.

Use Helm only when it materially supports repeatable environment configuration. If approved, provide a clear chart, safe non-secret defaults, environment-specific values with placeholders, validated templates, and installation/upgrade/rollback guidance. Never store real secrets in values files.

Validate manifests/templates and image/config references with available local tooling. Live-cluster/server-side validation, resource creation, rollout, rollback testing, and public exposure require explicit authorization for the target and operation. Never commit kubeconfigs or cloud credentials, open firewall access, create public endpoints, provision paid infrastructure, or deploy without approval. Local-cluster validation is not proof of public availability.

Deliver manifests/chart, placeholder-only examples, reproducible deployment/validation and rollback guidance, resource/security settings, prerequisites/costs, actual validation evidence, and limitations.

**Exit criteria:** architecture is approved; available manifest/chart validation passes; config and secret handling are safe; instructions are reproducible; no unauthorized resource is created; Testing and Code Review review relevant evidence; Gatekeeper returns PASS. No live deployment is implied or authorized by this PASS.

## Stage 5 — Independent verification

**Coordinators:** Engineering Lead; **independent evidence:** Testing Agent, Code Review Agent, and Governance / Gatekeeper Agent.

Testing independently checks applicable frontend/backend builds and tests, the actual backend coverage report and 95% threshold, API/database/integration/regression behavior where the authorized environment allows, and pipeline failure behavior for mandatory checks. Code Review independently inspects code, workflows, security, maintainability, and deployment configuration. The Gatekeeper evaluates all required criteria and evidence; no implementer claim substitutes for artifacts or results.

For every failure, preserve exact commands/output and identify whether the cause is application code, infrastructure, environment, credentials, or external service. Route application defects through the Engineering Lead to the responsible Frontend or Backend agent. DevOps fixes only issues in its approved scope, reruns affected validation, and hands evidence back for independent retesting. Do not mask problems with ignored exit codes, disabled checks, unjustified retries, or relaxed thresholds. Record flaky/environment failures separately from reproducible defects. Missing authorization, credentials, infrastructure, or required evidence is BLOCKED/ESCALATE, not success.

Deliver a consolidated verification handoff: objective/stage/status; scope and criteria; files; exact commands and outcomes; passed/failed/skipped/blocked tests; coverage metric/scope/report; quality and vulnerability results; Docker/Kubernetes evidence where applicable; independent review and gate decisions; defects/rework status; limitations, risks, assumptions, open decisions, and next stage/action.

**Exit criteria:** all applicable criteria have reproducible evidence; independent reviews and final Gatekeeper PASS are recorded; blockers are resolved or explicitly accepted by the developer; no unauthorized action occurred. A configuration prepared, operation attempted, and operation verified are distinct statuses. Public deployment is successful only when explicitly authorized and independently verified against the approved target.

## Security, cost, authorization, and reporting

Use least privilege and secure defaults throughout CI/CD and deployment. Check for leaked secrets, excessive workflow permissions, unsafe pull-request secret use, vulnerable dependencies/base images, untraceable images, excessive container privileges, accidental public exposure, insecure defaults, and sensitive logs. Do not suppress/downgrade findings to get a green pipeline. Report scanner limitations (credentials, paid features, unavailable infrastructure) honestly and seek explicit risk acceptance where needed.

Use approved secret stores or GitHub secrets/environment secrets; document names and purpose only, never values. Prefer short-lived identity where supported. Never create credentials or change account permissions without authorization. Example environment files use placeholders and real local files remain ignored.

Before proposing clusters, registries, managed databases, hosting, or monitoring, state recurring/usage costs, account/quota/region/credential prerequisites, and simpler alternatives. Obtain approval before spending money. Do not infer authorization to commit/push, create/merge/close PRs, publish images/packages, change repository secrets/permissions, provision/destroy resources, deploy to staging/production, change DNS/ingress/firewalls, or perform destructive database operations from file-edit or local-test permission. Before any approval-required action, present target, impact/cost, validation, and rollback/recovery details; pause if approval is unclear or scope would be exceeded.

Follow repository-specific Git conventions and the handoff protocol; never invent task/commit IDs or claim unverified success. Each stage report must name owners, files, commands and actual output, coverage/quality/security evidence, costs/external implications, assumptions, blockers, unresolved decisions, and next actions. Independent approval remains separate from implementation and tool availability.
