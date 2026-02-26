# EDTS Indomaret Franchise Application — Knowledge Base

> Status: GATHERING — collecting requirements and context from stakeholders

## What This Is

KB for planning and designing an Indomaret franchise application system for EDTS. This vault collects research, requirements, and design decisions before development begins.

## Primary Goal

**Reduce the franchise registration process from 8 months to 3 months** through digitization and streamlined workflows.

## Pipeline

```
source/ → research/ → requirements/ → design/ (blueprints)
                 ↑
             vault/ (notes, decisions, analysis)
```

- `source/` — original documents, screenshots, references (immutable, never edited)
- `research/` — structured findings from market, competitor, regulatory, and technical research
- `requirements/` — structured requirements derived from research + stakeholder input
- `design/` — architecture and design artifacts
- `vault/` — private working notes, not for external consumption (notes, ADRs, analysis)

## Project Context

- **Client**: EDTS (PT Elang Digital Teknologi Sejahtera)
- **Stakeholder**: Ivan Jonathan (EDTS)
- **Platform**: Web + Android + iOS (design all three, execute web first)
- **Maintenance**: EDTS handles ongoing maintenance
- **Build approach**: AI-assisted development

## Architectural Direction

### Three Separate Apps (ADR-001 — Accepted)

**3 apps, 3 databases, 3 separate auth systems, communicating via pub/sub.**

Ivan's reasoning: "lebih aman" (security through isolation) + "usernya beda bgt" (completely different users).

#### MitraApply — Pengajuan (Application)
Franchise application from registration to store opening:
- Daftar (register)
- Cek izin kepala daerah (regional head permit check)
- Down payment Rp4 juta
- Document collection & verification (KTP, NPWP, SIUP, IMB, NIB, etc.)
- Application status tracking
- Admin review & approval workflow
- **Users**: Calon mitra (applicants), Indomaret admin reviewers

#### MitraSurvey — Survey & Assessment
Location feasibility surveys during application AND ongoing surveys for active stores:
- Survey assignment & scheduling
- Field data collection (photos, measurements, area assessment)
- Feasibility scoring & reporting
- Spans both phases (application + post-opening)
- **Users**: Indomaret surveyors/field staff, registered mitra

#### MitraOps — Pasca Buka Toko (Post-Opening)
After store is operational:
- Store performance dashboard
- Financial reporting (omzet, royalty with progressive 0-4% structure)
- Operational management, compliance, audit
- Contract renewal management (5-year terms)
- **Users**: Mitra aktif (franchise owners), operational staff

### High-Level Registration Flow

**Target: 8 months → 3 months**

```
Daftar (MitraApply)
  → Cek izin kepala daerah (MitraApply)
    → Down payment Rp4 juta (MitraApply)
      → Survey assessment mendalam (MitraSurvey)
        → All clear? → Renovasi & buka gerai
          → Store live → MitraOps
```

### Inter-App Communication: Pub/Sub

Apps are separate but status/messaging flows smoothly via event bus:
- `application.ready_for_survey` — MitraApply → MitraSurvey
- `survey.completed` — MitraSurvey → MitraApply
- `application.approved` — MitraApply → MitraOps (handoff)
- `store.survey_requested` — MitraOps → MitraSurvey

Technology options: Google Cloud Pub/Sub (managed) or Redis Streams (simpler). TBD.

> See `vault/decisions/adr-001-two-phase-separation.md` for full rationale.

## Competitive Landscape (Distilled)

### Indomaret Today
- Basic web form, 4-step process (form → 2x presentation → MoU)
- No dedicated franchise app, no status tracking, no document portal
- **Current process takes ~8 months**

### Alfamart (Direct Competitor)
- 3 fragmented platforms (waralaba.alfamart.co.id, alfamartku.com, ALFA app)
- Partially broken, low adoption (10K downloads for 3,600+ partners)
- **Maturity: Medium-Low**

### International Best Practices
- **7-Eleven (USA)**: Salesforce CRM, automated credit checks, 7University (Schoox LMS), separate portals for recruitment vs operations
- **Circle K**: AI recruitment platform, gamified onboarding, reduced turnover 13%
- **GS25 (Korea)**: Electronic contracts, "Bossmon" franchisee app
- **FranConnect**: Leading franchise SaaS (1,500+ brands, full lifecycle)

### Key Opportunity
Both Indomaret and Alfamart have weak digital franchise systems. Bar to beat is low domestically.

## Indomaret Franchise Quick Facts

| Item | Detail |
|---|---|
| Investment (new store) | Rp394M–500M (excl. rental) |
| Investment (take over) | From Rp700M (incl. 5yr rental) |
| Franchise fee | Rp36M / 5 years |
| Down payment | Rp4 juta |
| Royalty | Progressive: 0% (<175M) → 2% → 3% → 4% (>225M) |
| Store size | 120–220 m² |
| Contract term | 5 years |
| Total stores | 23,100+ (~50% franchise) |
| Current registration time | ~8 months |
| Target registration time | 3 months |

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
| `research/technical/mitraapply-best-practices.md` | Technical best practices for MitraApply |

## Open Questions (Need Clarification from Ivan)

1. ~~**Platform**~~ → **Resolved: Web + Android + iOS (web first)**
2. ~~**Architecture**~~ → **Resolved: 3 apps, 3 DBs, pub/sub**
3. **Scope priority**: All 3 apps simultaneously or MitraApply first?
4. **Tech stack**: EDTS preferences per app?
5. **Pub/sub tech**: Google Cloud Pub/Sub vs Redis Streams?
6. **Integration**: Existing Indomaret systems?
7. **Timeline**: When does each app need to be ready?
8. **MitraSurvey detail**: What specific surveys do registered mitra perform?
9. **Where in the 8-month process are the bottlenecks?** (to target the 3-month goal)

## Conventions

- Frontmatter required: `title`, `status`, `date`, `tags`
- Status values: `draft` → `reviewed` → `final`
- Tags use kebab-case
- Indonesian language for business domain terms, English for technical terms
