---
title: "ADR-001: Separate Application Phase from Post-Opening Phase"
date: 2026-02-26
status: accepted
tags:
  - architecture
  - scope
  - phases
---

# ADR-001: Separate Application Phase from Post-Opening Phase

## Context

From initial discussion (Monthy, 2026-02-26):

> "kepikir mau pisahin fasa pengajuan dgn fasa setelah toko buka"

The idea is to treat franchise application and post-store-opening as two distinct phases, potentially two separate systems or modules.

## Decision

**Two separate applications with separate databases and separate authentication.**

Ivan confirmed (2026-02-26, 19:27–19:59):
- Separate DB → separate app
- "kalo terpisah lebih aman" — security through isolation
- "usernya beda bgt" — completely different user bases, no shared auth needed

### App 1: Pengajuan (Application)
Everything from initial interest to MoU signing / store opening:
- Franchise application form (data diri, lokasi, modal)
- Document collection & verification (KTP, NPWP, SIUP, IMB, NIB, etc.)
- Location proposal & feasibility survey
- Application status tracking
- Admin review & approval workflow
- Payment tracking (franchise fee ~Rp494M)
- Communication between applicant & Indomaret

**Users**: Calon mitra (franchise applicants), Indomaret admin reviewers

### App 2: Pasca Buka Toko (Post-Opening)
Everything after the store is operational:
- Store performance dashboard
- Financial reporting (omzet, royalty with progressive 0-4% structure)
- Operational management
- Inventory / supply chain visibility
- Compliance & audit
- Contract renewal management (5-year terms)

**Users**: Mitra aktif (active franchise owners), operational staff

## Architecture

| Aspect | App 1 (Pengajuan) | App 2 (Pasca Buka Toko) |
|---|---|---|
| Database | Separate | Separate |
| Auth | Separate | Separate |
| Users | Applicants + admin reviewers | Franchise owners + ops staff |
| Data lifecycle | Short (application process) | Long (ongoing operations) |
| Traffic pattern | Burst (promo periods) | Steady (daily operations) |

## Rationale

- **Different users** — applicants vs active franchise owners have no overlap
- **Different data models** — application documents vs operational/financial data
- **Security isolation** — sensitive application data (KTP, NPWP) isolated from operational data
- **Independent delivery** — App 1 can ship without waiting for App 2
- **Independent scaling** — different traffic patterns, different resource needs
- **Competitor lesson** — Alfamart mixed both in ALFA app → fragmented, low adoption (3.5/5, 10K downloads for 3,600+ partners)
- **Best practice** — 7-Eleven separates recruitment portal from operations app

## Status

Accepted — confirmed by Ivan Jonathan (2026-02-26).

## Resolved Questions

- ~~Are these two separate apps or two modules in one platform?~~ → **Two separate apps**
- ~~Shared auth or separate?~~ → **Separate auth** (users are completely different)

## Open Questions

- Does EDTS scope cover both apps or App 1 (Pengajuan) only?
- Does Indomaret have existing systems for post-opening that we'd integrate with?
- Platform for each app (web, mobile, both)?
