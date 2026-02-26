---
title: "ADR-001: Three Separate Apps with Pub/Sub Communication"
date: 2026-02-26
status: accepted
tags:
  - architecture
  - scope
  - phases
  - pub-sub
---

# ADR-001: Three Separate Apps with Pub/Sub Communication

## Context

From initial discussion (Monthy, 2026-02-26):

> "kepikir mau pisahin fasa pengajuan dgn fasa setelah toko buka"

From follow-up call: Ivan also needs a surveyor app for Indomaret staff to survey proposed locations. Survey also runs post-opening for active stores.

## Decision

**Three separate applications, separate databases, separate auth, communicating via pub/sub.**

### App 1: MitraApply — Pengajuan (Application)

Everything from initial interest to store opening:
- Daftar (register application)
- Izin kepala daerah check (regional head permit verification)
- Down payment tracking (Rp4 juta)
- Document collection & verification (KTP, NPWP, SIUP, IMB, NIB, etc.)
- Application status tracking
- Admin review & approval workflow
- Communication between applicant & Indomaret

**Users**: Calon mitra (franchise applicants), Indomaret admin reviewers

### App 2: MitraSurvey — Survey & Assessment

Location feasibility surveys during application AND ongoing surveys for active stores:
- Survey assignment & scheduling
- Field data collection (photos, measurements, area assessment)
- Feasibility scoring / reporting
- Survey result submission
- Used during application process (triggered from MitraApply)
- Also used post-opening for active store assessments (triggered from MitraOps)

**Users**: Indomaret surveyors/field staff, registered mitra (for some survey tasks)

### App 3: MitraOps — Pasca Buka Toko (Post-Opening)

Everything after the store is operational:
- Store performance dashboard
- Financial reporting (omzet, royalty with progressive 0-4% structure)
- Operational management
- Inventory / supply chain visibility
- Compliance & audit
- Contract renewal management (5-year terms)

**Users**: Mitra aktif (active franchise owners), operational staff

## High-Level Registration Process

**Goal: Reduce from 8 months → 3 months**

```
Daftar (MitraApply)
  → Cek izin kepala daerah (MitraApply)
    → Down payment Rp4 juta (MitraApply)
      → Survey assessment mendalam (MitraSurvey)
        → All clear? → Renovasi & buka gerai
          → Store live → MitraOps
```

## Inter-App Communication

**Pattern: Pub/Sub** — apps are separate but status/messaging must flow smoothly.

```
MitraApply ──publishes──→ [Event Bus] ──subscribes──→ MitraSurvey
MitraSurvey ─publishes──→ [Event Bus] ──subscribes──→ MitraApply
MitraApply ──publishes──→ [Event Bus] ──subscribes──→ MitraOps (handoff)
MitraOps ────publishes──→ [Event Bus] ──subscribes──→ MitraSurvey (ongoing surveys)
```

Example events:
- `application.ready_for_survey` — MitraApply → MitraSurvey
- `survey.completed` — MitraSurvey → MitraApply
- `application.approved` — MitraApply → MitraOps (handoff)
- `store.survey_requested` — MitraOps → MitraSurvey

**Technology options** (TBD — needs clarification from Ivan):
- Google Cloud Pub/Sub (managed, scalable)
- Redis Streams (simpler, lower cost for early stage)

## Architecture Overview

| Aspect | MitraApply | MitraSurvey | MitraOps |
|---|---|---|---|
| Database | Separate | Separate | Separate |
| Auth | Separate | Separate | Separate |
| Users | Applicants + admin | Surveyors + mitra | Franchise owners + ops |
| Platform | Web + Android + iOS | Web + Android + iOS | Web + Android + iOS |
| Data lifecycle | Medium (application) | Short (per survey) | Long (ongoing ops) |
| Traffic pattern | Burst (promo) | Event-driven | Steady (daily) |
| Communication | Publishes events | Subscribes + publishes | Subscribes + publishes |

## Rationale

- **Different users** — three distinct user bases with minimal overlap
- **Different data models** — application docs vs survey data vs operational data
- **Security isolation** — sensitive PII (KTP, NPWP) isolated per app
- **Independent delivery** — can ship apps in priority order
- **Independent scaling** — different traffic patterns per app
- **MitraSurvey spans both phases** — serves application process AND post-opening, hence its own app
- **Pub/sub for loose coupling** — apps don't need direct API dependencies, events propagate naturally

## Platform Strategy

**Design for all three platforms (Web + Android + iOS), execute web first.**
- AI-assisted build for speed
- EDTS handles ongoing maintenance
- Mobile follows web, shared design system

## Status

Accepted — confirmed by Ivan Jonathan (2026-02-26, call + WhatsApp).

## Resolved Questions

- ~~Two apps or three?~~ → **Three: MitraApply, MitraSurvey, MitraOps**
- ~~Shared auth or separate?~~ → **Separate auth**
- ~~Platform?~~ → **Web + Android + iOS** (web first)
- ~~Inter-app communication?~~ → **Pub/sub** (specific tech TBD)

## Open Questions (Need Clarification from Ivan)

- Pub/sub technology choice: Google Cloud Pub/Sub vs Redis Streams vs other?
- EDTS tech stack preferences for each app?
- Does EDTS scope cover all 3 apps or prioritize?
- Existing Indomaret systems to integrate with?
- Timeline per app?
- What specific surveys do registered mitra perform?
