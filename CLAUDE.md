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
- **Scope**: TBD — see Architectural Direction below

## Architectural Direction

### Two Separate Apps (ADR-001 — Accepted)

**Confirmed by Ivan (2026-02-26):** Two separate applications, separate databases, separate auth. Users are completely different between phases — no shared authentication needed.

**App 1 — Pengajuan (Application):** Initial interest → MoU signing / store opening
- Franchise application form (data diri, lokasi, modal)
- Document collection & verification (KTP, NPWP, SIUP, IMB, NIB, etc.)
- Location proposal & feasibility survey
- Application status tracking
- Admin review & approval workflow
- Payment tracking (franchise fee ~Rp494M)
- Communication between applicant & Indomaret
- **Users**: Calon mitra (applicants), Indomaret admin reviewers

**App 2 — Pasca Buka Toko (Post-Opening):** After store is operational
- Store performance dashboard
- Financial reporting (omzet, royalty with progressive 0-4% structure)
- Operational management
- Inventory / supply chain visibility
- Compliance & audit
- Contract renewal management (5-year terms)
- **Users**: Mitra aktif (franchise owners), operational staff

> See `vault/decisions/adr-001-two-phase-separation.md` for full rationale.

## Competitive Landscape (Distilled)

### Indomaret Today
- Basic web form at `indomaret.co.id/home/index/pendaftaran-waralaba`
- 4-step process: Online form → Presentation 1 → Presentation 2 → MoU
- No dedicated franchise app, no status tracking, no document portal
- Operations are "autopilot" via centralized internal systems
- **No Mitra (partner) app** — management through Indomaret's internal tools only

### Alfamart (Direct Competitor)
- 3 fragmented platforms: waralaba.alfamart.co.id, alfamartku.com, ALFA Franchise app
- Portal partially broken (registration page in maintenance)
- ALFA app: low adoption (10K downloads for 3,600+ partners, 3.5/5 rating)
- No status tracking, no document management, no payment integration
- **Maturity: Medium-Low** — digitized basics but poor execution

### International Best Practices
- **7-Eleven (USA)**: Gold standard — Salesforce CRM, automated credit checks, 7University training app (Schoox LMS), separate portals for recruitment vs operations
- **Circle K**: 2025 NACS Award — AI recruitment platform, gamified onboarding, reduced turnover 13% across 5,200+ stores
- **GS25 (Korea)**: Electronic contracts, "Bossmon" franchisee app, Scenera AIoT across 10K+ stores
- **FamilyMart**: Strong on store ops (Google Cloud, Vertex AI) but franchise recruitment still semi-manual — same gap as Indomaret
- **FranConnect**: Leading franchise SaaS (1,500+ brands) — full lifecycle from lead management to royalty automation

### Key Opportunity
Both Indomaret and Alfamart have weak digital franchise systems. A modern unified platform would leapfrog both. The bar to beat is low domestically; international examples (7-Eleven, Circle K) show what "great" looks like.

## Indomaret Franchise Quick Facts

| Item | Detail |
|---|---|
| Investment (new store) | Rp394M–500M (excl. rental) |
| Investment (take over) | From Rp700M (incl. 5yr rental) |
| Franchise fee | Rp36M / 5 years |
| Royalty | Progressive: 0% (<175M) → 2% → 3% → 4% (>225M) |
| Store size | 120–220 m² |
| Contract term | 5 years |
| Total stores | 23,100+ (~50% franchise) |
| BEP estimate | 3–4 years |

## Required Documents (Franchise Application)

**Business permits**: IMB/PBG, NPWP, PKP, NIB (via OSS), STPW, Ijin Lingkungan, Domisili

**Supporting docs**: KTP, KK, Sertifikat Bangunan, SIUP, TDP, IUTM/IUTS, Denah Lokasi, UUG

## Research Files

| File | Content |
|---|---|
| `research/market/indomaret-franchise-program.md` | Indomaret costs, process, documents, digital gap |
| `research/competitor/convenience-store-franchise-application-systems.md` | 7 brands compared + 7-Eleven deep dive |
| `research/competitor/alfamart-franchise-portal-deep-dive.md` | Alfamart portal, app, and UX analysis |
| `research/competitor/familymart-franchise-digital-systems-deep-dive.md` | FamilyMart across JP/TW/TH |
| `research/competitor/international-franchise-systems-best-practices.md` | Lawson, Circle K, GS25, CU, OXXO, SaaS platforms |

## Open Questions

1. **Platform**: Web only? Mobile (Android/iOS)? Both?
2. **Users**: Who uses the system? (Applicants, admin reviewers, management, Indomaret HQ?)
3. **Scope**: Does EDTS cover both apps or App 1 (Pengajuan) only?
4. ~~**Architecture**: Two separate apps or two modules in one platform?~~ → **Resolved: 2 separate apps, separate DB, separate auth** (ADR-001)
5. **Integration**: Existing EDTS/Indomaret systems to integrate with?
6. **Timeline**: When does this need to be ready?
7. **Tech stack**: Any EDTS preferences?

## Conventions

- Frontmatter required: `title`, `status`, `date`, `tags`
- Status values: `draft` → `reviewed` → `final`
- Tags use kebab-case
- Indonesian language for business domain terms, English for technical terms
