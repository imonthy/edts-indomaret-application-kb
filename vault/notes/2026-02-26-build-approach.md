---
title: Build Approach — Lean TDD with AI-Assisted Development
date: 2026-02-26
status: draft
tags:
  - approach
  - tdd
  - planning
  - sdd
  - knowledge-graph
---

# Build Approach — Lean TDD with AI-Assisted Development

## Philosophy

- Build as lean as possible
- Information management system, not complex business logic
- Core value: tracking, visibility, reducing handoff delays (8mo → 3mo)
- No Figma — KB is the single source of truth for everything
- AI-assisted build, EDTS handles maintenance
- Spec-Driven Development (SDD) — specs are AI-optimized, not just human-readable

## Spec-Driven Development (SDD)

This project follows the SDD methodology (see `research/spec-driven-ai-development-report.md`). Key principle: **specs are optimized for AI consumption**, not just human documentation.

### Why AI-Optimized Specs

Traditional specs are prose for humans. Our specs are structured data that AI reads and implements against directly:

- **Entities as typed IDs** — `application/APP-001`, not "the application". No ambiguity.
- **Relationships as triples** — `person/applicant → submits → application/APP-001` translates directly to schema, API, and tests.
- **Status flows as state machines** — `draft → submitted → izin_check → dp_paid → ...` becomes the exact enum, validation logic, and test cases.
- **Event contracts as structured YAML** — `application.ready_for_survey` with defined payload = AI generates both publisher and subscriber.
- **HTML mockups as parseable UI spec** — AI reads HTML, understands layout/fields/validation, implements in target framework.
- **Test cases defined before code** — AI reads acceptance criteria, writes test, then writes implementation.

### The Chain

```
Structured spec (YAML/MD in KB)
  → AI reads entity + relationship + status flow + mockup
    → AI generates: schema, model, API, test, UI component
      → Tests validate against the same spec
```

### What We Borrow from SDD

- `constitution.md` / guardrails — team-wide AI coding rules
- Structured spec format: requirements → design → tasks
- SKILL.md files — on-demand agent instructions (e.g., "implement-from-mockup")
- TDD as the core cycle (Tweag's Agentic Coding Handbook)

### What We Add Beyond Standard SDD

- Obsidian vault as authoring layer
- VitePress + Workers as browsable wiki for stakeholders
- HTML mockups deployed alongside specs at a URL
- Domain knowledge graph for AI grounding
- Cross-app event contracts (pub/sub between 3 apps)
- Research layer (competitive analysis → informed specs)

## Domain Knowledge Graph

### Purpose

The knowledge graph is NOT for implementation — it's for **grounding AI's understanding of the franchise domain** so it makes better decisions when implementing.

When AI reads "izin kepala daerah", it needs to understand that's a regional government approval that can block the entire pipeline for weeks. When it sees "survey assessment", it needs to know there's a physical person going to a location with a checklist. The knowledge graph provides this context without re-explaining every session.

### Lean Domain Concepts (not a full KG pipeline)

```yaml
concepts:
  franchise-application:
    what: "A formal request to open an Indomaret store"
    lifecycle: "8 months currently, target 3 months"
    bottlenecks: [izin-kepala-daerah, survey-scheduling, document-chasing]

  izin-kepala-daerah:
    what: "Regional government head must approve new minimarket in their area"
    why-slow: "Bureaucratic process, no tracking, paper-based"
    blocker: true

  survey-assessment:
    what: "Physical site visit by Indomaret surveyor"
    checks: [location-feasibility, traffic, demographics, competition-proximity]
    when: [during-application, post-opening-periodic]

  down-payment:
    what: "Rp4 juta initial commitment payment"
    when: "After izin check passes, before deep survey"

  mitra:
    what: "Franchise partner/owner"
    types: [calon-mitra (applicant), mitra-aktif (active owner)]
```

Just domain concepts with context that AI wouldn't know from code alone. Not 1,781 entities like PSMTI — just enough to prevent AI from building something structurally correct but domain-wrong.

### Graph Visualization

The domain concepts generate **interactive graph viz as HTML pages** in VitePress. Serves two purposes:

1. **Stakeholder communication** — Ivan sees a visual map of the system, not a wall of text. Reviews by saying "yes this is right" / "no, missing X".
2. **AI spec** — the same structured data that powers the viz feeds implementation.

#### Per-App Entity Graph

Each app gets a graph showing its owned entities and relationships:

```
MitraApply:
  [Calon Mitra] ──submits──→ [Application]
                                ├──proposes──→ [Location]
                                ├──requires──→ [Document] ×N
                                ├──triggers──→ [Survey]
                                ├──needs──→ [Izin Kepala Daerah]
                                ├──pays──→ [Down Payment]
                                └──status: draft→submitted→izin→dp→survey→approved→reno→open
```

#### Cross-App Relationship Graph

The full ecosystem view shows all 3 apps, their boundaries, shared concepts, and pub/sub connections:

```
┌─ MitraApply ─────────────────────────────────────┐
│                                                    │
│  [Calon Mitra] ──submits──→ [Application]          │
│                                ├──→ [Document] ×N  │
│                                ├──→ [Location]     │
│                                ├──→ [Izin]         │
│                                ├──→ [Down Payment] │
│                                │                   │
└────────────────────────────────┼───────────────────┘
                                 │
                    pub/sub: application.ready_for_survey
                                 │
┌─ MitraSurvey ─────────────────┼───────────────────┐
│                                ▼                   │
│  [Surveyor] ──conducts──→ [Survey]                 │
│                              ├──→ [Location] (ref) │
│                              ├──→ [Photos]         │
│                              └──→ [Score/Report]   │
│                                │                   │
└────────────────────────────────┼───────────────────┘
                                 │
                    pub/sub: survey.completed
                                 │
┌─ MitraApply ──────────────────┼───────────────────┐
│                                ▼                   │
│  [Application] status → approved                   │
│                                │                   │
└────────────────────────────────┼───────────────────┘
                                 │
                    pub/sub: application.approved (handoff)
                                 │
┌─ MitraOps ────────────────────┼───────────────────┐
│                                ▼                   │
│  [Store] ←──becomes── [Application]                │
│    ├──→ [Mitra Aktif] (was Calon Mitra)            │
│    ├──→ [Omzet/Royalty]                            │
│    ├──→ [Contract]                                 │
│    └──triggers──→ [Survey] (periodic, via pub/sub) │
│                                                    │
└────────────────────────────────────────────────────┘
```

Key insight from the cross-app graph: **Location** and **Survey** are shared concepts that cross app boundaries. The graph viz forces early definition of event contracts — what does MitraApply send about a Location that MitraSurvey needs?

#### Interactive Features (HTML)

- Click an app boundary → zoom into that app's entities
- Click a pub/sub line → see the event payload schema
- Click an entity → see its properties and relationships
- Color-coded by app ownership

## KB as the Design System

- **Design docs** live in the KB (markdown, AI-readable)
- **HTML mockups** for UIs — living specs that AI references directly when implementing
- **Graph visualizations** — interactive entity/relationship maps for stakeholder review + AI grounding
- **Guardrails** (CLAUDE.md-style files) to guide AI during implementation
- **Domain knowledge graph** — structured concepts for AI grounding

## VitePress + Cloudflare Workers

### Architecture

Hybrid of GCP KB (VitePress wiki) + Tax KB (Cloudflare Workers):

- **VitePress** for wiki-like spec pages (search, sidebar, navigation)
- **Raw HTML pages** for mockups, visualizations, graph viz
- **Cloudflare Workers** for deployment (not Pages)
- **Vault sync script** transforms Obsidian markdown → VitePress docs

```
vault/                          ← Obsidian source of truth
site/
├── .vitepress/
│   ├── config.mts              ← VitePress config
│   └── theme/                  ← custom components
├── scripts/
│   └── sync-vault-to-site.mjs  ← vault → docs
├── public/
│   ├── mockups/                ← raw HTML mockups (served as-is)
│   │   ├── mitraapply.html
│   │   ├── mitrasurvey.html
│   │   └── mitraops.html
│   └── viz/                    ← graph visualizations
│       ├── mitraapply-entities.html
│       ├── mitrasurvey-entities.html
│       ├── mitraops-entities.html
│       └── ecosystem.html      ← cross-app relationship graph
├── package.json
└── wrangler.toml               ← Cloudflare Workers
```

### Flow

```
Edit in Obsidian/Claude Code (vault/)
  → sync script → VitePress docs
  → wrangler deploy → Cloudflare Workers
  → Ivan browses specs at URL, reviews mockups + graph viz
  → Feedback → update KB → iterate
```

## Spec as Contract — Enabling AI Agents

### Contract-Based Development

When the SDD spec is applied to an app repo, it functions as a **contract** — a formal agreement of what the system should do, structured enough for AI to verify against.

This enables:

1. **Implementation agents** — read the spec, generate code that fulfills the contract, run tests to verify compliance
2. **Planner agents** — spin off to explore "what if" scenarios against the contract without touching code
3. **Review agents** — validate that implementation matches the spec, flag drift
4. **Test agents** — generate test cases from the contract, ensure coverage

### What-If Scenarios with Planner Agents

Because the spec is structured (entities, relationships, status flows, event contracts), AI can reason about the system without implementing:

- "What if we add a pre-survey step before izin check?" → planner agent reads the status flow, traces the impact on event contracts, estimates which apps are affected
- "What if MitraSurvey needs to access applicant documents?" → planner checks entity ownership boundaries, proposes event payload changes
- "What if we merge MitraApply and MitraSurvey?" → planner evaluates entity overlap, user overlap, data model impact
- "What if izin check is parallelized with document collection?" → planner simulates the new flow against the 8→3 month goal

This works because the spec is:
- **Structured** — not prose, so AI can traverse relationships
- **Complete** — entities, flows, events, boundaries are all defined
- **Bounded** — app boundaries are explicit, so impact analysis is scoped
- **Testable** — acceptance criteria exist, so "what if" can be validated

### Agent Workflow

```
Human: "What if we add a pre-approval step?"
  → Planner agent reads spec (entities, flows, events)
    → Traces impact across 3 apps
      → Proposes spec changes (new status, new event, affected tests)
        → Human reviews: "yes" / "no" / "modify"
          → Spec updated → implementation agent builds it
```

The KB isn't just documentation — it's the **API for AI agents to reason about the system**.

## KB ↔ App Repo Relationship

This KB is the **design authority**. App repos consume it.

```
edts-indomaret-application-kb (this repo)
  ├── CLAUDE.md — goals, architecture, event contracts
  ├── specs + test cases per app
  ├── HTML mockups (deployed for Ivan to review)
  ├── graph viz (entity maps for review + AI grounding)
  └── guardrails (CLAUDE.md templates for each app repo)
        │
        ▼ referenced by
  ┌─────────────────────────┐
  │ mitraapply repo         │ ← CLAUDE.md says "see KB for specs"
  │ mitrasurvey repo        │ ← pulls goals, test cases, event schema from KB
  │ mitraops repo           │ ← mockups + graph viz are the visual spec
  └─────────────────────────┘
```

- **KB owns "what to build"** — goals, specs, mockups, domain knowledge, event contracts
- **App repo owns "how to build"** — tech stack, coding conventions, test commands, infrastructure
- **Reference by URL** — app repo CLAUDE.md points to KB repo + deployed Workers site. Keep it simple, no submodules.

## Tests as the Universal Contract Validator

Tests in the KB serve multiple roles beyond traditional TDD:

### For Implementation Agents
- Read spec → write code → run tests → verify compliance
- Standard TDD: red → green → refactor

### For Planner / What-If Agents
- Propose a spec change → run tests against the proposed change → see what breaks
- "What if we remove the izin check step?" → planner sees 4 tests fail → knows the impact before any code changes
- Tests define the **invariants** of the system — what must always be true regardless of changes

### For Review Agents
- Compare implementation against tests → flag drift from spec
- Tests are the source of truth, not the code

### For Cross-App Contract Validation
- Event contract tests verify: "if MitraApply publishes `application.ready_for_survey`, MitraSurvey can process it"
- Changing an event payload in one app → contract tests fail in the other → drift caught immediately

### Test Structure in KB

```yaml
# specs/test-cases/mitraapply/application-submission.yaml
test: application-submission
invariants:
  - "Application must have at least 1 proposed location"
  - "All required documents must be listed (not necessarily uploaded)"
  - "Status transitions: draft → submitted (only when form complete)"

acceptance:
  - given: "Calon mitra fills all required fields"
    when: "Submits application"
    then: "Status becomes 'submitted', admin gets notified"

  - given: "Application is submitted"
    when: "Admin triggers izin check"
    then: "Status becomes 'izin_check', event published to track"

contract:
  - event: "application.ready_for_survey"
    payload_must_include: [application_id, location_id, location_coords]
    consumer: "MitraSurvey"
```

The same test file is used by:
- **Implementation agent** → generates unit tests from `acceptance`
- **Planner agent** → checks `invariants` before proposing changes
- **Contract agent** → validates `contract` across app boundaries

Tests aren't just for code — they're the **guardrails for all AI reasoning about the system**.

## TDD Cycle

1. Define business goal in KB
2. Define domain concepts (knowledge graph) for AI grounding
3. Write tests in KB — acceptance criteria, invariants, cross-app contracts
4. Generate HTML mockup + graph viz for stakeholder review
5. Implement with guardrails guiding AI (in app repo)
6. Run tests — verify implementation, catch drift, validate contracts
7. Iterate

## KB Structure Plan (when ready to execute)

```
design/
├── architecture/       ← tech stack, system design, pub/sub event contracts
├── data-model/         ← domain concepts (YAML), entity definitions, shared event schema
├── mockups/            ← HTML prototypes (living specs)
│   ├── mitraapply/
│   ├── mitrasurvey/
│   └── mitraops/
├── viz/                ← graph visualizations (HTML)
│   ├── per-app entity graphs
│   └── ecosystem cross-app graph
└── guardrails/         ← CLAUDE.md templates for each app repo

specs/
├── business-goals.md   ← measurable goals (8mo→3mo, etc.)
├── test-cases/         ← acceptance criteria as test definitions BEFORE code
│   ├── mitraapply/
│   ├── mitrasurvey/
│   └── mitraops/
└── user-flows.md       ← step-by-step flows with expected behavior
```

## Grounding Work (before implementation)

- **Domain knowledge graph** — concepts, relationships, context for AI grounding
- **Data model** — entities per app + shared event schema for pub/sub
- **Tech stack decision** — ADR comparing options (EDTS maintains, so their input matters)
- **API contracts** — inter-app events defined before implementation
- **Auth model** — per app, keep simple
- **Shared event schema** — first guardrail to write, prevents drift between 3 apps

## Key Insight

The 8-month delay is likely from manual document chasing, untracked izin checks, survey scheduling sitting in inboxes, and no visibility into where applications are stuck. The system solves this by making every handoff trackable and every status visible.

## Next Steps (when continuing)

1. Clarify open questions with Ivan (see CLAUDE.md)
2. Set up VitePress + Workers scaffold
3. Define domain knowledge graph (concepts YAML)
4. Define business goals with measurable criteria
5. Sketch first graph viz (MitraApply entities + cross-app ecosystem)
6. Plan data structures and tech stack
7. Write test cases / acceptance criteria
8. Build HTML mockups
9. Set up guardrails for each app repo
10. Start TDD implementation (MitraApply first)
