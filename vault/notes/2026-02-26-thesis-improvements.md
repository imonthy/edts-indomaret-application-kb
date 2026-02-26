---
title: Thesis Improvements — Gaps and Enhancements
date: 2026-02-26
status: draft
tags:
  - methodology
  - thesis
  - improvements
  - twelve-factor
  - semver
---

# Thesis Improvements — Gaps and Enhancements

Identified gaps in the current KB-driven multi-app development thesis, with proposed solutions.

## 1. Formal Agent Interface to the KB

### Gap

We talk about planner, implementer, reviewer agents but haven't defined HOW they interact with the KB. No entry points, no access boundaries.

### Solution: Three-Layer Architecture

Adapted from the Codified Context paper (arXiv:2602.20478):

```
Hot memory  → CLAUDE.md (always loaded, <300 lines, distilled context)
Warm layer  → Agent role definitions with entry points + tool access
Cold memory → Full KB (on-demand, agent fetches what it needs)
```

### Agent Interface Definitions

```yaml
agents:
  planner:
    purpose: "Reason about system changes, what-if scenarios, impact analysis"
    reads: [domain-concepts.yaml, test-cases/*.yaml, event-contracts.yaml, status-flows.yaml]
    writes: [vault/analysis/what-if-*.md]
    never-touches: [code repos, mockups]
    entry-point: "domain-concepts.yaml → trace relationships → check invariants"

  implementer:
    purpose: "Build features against specs"
    reads: [specs/*.md, mockups/*.html, guardrails/*.md, domain-concepts.yaml]
    writes: [code in app repo]
    validates-against: [test-cases/*.yaml, event-contracts.yaml]
    entry-point: "spec file → read mockup → implement → run tests"

  reviewer:
    purpose: "Validate implementation matches spec, flag drift"
    reads: [test-cases/*.yaml, event-contracts.yaml, app repo code]
    writes: [vault/analysis/review-*.md]
    entry-point: "test-cases/*.yaml → compare with implementation → report"

  architect:
    purpose: "Evolve specs, propose structural changes"
    reads: [everything in KB]
    writes: [specs/, design/, event-contracts.yaml]
    validates-against: [test-cases/*.yaml (invariants must still pass)]
    entry-point: "business-goals.md → domain-concepts.yaml → trace impact"
```

---

## 2. Feedback Loop — KB as Living System

### Gap

Current flow is one-directional: KB → app repos. No mechanism for implementation learnings to flow back.

### Solution: Bidirectional Spec Refinement

```
KB → implement → discover gap → update KB → re-implement
         ↑                            │
         └────── learnings ───────────┘
```

Define triggers for KB updates:
- **Implementation reveals spec gap** → implementer flags it, architect updates spec
- **Planner what-if produces insight** → captured in `vault/analysis/`, optionally promoted to spec change
- **Test failure reveals wrong assumption** → trace back to domain concept or event contract, fix at source
- **Stakeholder feedback on mockup** → update mockup + spec together
- **Post-deployment observation** → operational learnings feed back into domain KG

### KB Maturity Stages

```
GATHERING → SPECIFYING → IMPLEMENTING → OPERATING → EVOLVING
                                              │           │
                                              └─feedback──┘
```

---

## 3. Domain KG Formal Schema

### Gap

Current domain concepts are prose sketches. AI can't traverse them precisely enough to generate schemas or trace impact.

### Solution: Machine-Readable Domain Schema

```yaml
# design/data-model/domain-schema.yaml
schema_version: "1.0.0"

entity_types:
  Application:
    properties:
      id: { type: string, format: "APP-{YYYY}-{NNN}" }
      status: { type: enum, values: [draft, submitted, izin_check, dp_paid, survey_assigned, survey_complete, approved, rejected, renovation, store_open] }
      submitted_at: { type: datetime, nullable: true }
      applicant_id: { type: ref, target: CalonMitra }
      location_id: { type: ref, target: Location }
    status_machine:
      draft: [submitted]
      submitted: [izin_check, rejected]
      izin_check: [dp_paid, rejected]
      dp_paid: [survey_assigned]
      survey_assigned: [survey_complete]
      survey_complete: [approved, rejected]
      approved: [renovation]
      renovation: [store_open]
    owned_by: MitraApply

  Location:
    properties:
      id: { type: string }
      coords: { type: [lat, lng] }
      address: { type: string }
      size_m2: { type: number, min: 120, max: 220 }
      type: { type: enum, values: [ruko, standalone, mall] }
      ownership: { type: enum, values: [owned, rented] }
    shared_between: [MitraApply, MitraSurvey]

relationships:
  - from: Application
    rel: proposes
    to: Location
    cardinality: "1:1"
  - from: Application
    rel: requires
    to: Document
    cardinality: "1:N"
  - from: Survey
    rel: assesses
    to: Location
    cardinality: "N:1"
```

AI reads this and can: generate migrations, generate API endpoints, generate test assertions, trace impact of changes.

---

## 4. Spec Semantic Versioning

### Gap

When a spec changes in the KB, app repos don't know. Breaking changes to event contracts could silently break apps.

### Solution: Semantic Versioning for Specs

Apply semver (`MAJOR.MINOR.PATCH`) to specs, just like software:

- **MAJOR** — breaking change (event payload restructured, entity removed, status flow changed)
- **MINOR** — backward-compatible addition (new optional field, new event, new status added to flow)
- **PATCH** — clarification, typo fix, no behavioral change

### Applied to Event Contracts

```yaml
# design/architecture/event-contracts.yaml
contract_version: "1.2.0"

events:
  application.ready_for_survey:
    version: "1.0.0"
    published_by: MitraApply
    consumed_by: MitraSurvey
    payload:
      application_id: { type: string, required: true }
      location_id: { type: string, required: true }
      location_coords: { type: [lat, lng], required: true }
      applicant_name: { type: string, required: false }  # added in 1.1.0
    changelog:
      "1.0.0": "Initial contract"
      "1.1.0": "Added optional applicant_name"

  survey.completed:
    version: "1.0.0"
    published_by: MitraSurvey
    consumed_by: MitraApply
    payload:
      survey_id: { type: string, required: true }
      application_id: { type: string, required: true }
      result: { type: enum, values: [feasible, not_feasible, conditional], required: true }
      score: { type: number, min: 0, max: 100, required: true }
      report_url: { type: string, required: false }
```

### Applied to Domain Schema

```yaml
# design/data-model/domain-schema.yaml
schema_version: "1.0.0"
# MAJOR bump: entity removed, relationship changed, status flow restructured
# MINOR bump: new entity, new optional property, new status added
# PATCH bump: description clarified, no behavioral change
```

### Applied to Test Cases

```yaml
# specs/test-cases/mitraapply/application-submission.yaml
spec_version: "1.0.0"
compatible_with:
  domain_schema: ">=1.0.0 <2.0.0"
  event_contracts: ">=1.0.0 <2.0.0"
```

### App Repo Pinning

Each app repo's CLAUDE.md declares which spec versions it implements against:

```yaml
# In mitraapply repo CLAUDE.md
spec_dependencies:
  domain_schema: "1.0.0"
  event_contracts: "1.2.0"
  test_cases: "1.0.0"
```

Version mismatch → contract tests fail → drift caught immediately.

---

## 5. Twelve-Factor Spec Methodology

### Concept

The [Twelve-Factor App](https://12factor.net/) methodology defined principles for building modern SaaS apps. We can synthesize a similar set of principles for KB-driven multi-app development — a "Twelve-Factor Spec" methodology.

### The Twelve Factors (Draft)

#### I. Codebase — One KB, Many Apps
One KB repo tracked in version control, many app repos deployed from it. Each app repo references the same KB. The KB is the single source of truth.

#### II. Dependencies — Explicit Spec Declarations
App repos explicitly declare their spec dependencies with versions. Never assume implicit knowledge. Everything the AI agent needs to implement is in the KB.

#### III. Config — Domain Context in the KB, Tech Config in the App
Business domain knowledge (entities, relationships, flows) lives in the KB. Technology configuration (framework settings, env vars, infra) lives in the app repo. Never mix.

#### IV. Backing Services — Apps as Attached Resources
Each app (MitraApply, MitraSurvey, MitraOps) is a backing service to the others. Communicate only through event contracts defined in the KB. No direct DB access across app boundaries.

#### V. Build, Release, Run — Spec, Implement, Deploy
Strictly separate spec authoring (KB), implementation (app repo), and deployment. Never modify specs during implementation. If implementation reveals a spec gap, update the KB first, bump the version, then re-implement.

#### VI. Processes — Stateless Agents
AI agents (planner, implementer, reviewer) are stateless processes. They read from the KB, perform their task, and write output. No agent carries state between sessions. The KB is the persistent state.

#### VII. Port Binding — Event Contracts as Interfaces
Apps expose functionality via event contracts, not direct API calls. The KB defines the contract; each app binds to it. Adding a new app means defining its event subscriptions in the KB.

#### VIII. Concurrency — Scale by Agent Type
Scale AI work by spinning up more agents of different types. Multiple implementers can work on different apps simultaneously because the KB coordinates, not the agents. Planner agents work independently of implementers.

#### IX. Disposability — Fast Agent Startup
Agents start fast, read KB context quickly (hot → warm → cold), and produce output. Minimize startup time by keeping CLAUDE.md under 300 lines (hot memory) and using on-demand access for cold memory. Agents can be killed and restarted without losing system state (state lives in KB).

#### X. Dev/Prod Parity — Spec is the Contract
The spec in the KB matches what's deployed. No gap between "what we specified" and "what's running." Contract tests verify this continuously. The same test YAML that validates during development validates in production.

#### XI. Logs — Structured Decision Records
Treat architectural decisions, what-if analyses, and spec changes as event streams. ADRs, changelog entries, and version bumps are the "logs" of the system's evolution. Fully traceable via git history.

#### XII. Admin Processes — One-Off Agent Tasks
Run one-off management tasks (impact analysis, spec validation, cross-app contract check) as agents against the KB. Same agent definitions, same KB access, same tools. Don't hand-roll special scripts.

### Why Twelve-Factor Spec Works

The original twelve-factor app solved coordination problems at scale — how do you keep multiple deployments consistent, portable, and maintainable? KB-driven multi-app development has the same problems at the specification layer:

- How do you keep multiple apps consistent? → Single KB, versioned specs
- How do you scale AI work? → Stateless agents, concurrent by type
- How do you prevent drift? → Contract tests, explicit dependencies
- How do you evolve? → Semver, feedback loops, ADR logs

---

## 6. The Bootstrap Problem

### Gap

The KB guides AI to build apps. But the KB itself needs to be built — also by AI + human. No formal process for "how to evolve the KB itself."

### Solution: KB Acceptance Criteria

The KB is ready for implementation when:

```yaml
kb_readiness:
  domain_concepts:
    - "All entity types defined with properties and ownership"
    - "All relationships defined with cardinality"
    - "Status machines defined for all stateful entities"
  event_contracts:
    - "All cross-app events defined with versioned payloads"
    - "Publisher and consumer declared for each event"
  test_cases:
    - "Invariants defined for all entity types"
    - "Acceptance criteria for all status transitions"
    - "Contract tests for all cross-app events"
  mockups:
    - "Key screens for each app have HTML mockups"
    - "Mockups reviewed by stakeholder"
  graph_viz:
    - "Per-app entity graph generated"
    - "Cross-app ecosystem graph generated"
    - "Stakeholder has reviewed and approved"
```

The KB bootstrap follows its own TDD: define what "ready" looks like, then build toward it.

---

## 7. Prioritization Framework

### Gap

We have the 8→3 month goal but haven't mapped where time is lost. Can't prioritize specs without knowing bottlenecks.

### Solution: Bottleneck-First Spec Writing

Need Ivan to break down the current 8-month process:

```
Step                          | Current Duration | Bottleneck?
------------------------------|-----------------|------------
Daftar (register)             | ? weeks         | ?
Izin kepala daerah check      | ? weeks         | likely YES
Document collection           | ? weeks         | likely YES
Down payment                  | ? weeks         | ?
Survey scheduling             | ? weeks         | likely YES
Survey execution              | ? weeks         | ?
Survey report & decision      | ? weeks         | ?
Renovation                    | ? weeks         | ?
Store opening                 | ? weeks         | ?
```

Specs get written in bottleneck-priority order. If izin check takes 3 months of the 8, that's the first spec to write and the first feature to build.

---

## 8. Helper Scripts for AI Agent Traversal

### Gap

AI agents currently need to manually traverse the KB — glob for files, read YAML, parse relationships, trace impact. This is slow, error-prone, and wastes context window on file traversal instead of reasoning.

### Solution: CLI Scripts as Agent Tools

Build helper scripts that do the traversal and return structured output. AI agents call the script instead of manually reading dozens of files.

```bash
# Query domain entities
./scripts/kb query entity Application
# Returns: properties, status machine, relationships, owned_by, test count

# Trace cross-app impact of a change
./scripts/kb impact "Application.status add 'pending_review'"
# Returns: affected events, affected apps, failing tests, affected mockups

# Validate all contracts
./scripts/kb validate contracts
# Returns: pass/fail per event, version mismatches, missing consumers

# Check KB readiness
./scripts/kb readiness
# Returns: checklist with pass/fail per criterion

# List all events between two apps
./scripts/kb events MitraApply MitraSurvey
# Returns: event names, versions, payloads, direction

# Generate graph data for visualization
./scripts/kb graph mitraapply --format json
# Returns: nodes and edges for the entity graph viz

# Check spec version compatibility
./scripts/kb compat mitraapply --schema 1.0.0 --contracts 1.2.0
# Returns: compatible/incompatible, breaking changes list

# Diff two spec versions
./scripts/kb diff domain-schema 1.0.0 2.0.0
# Returns: added/removed/changed entities, relationships, properties
```

### Why Scripts, Not Just Structured Files

- **Context efficiency** — agent calls one script, gets exactly what it needs. No wasting tokens reading irrelevant files.
- **Consistency** — scripts enforce the schema. If an entity is malformed, the script catches it, not the agent.
- **Composability** — planner agent calls `kb impact`, gets structured output, reasons about it. Implementer calls `kb query entity`, gets schema, generates code.
- **Validation** — `kb validate contracts` runs as a CI check, not just when an agent happens to read the right files.
- **Same scripts for human and AI** — developer runs `kb readiness` to check progress, AI agent runs the same command.

### Implementation

Scripts read from the structured YAML files (domain-schema.yaml, event-contracts.yaml, test-cases/*.yaml) and produce structured output. Written in Python or Node — language doesn't matter since they're CLI tools.

The key: CLAUDE.md in each app repo documents these scripts as available tools. AI agents learn to call them instead of manually traversing.

```markdown
# In app repo CLAUDE.md
## KB Tools
The spec KB at ../edts-indomaret-application-kb provides helper scripts:
- `./scripts/kb query entity <name>` — get entity schema
- `./scripts/kb events <app1> <app2>` — list event contracts
- `./scripts/kb validate contracts` — check all contracts
```

---

## Summary of Improvements

| # | Gap | Solution | Priority |
|---|---|---|---|
| 1 | No agent interface | Three-layer architecture + agent role definitions | High — needed before implementation |
| 2 | No feedback loop | Bidirectional spec refinement + KB maturity stages | Medium — needed during implementation |
| 3 | Domain KG too informal | Machine-readable YAML schema with types + cardinality | High — needed before implementation |
| 4 | No spec versioning | Semantic versioning for schemas, contracts, tests | High — needed before multi-app work |
| 5 | No methodology principles | Twelve-Factor Spec methodology | Medium — formalizes the approach |
| 6 | Bootstrap problem | KB acceptance criteria | Medium — guides KB development |
| 7 | No prioritization | Bottleneck mapping with Ivan | High — needed immediately |
| 8 | AI traverses KB manually | Helper scripts as agent tools (`kb query`, `kb impact`, `kb validate`) | High — builds with the KB |
