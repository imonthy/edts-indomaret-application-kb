---
title: "ADR-002: Add MitraSurvey as Third Separate Application"
date: 2026-02-26
status: accepted
tags:
  - architecture
  - scope
  - phases
  - survey
amends: adr-001-two-phase-separation
---

# ADR-002: Add MitraSurvey as Third Separate Application

## Context

ADR-001 established a two-app architecture: **MitraApply** (application phase) and **MitraOps** (post-opening phase). In that design, location feasibility survey was bundled into MitraApply — either as a surveyor role within the same app, or as a module within the application workflow.

On further analysis, the survey function has:
- **Different users** — Indomaret's internal surveyor team, not applicants or admin reviewers
- **Different usage context** — field work (on-site, mobile-first), not desk-based admin or applicant self-service
- **Different data model** — geospatial data, checklists, scoring matrices, geotagged photos
- **Different lifecycle** — a survey can be triggered, rescheduled, or repeated independently of the application status

Bundling survey into MitraApply would force one app to serve three very different user groups (applicants, admin reviewers, and field surveyors) with very different UX needs.

## Decision

**Three separate applications, each with separate databases and separate authentication.**

| App | Users | Purpose |
|---|---|---|
| **MitraApply** | Calon mitra (applicants) + Indomaret admin | Franchise application submission & tracking |
| **MitraSurvey** (new) | Indomaret surveyor team | On-site location feasibility assessment |
| **MitraOps** | Mitra aktif + operational staff | Post-opening store management |

## MitraSurvey — Survei Lokasi (Location Survey)

On-site feasibility assessment of proposed franchise locations:

- **Survey assignment & scheduling** — receive survey tasks from MitraApply pipeline, schedule site visits
- **Mobile checklist assessment** — structured evaluation form covering:
  - Accessibility & visibility
  - Parking availability
  - Foot traffic count
  - Competitor proximity (existing Indomaret/Alfamart stores nearby)
  - Building condition & suitability
  - Lease terms verification
  - Regulatory compliance (zoning)
  - Utility infrastructure
- **Geotagged photo capture** — on-site photos with GPS metadata (storefront, interior, surroundings)
- **Feasibility scoring** — weighted scoring based on survey criteria, auto-calculated
- **Survey report generation** — auto-generated report from survey data for MitraApply review workflow
- **GIS integration** — proximity analysis to existing stores, demographic overlay, competitor mapping

**Users**: Indomaret surveyor team (field staff assigned to assess proposed locations)

## Updated MitraApply Scope

With survey extracted to MitraSurvey, MitraApply focuses on:
- Franchise application form (data diri, lokasi, modal)
- Document collection & verification (KTP, NPWP, SIUP, IMB, NIB, etc.)
- **Location proposal submission** — applicant drops pin on map, uploads photos, provides location details (size, ownership, lease terms)
- Application status tracking (including survey status from MitraSurvey)
- Admin review & approval workflow
- Payment tracking (franchise fee ~Rp494M)
- Communication between applicant & Indomaret

MitraApply triggers a survey request to MitraSurvey and receives the survey result/score back.

## MitraOps Scope (Unchanged)

No change from ADR-001.

## Integration Between Apps

```
MitraApply ──(survey request)──→ MitraSurvey
MitraSurvey ──(survey result/score)──→ MitraApply
```

- MitraApply sends a survey request when an application reaches the location survey stage
- MitraSurvey returns the feasibility result (score + report) to MitraApply
- Integration mechanism TBD (API, event bus, shared queue)
- MitraApply and MitraSurvey remain separate databases and separate auth

## Architecture

| Aspect | MitraApply | MitraSurvey | MitraOps |
|---|---|---|---|
| Database | Separate | Separate | Separate |
| Auth | Separate | Separate | Separate |
| Users | Applicants + admin | Surveyor team | Franchise owners + ops staff |
| Primary UX | Web (applicant portal + admin dashboard) | Mobile-first (field work) | TBD |
| Data lifecycle | Short (application process) | Short (per-survey) | Long (ongoing operations) |
| Traffic pattern | Burst (promo periods) | Steady-low (scheduled surveys) | Steady (daily operations) |

## Rationale

- **Single responsibility** — each app serves one user group with one primary workflow
- **Mobile-first surveyor UX** — field surveyors need a dedicated mobile experience (GPS, camera, offline capability), which is better served by a purpose-built app than a role within MitraApply
- **Independent delivery** — MitraSurvey can be developed and shipped independently
- **Offline capability** — surveyors may be in areas with poor connectivity; a dedicated app can be optimized for offline-first with sync
- **Clean integration boundary** — MitraApply and MitraSurvey communicate through a well-defined interface (survey request → survey result), not shared database tables
- **Follows ADR-001 principles** — same reasoning (different users, different data, security isolation) that justified separating MitraApply from MitraOps applies equally to separating MitraSurvey

## Status

Accepted — amends ADR-001 (expands from 2 apps to 3 apps).

## Open Questions

- Integration mechanism between MitraApply and MitraSurvey (REST API, webhook, event-driven?)
- Does MitraSurvey need offline-first capability? (likely yes for field work)
- Platform for MitraSurvey — native mobile (Android/iOS), PWA, or cross-platform?
- Who assigns surveys — MitraApply admin triggers it, or MitraSurvey has its own scheduling?
- Does EDTS scope cover all 3 apps?
