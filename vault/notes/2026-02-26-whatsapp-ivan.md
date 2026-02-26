---
title: WhatsApp — Ivan Jonathan — Initial Request
date: 2026-02-26
status: draft
tags:
  - conversation
  - initial-request
  - ivan-jonathan
---

# WhatsApp — Ivan Jonathan — Initial Request

## Conversation (2026-02-26, 18:06–18:19)

```
[18.06.50] Ivan Jonathan: Mon
[18.07.04] Monthy: Yo
[18.08.13] Ivan Jonathan: Perlu bikin aplikasi nih
[18.08.19] Ivan Jonathan: Hahaha
[18.08.28] Monthy: Aplikasi apa?
[18.09.00] Ivan Jonathan: Pengajuan franchise
[18.09.16] Monthy: Interesting
[18.09.38] Monthy: Franchise apa
[18.19.11] Ivan Jonathan: Indomaret
[18.19.23] Monthy: Sipp
[18.19.32] Monthy: Di platform apa aja?
[18.19.37] Ivan Jonathan: Iya
```

## Conversation (2026-02-26, 19:27–19:59) — Architecture Decision

```
[19.27] Ivan Jonathan: Iya, gw kepikir mau pisahin fasa pengajuan dgn fasa setelah toko buka
[19.51] Monthy: separate apps or one platform with two modules?
[19.54] Ivan Jonathan: Separate db
[19.54] Monthy: Separate db tapi 1 app? Or 2 app?
[19.58] Ivan Jonathan: Yg aman gmn ya?
[19.58] Ivan Jonathan: Gw pikir kalo terpisah lebih aman
[19.59] Ivan Jonathan: Dan usernya beda bgt
```

## Key Takeaways

- Ivan needs a **franchise application ("pengajuan franchise")** system
- Brand: **Indomaret**
- Platform question was asked but not clearly answered ("Iya" is ambiguous)
- **Architecture decided: 2 separate apps, separate DBs, separate auth**
  - Ivan's reasoning: "lebih aman" (more secure) + "usernya beda bgt" (users are very different)
  - Phase 1 users: calon mitra (franchise applicants) + admin reviewers
  - Phase 2 users: mitra aktif (active franchise owners) + operational staff

## Follow-up Needed

- Clarify platform (web, mobile, both?)
- Get detailed requirements / feature list
- Understand user roles (who submits, who reviews)
- Timeline and budget
- Any existing systems at EDTS to integrate with
