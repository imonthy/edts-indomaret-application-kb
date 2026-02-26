---
title: "ADR-001: Separate Application Phase from Post-Opening Phase"
date: 2026-02-26
status: proposed
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

Split the product into two clear phases:

### Phase 1: Pengajuan (Application)
Everything from initial interest to MoU signing / store opening:
- Franchise application form
- Document collection & verification
- Location proposal & feasibility
- Application status tracking
- Admin review & approval workflow
- Payment tracking (franchise fee)
- Communication between applicant & Indomaret

### Phase 2: Pasca Buka Toko (Post-Opening)
Everything after the store is operational:
- Store performance dashboard
- Financial reporting (omzet, royalty)
- Operational management
- Inventory / supply chain visibility
- Compliance & audit
- Contract renewal management

## Rationale

- Different user needs at each stage (applicant vs franchise owner)
- Different data models and workflows
- Can ship Phase 1 first without waiting for Phase 2
- Alfamart's mistake: mixing both in one fragmented system (ALFA app)
- 7-Eleven does this well — separate portals for recruitment vs operations

## Status

Proposed — awaiting validation with Ivan.

## Open Questions

- Are these two separate apps or two modules in one platform?
- Does EDTS scope cover both phases or only Phase 1?
- Does Indomaret have existing systems for Phase 2 that we'd integrate with?
