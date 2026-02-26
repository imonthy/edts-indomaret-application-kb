---
title: Build Approach — Lean TDD with AI-Assisted Development
date: 2026-02-26
status: draft
tags:
  - approach
  - tdd
  - planning
---

# Build Approach — Lean TDD with AI-Assisted Development

## Philosophy

- Build as lean as possible
- Information management system, not complex business logic
- Core value: tracking, visibility, reducing handoff delays (8mo → 3mo)
- No Figma — KB is the single source of truth for everything
- AI-assisted build, EDTS handles maintenance

## KB as the Design System

- **Design docs** live in the KB
- **HTML mockups** for the management system UIs — living specs that AI references directly when implementing
- **Guardrails** (CLAUDE.md-style files) to guide AI during implementation
- **Best practice grounding** — data structure, tech stack, patterns decided upfront in KB

## TDD Cycle

1. Define business goal in KB
2. Write acceptance test / test case in KB (what "done" looks like)
3. Generate HTML mockup that satisfies the test
4. Implement with guardrails guiding AI
5. Run tests, iterate

## KB Structure Plan (when ready to execute)

```
design/
├── architecture/       ← tech stack, system design, pub/sub event contracts
├── data-model/         ← schema definitions per app, shared event schema
├── mockups/            ← HTML prototypes (living specs)
│   ├── mitraapply/
│   ├── mitrasurvey/
│   └── mitraops/
└── guardrails/         ← CLAUDE.md templates for each app repo
                          (coding conventions, test patterns, boundaries)

specs/
├── business-goals.md   ← measurable goals (8mo→3mo, etc.)
├── test-cases/         ← acceptance criteria as test definitions BEFORE code
│   ├── mitraapply/
│   ├── mitrasurvey/
│   └── mitraops/
└── user-flows.md       ← step-by-step flows with expected behavior
```

## Grounding Work (before implementation)

- **Data model** — entities per app + shared event schema for pub/sub
- **Tech stack decision** — ADR comparing options (EDTS maintains, so their input matters)
- **API contracts** — inter-app events defined before implementation
- **Auth model** — per app, keep simple
- **Shared event schema** — first guardrail to write, prevents drift between 3 apps

## Key Insight

The 8-month delay is likely from manual document chasing, untracked izin checks, survey scheduling sitting in inboxes, and no visibility into where applications are stuck. The system solves this by making every handoff trackable and every status visible.

## Next Steps (when continuing)

1. Clarify open questions with Ivan (see CLAUDE.md)
2. Define business goals with measurable criteria
3. Plan data structures and tech stack
4. Write test cases / acceptance criteria
5. Build HTML mockups
6. Set up guardrails for each app repo
7. Start TDD implementation (MitraApply first)
