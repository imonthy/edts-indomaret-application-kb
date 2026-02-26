# EDTS Indomaret Franchise Application — Knowledge Base

> Status: GATHERING — collecting requirements and context from stakeholders

## What This Is

KB for planning and designing an Indomaret franchise application system for EDTS. This vault collects research, requirements, and design decisions before development begins.

## Pipeline

```
source/ → research/ → requirements/ → design/ (blueprints)
                 ↑
             vault/ (notes, decisions, analysis)
```

- `source/` — original documents, screenshots, references (immutable, never edited)
- `research/` — structured findings from market, competitor, regulatory, and technical research
  - `market/` — Indomaret franchise market data, pricing, processes
  - `competitor/` — existing franchise management systems
  - `regulatory/` — legal requirements, permits, compliance
  - `technical/` — tech stack research, integrations, infrastructure
- `requirements/` — structured requirements derived from research + stakeholder input
  - `functional/` — feature specs, user stories, acceptance criteria
  - `non-functional/` — performance, security, scalability, compliance
- `design/` — architecture and design artifacts
  - `architecture/` — system design, API design, tech stack decisions
  - `wireframes/` — UI/UX mockups and flows
  - `data-model/` — ERD, schema design
- `vault/` — private working notes, not for external consumption
  - `notes/` — meeting notes, WhatsApp context, conversations
  - `decisions/` — ADRs (Architecture Decision Records)
  - `analysis/` — comparative analysis, feasibility studies

## Link Direction

```
vault/ → design/ → requirements/ → research/ → source/
  (working)  (blueprints)  (specs)      (findings)   (immutable)
```

Links flow outward only. vault/ may reference anything. design/ references requirements/ and research/. requirements/ references research/ and source/. Never reverse.

## Project Context

- **Client**: EDTS (PT Elang Digital Teknologi Sejahtera)
- **Application**: Indomaret franchise submission/management system
- **Stakeholder**: Ivan Jonathan (EDTS)
- **Platform**: TBD — awaiting stakeholder confirmation
- **Scope**: TBD — likely includes franchise application form, document upload, status tracking, admin dashboard

## What We Know So Far

From initial WhatsApp conversation (2026-02-26):
- Ivan needs a "pengajuan franchise" (franchise application) system
- Franchise brand: Indomaret
- Platform not yet confirmed (web? mobile? both?)
- No detailed requirements yet — need to gather from Ivan

## Open Questions

1. **Platform**: Web only? Mobile (Android/iOS)? Both?
2. **Users**: Who uses the system? (Applicants, admin reviewers, management, Indomaret HQ?)
3. **Scope**: What's the full feature list? Possible features:
   - Franchise application form (data diri, lokasi, modal)
   - Document upload (KTP, NPWP, SIUP, etc.)
   - Application status tracking
   - Admin review dashboard
   - Notifications (email, push, WhatsApp?)
   - Reporting/analytics
   - Payment tracking (franchise fee)
4. **Integration**: Any existing EDTS systems to integrate with?
5. **Timeline**: When does this need to be ready?
6. **Tech stack**: Any EDTS preferences? (React, Flutter, etc.)

## Conventions

- Frontmatter required: `title`, `status`, `date`, `tags`
- Status values: `draft` → `reviewed` → `final`
- Tags use kebab-case
- Indonesian language for business domain terms, English for technical terms
