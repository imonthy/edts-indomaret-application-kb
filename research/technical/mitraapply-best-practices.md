---
title: "MitraApply — Franchise Application System Best Practices Research"
date: 2026-02-26
status: draft
tags:
  - technical-research
  - best-practices
  - mitraapply
  - franchise-application
  - pengajuan
---

# MitraApply — Franchise Application System Best Practices Research

> Research into best-in-class patterns for building a digital franchise application/submission system (MitraApply: Pengajuan), covering UX, document verification, pipeline management, payment tracking, admin workflows, and Indonesia-specific considerations.

## Table of Contents

1. [Franchise Application System Best Practices](#1-franchise-application-system-best-practices)
2. [Document Collection & Verification](#2-document-collection--verification)
3. [Application Tracking & CRM Patterns](#3-application-tracking--crm-patterns)
4. [Payment Integration for High-Value Transactions](#4-payment-integration-for-high-value-transactions)
5. [Admin Review & Approval Workflow Patterns](#5-admin-review--approval-workflow-patterns)
6. [Location Proposal & Feasibility Survey](#6-location-proposal--feasibility-survey)
7. [Communication Between Applicant & Admin](#7-communication-between-applicant--admin)
8. [Digital Signatures & Legal Compliance](#8-digital-signatures--legal-compliance)
9. [Consolidated Feature Recommendations for MitraApply](#9-consolidated-feature-recommendations-for-mitraapply)
10. [Sources](#10-sources)

---

## 1. Franchise Application System Best Practices

### What Best-in-Class Looks Like

The gold standard for digital franchise application systems in 2025–2026 is defined by **centralized platforms** that replace fragmented email/spreadsheet/paper processes with a single integrated system. Key characteristics:

**Multi-step progressive disclosure forms.** Rather than presenting applicants with a massive single-page form, best-in-class systems break applications into logical stages. Research from the Interaction Design Foundation and Nielsen Norman Group confirms that progressive disclosure — showing only essential fields first and revealing complexity on demand — improves learnability, reduces error rates, and increases completion rates. Multi-step forms have measurably lower bounce rates and higher conversion than monolithic forms.

**Mobile-first design.** Given that Indonesia's internet traffic is overwhelmingly mobile (70%+), the form must be designed mobile-first. This means large touch targets, minimal typing (use dropdowns, date pickers, camera-based document upload), and responsive layouts that work on low-end Android devices.

**Save-and-resume functionality.** Franchise applications require detailed inputs (financial data, location details, document uploads) that applicants cannot always complete in one session. Best practice is to auto-save progress and allow applicants to return via a magic link or authenticated session.

**Real-time validation with clear feedback.** Inline validation (not just on-submit) for fields like NIK format (16 digits), NPWP format, phone number, and email. Error messages in Bahasa Indonesia, specific to each field.

**Conversion optimization patterns from franchise industry:**
- Sticky CTAs (call-to-action buttons that remain visible as the user scrolls)
- Email verification early in the process to ensure lead quality
- Progress indicators showing completion percentage
- Short initial form (name, phone, location, capital range) to capture leads before requiring full details
- "Hubungi Kami" (Contact Us) floating button for WhatsApp/chat support

**International benchmarks:**
- **FranConnect** (1,500+ franchise brands): Seven-stage pipeline from Discovery → Inquiry → Qualification → Application → Discovery Day → Agreement/FDD → Commitment. Each stage triggers automated workflows. Their "Sales Accelerator" reduced one brand's pipeline from 6 months to 2 weeks.
- **7-Eleven (USA)**: Salesforce CRM with automated credit checks, separate recruitment portal, 7University training app. Candidate scoring and automated follow-up at every stage.
- **Circle K**: Won 2025 NACS Award for AI-powered recruitment platform with gamified onboarding that reduced turnover by 13%.

### Indonesia-Specific Relevance

- **Indomaret's current state**: Basic web form at `indomaret.co.id/home/index/pendaftaran-waralaba`, 4-step process (Online Form → Presentation 1 → Presentation 2 → MoU), no status tracking, no document portal, no digital payment tracking.
- **Alfamart's ALFA Franchise app**: 10K downloads for 3,600+ partners (extremely low adoption), 3.5/5 rating. Fragmented across 3 platforms. Registration page frequently in maintenance.
- The bar is very low domestically — any modern, well-designed system would leapfrog both competitors.
- Indonesian users strongly prefer **WhatsApp** for communication. Any system must integrate WhatsApp notifications alongside in-app notifications.
- Bahasa Indonesia as primary language, with business/legal terms in Indonesian is mandatory.

### Concrete Recommendations for MitraApply

1. **Two-phase form approach**: Quick interest form (5–7 fields: nama, nomor HP, email, kota, estimasi modal, jenis waralaba) to capture leads fast. Full application form after qualification, broken into 4–5 steps with progress bar.
2. **Mobile-first, responsive web app** — not native app initially (reduces friction; no app store download required). Consider PWA for offline capability.
3. **Auto-save on every field change** with "Lanjutkan Nanti" (Continue Later) button.
4. **WhatsApp integration** for status notifications at every stage transition.
5. **Sticky floating CTA** on the landing page and within the form.
6. **Bahasa Indonesia throughout** with contextual help tooltips for technical terms (e.g., "NIB", "SIUP", "IMB/PBG").

---

## 2. Document Collection & Verification

### What Best-in-Class Looks Like

Modern franchise and financial onboarding systems use **eKYC (electronic Know Your Customer)** to digitally collect, extract, and verify identity and business documents. The key technologies are:

**OCR (Optical Character Recognition):** AI-powered extraction of structured data from photographed or scanned documents. For Indonesian KTP, best-in-class OCR achieves 98–99.5% accuracy, extracting NIK, name, address, date of birth, gender, and religion from the card image.

**Government database verification (Dukcapil):** The critical differentiator in Indonesia. The best eKYC providers can cross-reference extracted data (especially NIK) against the Dukcapil (Civil Registration) database to confirm identity authenticity. This is the Indonesian equivalent of a credit check or SSN verification in the US.

**Liveness detection & face matching:** For high-value applications like franchise ownership (Rp494M+ investment), biometric verification adds a security layer. The applicant takes a selfie that is matched against the photo on their KTP, with liveness detection to prevent spoofing.

**Multi-document support:** Franchise applications require many document types beyond KTP:

| Document | Purpose | Verification Approach |
|---|---|---|
| KTP (Kartu Tanda Penduduk) | Identity verification | OCR + Dukcapil database check |
| NPWP (Nomor Pokok Wajib Pajak) | Tax ID verification | OCR + DJP/Coretax validation |
| KK (Kartu Keluarga) | Family card / address proof | OCR extraction |
| SIUP (Surat Izin Usaha Perdagangan) | Business license | OCR + manual review |
| NIB (Nomor Induk Berusaha) | Business ID from OSS | OSS system cross-reference |
| IMB/PBG | Building permit | Manual review + document authenticity check |
| Sertifikat Bangunan | Building ownership certificate | Manual review |
| STPW | Franchise registration certificate | Manual review |
| Ijin Lingkungan | Environmental permit | Manual review |
| Domisili | Domicile letter | Manual review |
| Denah Lokasi | Location floor plan | Manual review (uploaded as image/PDF) |
| Bukti Kepemilikan Modal | Proof of capital | Bank statement OCR or manual review |

### Key Indonesian eKYC Providers

| Provider | Key Capabilities | Notable Clients |
|---|---|---|
| **Verihubs** | OCR for KTP/SIM/NPWP/KK/Passport (98–99.9% accuracy), face recognition, liveness detection, deepfake detection, Dukcapil verification | Wide adoption across fintech |
| **GLAIR AI** | OCR for KTP/KK/NPWP/STNK/BPKB, Dukcapil integration, face verification, active/passive liveness | Financial services |
| **ASLI RI** | Biometric + facial recognition, Indonesian regulatory compliance | Bank BRI, OVO, Kredivo |
| **Arya.ai** | KTP extraction API, fast integration | Fintech, gig economy |
| **Iluma AI** | NPWP validator (updated for 2025 format changes), bank name validator, ID card OCR | Financial services |

### Indonesia's Digital Identity Landscape

- **IKD (Identitas Kependudukan Digital)**: Indonesia is rolling out a smartphone app that digitizes KTP, KK, and birth certificates. MitraApply should be designed to accept IKD-based verification in the future.
- **OSS (Online Single Submission)**: The national business licensing platform at oss.go.id issues NIB. The OSS-RBA system integrates with Dukcapil, Coretax (tax), AHU (legal entities), and spatial planning databases. While no public API exists for third-party integration, the system's NIB numbers can be cross-referenced.
- **NPWP format change (2025)**: The old 15-digit NPWP is being phased out in favor of NIK-based tax identification. MitraApply must support both formats during the transition.

### Concrete Recommendations for MitraApply

1. **Integrate a single eKYC provider** (Verihubs or GLAIR recommended) for KTP OCR + Dukcapil verification + face matching. This handles the core identity verification automatically.
2. **Camera-first document upload**: On mobile, open the camera directly rather than a file picker. Guide users with an overlay frame showing where to position the document (like banking apps do).
3. **Two-tier verification**: Automated (OCR + database check) for KTP, NPWP, NIB. Manual admin review for business permits (SIUP, IMB, STPW, etc.) that lack centralized databases.
4. **Document status per-item**: Each uploaded document should have its own status (Belum Diunggah / Menunggu Verifikasi / Terverifikasi / Ditolak — with rejection reason).
5. **Re-upload flow**: When a document is rejected (blurry photo, expired, wrong document), the applicant gets a specific notification with the reason and can re-upload just that document without restarting the application.
6. **Secure storage**: All documents encrypted at rest (AES-256). Comply with UU PDP (Law No. 27 of 2022 on Personal Data Protection). Data retention policy aligned with franchise lifecycle.
7. **Progressive document collection**: Do not require all documents upfront. Collect KTP + NPWP at application stage; collect business permits and location documents after initial qualification.

---

## 3. Application Tracking & CRM Patterns

### What Best-in-Class Looks Like

Franchise-specific CRM systems like FranConnect, FranchiseSoft, BrandWide, and ClientTether all converge on a **stage-based pipeline model** with automation at each transition. Key patterns:

**Defined pipeline stages.** The industry standard is 5–8 stages, customized to the franchisor's process. FranConnect's canonical model:

```
Discovery → Inquiry → Qualification → Application → Discovery Day → Agreement → Commitment
```

Mapped to Indomaret's current 4-step process + MitraApply enhancements:

```
1. Pendaftaran Awal (Initial Registration) — Quick interest form
2. Kualifikasi (Qualification) — Admin reviews basic eligibility
3. Pengajuan Lengkap (Full Application) — Complete form + document upload
4. Verifikasi Dokumen (Document Verification) — Admin verifies documents
5. Survei Lokasi (Location Survey) — Feasibility assessment
6. Presentasi & Negosiasi (Presentation & Negotiation) — Indomaret presents terms
7. Pembayaran & MoU (Payment & MoU) — Fee payment + contract signing
8. Persiapan Buka Toko (Pre-Opening Preparation) — Handoff to MitraOps
```

**Lead scoring.** FranConnect's "Lead Heat Index" scores applicants based on behavioral signals (form completion %, document upload speed, response time to communications, financial qualification). This helps admins prioritize the most promising applicants. FranConnect's AI "Sales Coach" ranks leads by engagement and investment readiness.

**Automated stage transitions.** When all required actions for a stage are complete (e.g., all documents uploaded and verified), the system can either auto-advance to the next stage or queue for admin approval of the transition. Triggers include:
- Document verification complete → auto-advance to Survey stage
- Payment confirmed → auto-advance to MoU stage
- SLA timeout (e.g., applicant inactive for 14 days) → auto-send reminder, then auto-archive after 30 days

**Automated notifications at every stage change:**
- Applicant receives: "Selamat! Aplikasi Anda telah lolos tahap Kualifikasi. Silakan lengkapi dokumen di tahap berikutnya."
- Admin receives: "Aplikasi baru dari [Nama] siap untuk review di tahap Verifikasi Dokumen."

**Pipeline visibility dashboards:**
- Funnel view: How many applicants at each stage
- Time-in-stage metrics: Average days per stage, identifying bottlenecks
- Conversion rates between stages
- Aging alerts: Applicants stuck at a stage beyond SLA threshold

**Key platform capabilities from leading franchise CRMs:**

| Feature | FranConnect | FranchiseSoft | BrandWide | ClientTether |
|---|---|---|---|---|
| Customizable pipeline stages | Yes | Yes | Yes | Yes |
| Lead scoring / heat index | AI-powered | Basic | Basic | Basic |
| Automated workflows | Yes, per-stage | Yes | Yes | Yes |
| Document management | Yes | Yes | Yes | Limited |
| E-signatures | Yes | Yes (DocuSign) | Yes | Yes |
| AI candidate coaching | Frannie AI | No | No | No |
| Onboarding checklists | Opener module | Yes | Yes | No |
| Analytics / dashboards | Advanced | Standard | Standard | Standard |
| WhatsApp integration | No (US-focused) | No | No | No |

### Indonesia-Specific Relevance

- None of the major franchise CRMs have Indonesian market support, WhatsApp integration, or Bahasa Indonesia UI. MitraApply must build this natively.
- Indonesian applicants expect **WhatsApp as primary notification channel**, not email. In-app notifications are secondary.
- The pipeline must accommodate Indomaret's specific process: location survey is a critical gate (many applicants are rejected at this stage due to location unsuitability).

### Concrete Recommendations for MitraApply

1. **8-stage pipeline** as mapped above, with configurable stage definitions (admin can rename stages, add/remove stages, change required actions per stage).
2. **Applicant-facing status tracker**: Simple, visual progress bar (like a Shopee/Tokopedia order tracker) showing current stage, completed stages, and next steps. Indonesian users are very familiar with this pattern from e-commerce.
3. **Admin dashboard** with: Funnel view, per-stage counts, aging alerts, lead scoring (basic: completeness + responsiveness + financial qualification).
4. **WhatsApp notifications** via WhatsApp Business API (providers: Mekari Qontak, Sprint Asia, or global providers like Twilio/Infobip) for all stage transitions and action-required alerts.
5. **Push notifications + email as fallback** channels.
6. **Automated reminders**: If applicant has not completed required actions within X days (configurable), send reminder via WhatsApp. After Y days, send escalation. After Z days, mark as inactive.
7. **Kanban board for admins**: Drag-and-drop applicant cards between stages, with filters by region, date, status, and assigned reviewer.

---

## 4. Payment Integration for High-Value Transactions

### What Best-in-Class Looks Like

Franchise fee payments (~Rp494M / ~$30K USD) are fundamentally different from e-commerce payments. They are high-value, milestone-based, and require robust tracking, reconciliation, and audit trails.

**Milestone-based payment structure.** Best practice for high-value franchise transactions is to break the total fee into milestones tied to application progress:

| Milestone | Typical Timing | Example Amount |
|---|---|---|
| Registration fee / commitment deposit | After qualification | Rp5–10M (refundable/non-refundable) |
| Franchise fee installment 1 | After location survey approval | Rp36M (franchise fee for 5 years) |
| Store build-out / investment deposit | After MoU signing | Rp100–150M |
| Equipment & inventory | Pre-opening | Rp150–200M |
| Final settlement | Store opening | Remaining balance |

**Virtual Account (VA) payments.** For high-value B2B payments in Indonesia, Virtual Accounts are the standard. Each applicant gets a unique VA number per payment milestone, enabling automatic reconciliation. Major banks supported: BCA, Mandiri, BRI, BNI, Permata.

**BI-FAST for real-time settlement.** Indonesia's national real-time payment infrastructure (BI-FAST) connects 135+ banks for 24/7 instant transfers, processing in under 25 seconds. Ideal for confirming large franchise payments.

**Escrow concepts.** While full escrow (third-party holding funds) may be overkill for a direct franchisor-franchisee relationship, the concept of **milestone-gated fund release** is relevant. Payments confirmed at each milestone before advancing to the next stage — similar to construction escrow systems.

### Key Indonesian Payment Providers

| Provider | Key Strengths for MitraApply |
|---|---|
| **Midtrans** (by GoTo) | Most complete payment gateway in Indonesia. 500K+ businesses. Virtual accounts, bank transfers, QRIS. Aegis fraud detection. Payout system for disbursements. |
| **Xendit** | Strong API. Virtual accounts with real-time webhook notifications. Batch payouts. Marketplace fund routing. 38+ payment methods. |
| **DOKU** | One of Indonesia's oldest payment gateways. 150K+ merchants. All major payment types. |
| **Flip** | Fee-free interbank transfers. Good for disbursements/refunds. |

### Indonesia-Specific Considerations

- **Bank transfer is king**: For high-value transactions, Indonesians overwhelmingly use bank transfers (not e-wallets or credit cards). Virtual Accounts via major banks are the expected payment method.
- **Manual reconciliation is common pain point**: Many Indonesian businesses still reconcile payments manually by checking bank statements. Automated reconciliation via payment gateway webhooks is a major value-add.
- **Payment proof culture**: Indonesian business culture often involves sending "bukti transfer" (transfer receipt screenshot) via WhatsApp. MitraApply should support this as a fallback while prioritizing automated VA reconciliation.
- **Tax implications**: Franchise fee payments may involve PPN (VAT) and PPh (income tax withholding). The system should generate proper tax invoices (faktur pajak) or integrate with tax systems.

### Concrete Recommendations for MitraApply

1. **Integrate Midtrans or Xendit** as primary payment gateway. Both offer Virtual Account support across major banks with webhook-based real-time payment confirmation.
2. **Milestone payment tracker**: Visual timeline in the applicant portal showing each payment milestone, amount due, due date, payment status (Belum Dibayar / Menunggu Konfirmasi / Lunas).
3. **Unique Virtual Account per milestone**: When a milestone is due, generate a VA number for the applicant. Payment auto-confirms via webhook and advances the pipeline stage.
4. **Manual payment confirmation fallback**: Allow admin to manually confirm payment with uploaded bukti transfer, for cases where VA payment fails or applicant uses different payment method.
5. **Payment receipt generation**: Auto-generate digital receipts (kwitansi) for each confirmed payment, with e-Materai for amounts over Rp5,000,000.
6. **Payment reminder automation**: WhatsApp + in-app reminders for upcoming due dates and overdue payments.
7. **Refund tracking**: For cases where applications are rejected after partial payment, track refund status with the same milestone-based UI.
8. **Audit trail**: Every payment event (generated, attempted, confirmed, failed, refunded) logged with timestamp, amount, method, and confirming user.

---

## 5. Admin Review & Approval Workflow Patterns

### What Best-in-Class Looks Like

Research from Atlassian, Wrike, Moxo, and Kissflow converges on these principles for multi-stage approval workflows:

**Optimal stage count: 3–4 review stages.** Over-complicating with too many approval levels creates bottlenecks. For MitraApply, the key review gates are:

```
Gate 1: Initial Qualification (Admin reviews basic eligibility)
Gate 2: Document Verification (Admin verifies uploaded documents)
Gate 3: Location Feasibility (Surveyor approves proposed location)
Gate 4: Final Approval (Senior admin / management approves MoU)
```

**Clear roles and ownership.** Every review stage must have a designated approver or approver pool. Key roles for MitraApply:

| Role | Responsibility |
|---|---|
| **Admin Reviewer** | Initial qualification, document verification, routine approvals |
| **Location Surveyor** | On-site feasibility assessment, location scoring |
| **Senior Admin / Manager** | Final approval for MoU, exception handling, escalations |
| **Finance Admin** | Payment verification, refund approvals |

**SLA tracking and enforcement.** Each review stage should have a defined SLA (Service Level Agreement):

| Stage | Recommended SLA | Escalation |
|---|---|---|
| Initial qualification review | 2 business days | Auto-escalate to manager after 3 days |
| Document verification | 3 business days per document batch | Notify team lead after 4 days |
| Location survey scheduling | 5 business days after doc verification | Auto-notify regional manager |
| Location survey completion | 10 business days after scheduling | Escalate to operations head |
| Final approval | 3 business days after survey report | Auto-escalate to director level |

**Key metrics to track:**
- **Cycle time**: Total days from application submission to MoU signing (target: define based on Indomaret's current process, then improve)
- **Time-in-stage**: Average days at each review stage
- **SLA compliance rate**: Percentage of reviews completed within SLA
- **First-pass approval rate**: Percentage of documents approved without rework
- **Rejection rate by stage**: Identifies where most applicants drop off (useful for process improvement)

**Parallel vs. sequential review.** Some review stages can run in parallel:
- Document verification and location survey scheduling can happen simultaneously
- Financial qualification and background check (if any) can run in parallel
- This reduces total cycle time significantly

**Automation patterns:**
- Auto-assign new applications to reviewers based on workload balancing or region
- Auto-send checklists to reviewers when an application enters their stage
- Auto-escalate when SLA is breached
- Auto-archive inactive applications after configurable timeout
- Conditional routing: Applications above a certain investment threshold route to senior approval; standard applications follow normal flow

**Audit trail and compliance:**
- Every review action (approve, reject, request revision, reassign, escalate) logged with timestamp, user ID, and comments
- Segregation of duties: The person who verifies documents should not be the same person who gives final approval
- Role-based access: Reviewers only see applications assigned to them or their region

### Indonesia-Specific Relevance

- Indomaret's current process is largely manual — applications likely tracked in spreadsheets or internal systems with limited automation.
- Indonesian business culture values personal relationships and communication. The system should make it easy for admins to add personal notes and communicate with applicants, not just automated messages.
- Regional distribution: Indomaret operates across Indonesia. Review workflows should support regional assignment (Jabodetabek, Jawa Barat, Jawa Timur, Sumatera, etc.) so local teams handle local applications.

### Concrete Recommendations for MitraApply

1. **4 review gates** as defined above, with configurable SLAs per gate.
2. **Role-based admin system**: Admin Reviewer, Location Surveyor, Senior Admin, Finance Admin, Super Admin (system configuration).
3. **Assignment engine**: Auto-assign to available reviewer in the applicant's region. Support manual reassignment.
4. **Review interface per-stage**: Structured review form with checklist items, approve/reject/request revision buttons, and mandatory comment field on rejection.
5. **SLA dashboard**: Real-time view of SLA compliance across all stages, with red/yellow/green indicators. Email/WhatsApp alerts to managers when SLAs are breached.
6. **Parallel processing**: Allow document verification and location survey to run simultaneously where possible.
7. **Batch operations**: Admin can approve/reject multiple documents or applications at once for efficiency.
8. **Review history**: Full audit trail visible to senior admins. Every action timestamped and attributed.
9. **Rejection templates**: Pre-defined rejection reasons (Dokumen tidak jelas / Lokasi tidak memenuhi syarat / Modal tidak mencukupi / etc.) with free-text field for additional notes. This ensures consistency and speeds up the review process.

---

## 6. Location Proposal & Feasibility Survey

### What Best-in-Class Looks Like

Location selection is one of the most critical factors in franchise success. Modern systems combine **digital mapping, demographic data, and structured survey workflows**.

**Digital location submission:**
- Applicant drops a pin on an interactive map (Google Maps / Mapbox) to identify their proposed location
- System auto-captures GPS coordinates, address, and basic area information
- Applicant uploads photos of the location (interior, exterior, surrounding area, street view)
- Applicant provides location details: size (m²), ownership status (milik sendiri/sewa), lease terms, distance from main road

**GIS-based preliminary assessment:**
- Automated proximity check: How close is the proposed location to existing Indomaret/Alfamart stores? (cannibalization risk)
- Demographic overlay: Population density, household income levels, foot traffic estimates in the area
- Competitor mapping: Other convenience stores, minimarkets, and retail within a radius
- Tools like Esri ArcGIS, Maptitude, and GrowthFactor provide these capabilities, though for the Indonesian market, Google Maps Platform + BPS (Badan Pusat Statistik) data may be more practical

**Structured survey workflow:**
- After preliminary assessment passes, a physical survey is scheduled
- Surveyor receives a mobile-friendly checklist (can use the same MitraApply app with a surveyor role)
- Survey checklist items: accessibility, visibility, parking, foot traffic count, competitor proximity, building condition, lease terms verification, regulatory compliance (zoning), utility infrastructure
- Surveyor uploads geotagged photos and completes the structured assessment form
- Survey report auto-generates a feasibility score based on weighted criteria

**International benchmark — 7-Eleven (USA):**
- Uses Salesforce CRM integrated with commercial real estate data
- AI-assisted site scoring based on historical performance of similar locations
- Trade area analysis with demographic overlays

**International benchmark — GrowthFactor:**
- AI agent automates site evaluation, processing 5x more potential sites than manual methods
- 99.8% accuracy claimed for data-driven site selection
- Territory mapping for exclusive franchise zones

### Indonesia-Specific Relevance

- Location is the #1 decision factor for Indomaret franchise approval. Many applicants are rejected at this stage.
- Indomaret already has a massive network (23,100+ stores). Cannibalization analysis (distance from existing stores) is critical.
- Indonesian addresses can be imprecise. GPS/pin-drop is far more reliable than typed addresses.
- Local factors matter: proximity to sekolah (schools), pasar (markets), perumahan (housing complexes), perkantoran (office areas), jalan utama (main roads).
- Physical survey by local teams is essential and cannot be fully replaced by digital tools in the Indonesian context. But the survey process itself should be digitized.

### Concrete Recommendations for MitraApply

1. **Interactive map for location submission**: Google Maps integration. Applicant drops pin, uploads photos, fills in location details. GPS coordinates auto-captured.
2. **Automated proximity check**: Show distance to nearest existing Indomaret stores. Flag locations within a configurable minimum radius (e.g., <500m from existing store).
3. **Location photo requirements**: Mandatory photo uploads — minimum 4 photos: storefront, interior, street view left, street view right. Guide with overlay examples.
4. **Surveyor mobile module**: Dedicated interface for surveyors within the same platform. Checklist-based assessment form, geotagged photo upload, structured scoring.
5. **Survey scheduling**: Built-in scheduling system. Applicant can see proposed survey dates. Surveyor receives calendar integration.
6. **Feasibility score**: Weighted scoring based on survey results, auto-calculated. Configurable weights (e.g., accessibility 20%, foot traffic 25%, competition 20%, building condition 15%, lease terms 20%).
7. **Survey report generation**: Auto-generated PDF report from survey data for internal review and applicant presentation.

---

## 7. Communication Between Applicant & Admin

### What Best-in-Class Looks Like

Effective applicant communication requires a **multi-channel approach** with a centralized message history.

**In-app messaging:**
- Threaded conversations per application (similar to customer support ticket systems)
- Messages visible to both applicant and assigned admin reviewer
- Rich messages: text, images, document attachments, links
- In-app messages have 75% open rate — nearly 3x higher than push notifications
- In-app messaging has 18–20x higher click-through rates than push or web notifications

**WhatsApp Business API integration:**
- Indonesia's dominant communication channel. 87% of Indonesian internet users use WhatsApp.
- Official WhatsApp BSPs in Indonesia: **Mekari Qontak** (ISO 27001 certified, Kominfo registered, official Meta partner), **Sprint Asia**, or global providers (Twilio, Infobip, Sinch)
- Use cases: Stage transition notifications, document rejection alerts, payment reminders, survey scheduling confirmations, MoU signing reminders
- Pre-approved message templates (required by Meta) for transactional notifications
- Interactive messages with buttons ("Lihat Detail" / "Unggah Dokumen") to drive users back to the app
- End-to-end encrypted by default

**Notification hierarchy:**
1. **WhatsApp** (primary) — highest open/response rate in Indonesia
2. **In-app notification + badge** — for users who are active in the app
3. **Push notification** (mobile) — for re-engagement
4. **Email** — for formal communications (receipts, MoU drafts, official letters)
5. **SMS** — fallback for users without WhatsApp (rare but possible)

**Communication automation:**
- Automated messages at every stage transition (customizable templates in Bahasa Indonesia)
- Reminder sequences: Day 3 → Day 7 → Day 14 for incomplete actions
- Personalized messages using applicant name and application details
- Escalation: If applicant is unresponsive after X days, notify senior admin

**Real-world examples in Indonesia:**
- Tokopedia, Gojek, Traveloka, Bank Mandiri, and Shopee all use WhatsApp Business API for transactional notifications
- Banking apps in Indonesia use WhatsApp for balance alerts, transaction confirmations, and customer service

### Concrete Recommendations for MitraApply

1. **In-app message center**: Threaded conversations per application. Both applicant and admin can send messages, attach documents, and view full history.
2. **WhatsApp Business API integration**: Via Mekari Qontak or equivalent BSP. Pre-built templates for every stage transition and action-required notification.
3. **Notification preferences**: Allow applicants to choose primary notification channel (WhatsApp on by default).
4. **Automated notification engine**: Configurable triggers and templates for each pipeline event. Admin can preview and customize message templates.
5. **Quick-reply templates for admins**: Pre-written responses for common scenarios (document rejected, payment reminder, survey scheduled) to speed up admin communication.
6. **Chatbot for FAQ**: Basic chatbot (WhatsApp or in-app) handling common questions: "Berapa biaya franchise?", "Dokumen apa saja yang diperlukan?", "Berapa lama proses pengajuan?"

---

## 8. Digital Signatures & Legal Compliance

### What Best-in-Class Looks Like

The franchise MoU and related agreements require legally valid signatures and proper stamp duty in Indonesia.

**Electronic signature types in Indonesia:**
- **Certified Electronic Signature (Tanda Tangan Elektronik Tersertifikasi)**: Created by an Indonesian-accredited PSrE (Electronic Certification Provider). Highest legal validity. Required for official documents, high-value contracts, government filings. Upheld by Indonesian Supreme Court (2023 ruling).
- **Uncertified Electronic Signature**: Created by non-accredited providers (e.g., DocuSign). Legally admissible but with lower evidentiary value. Acceptable for lower-stakes documents.

**Indonesian PSrE providers (certified digital signature):**
- PrivyID
- Vida
- Digisign
- Peruri Sign (government-backed)
- TekenAja
- Tilaka
- Xignature

**E-Materai (Electronic Stamp Duty):**
- Required on civil documents and transactions above Rp5,000,000 — clearly applicable to franchise agreements
- Issued exclusively by PERURI (state printing company)
- Rp10,000 per stamp
- Contains unique 22-digit serial number + QR code for verification via PERURI Scanner app
- API integration available for programmatic stamping (synchronous and asynchronous/bulk modes)
- Distributors: PT Peruri Digital Security, PT Finnet Indonesia, PT Mitra Pajakku, PT Mitracomm Ekasarana
- **Mekari Sign** offers an all-in-one platform combining digital signatures + e-Materai + document management with API integration

**Compliance requirements:**
- UU PDP (Law No. 27 of 2022): Personal Data Protection — governs how applicant data is collected, stored, processed, and shared
- UU ITE (Law No. 11 of 2008, amended): Electronic Information and Transactions — legal basis for electronic documents and signatures
- PP No. 71 of 2019: Electronic System and Transaction Implementation
- OJK regulations for financial due diligence (KYC/AML) if applicable

### Concrete Recommendations for MitraApply

1. **Integrate a certified PSrE** for MoU signing: PrivyID or Vida are the most established. Peruri Sign for government alignment.
2. **E-Materai integration** via Mekari Sign API or directly via Peruri Digital Security for stamping franchise agreements and payment receipts above Rp5M.
3. **Document signing workflow**: Generate MoU from template → Admin reviews and approves → E-Materai auto-affixed → Sent to applicant for certified digital signature → Countersigned by Indomaret → Both parties receive signed copy.
4. **Signed document vault**: All signed documents stored securely with verification metadata (signature certificate, e-Materai serial number, timestamp).
5. **PDP compliance**: Privacy policy, data processing consent, data retention policy, right to deletion — all built into the applicant onboarding flow.

---

## 9. Consolidated Feature Recommendations for MitraApply

### Priority 1 — Core MVP

| Feature | Description |
|---|---|
| Multi-step application form | Progressive disclosure, mobile-first, auto-save, Bahasa Indonesia |
| Document upload & OCR | Camera-first upload, KTP/NPWP OCR via Verihubs or GLAIR, per-document status tracking |
| Pipeline status tracker | 8-stage pipeline with visual progress bar (applicant-facing) and kanban board (admin-facing) |
| Admin review workflow | Role-based review, structured checklists, approve/reject/revise actions, mandatory comments |
| WhatsApp notifications | Stage transitions, action-required alerts, payment reminders via WhatsApp Business API |
| Virtual Account payments | Milestone-based payment tracker with auto-reconciliation via Midtrans or Xendit |
| Location submission | Google Maps pin-drop, photo upload, basic location details form |
| In-app messaging | Threaded conversations between applicant and admin |

### Priority 2 — Enhanced Experience

| Feature | Description |
|---|---|
| eKYC verification | Dukcapil database check, face matching, liveness detection |
| SLA tracking | Per-stage SLA with auto-escalation and dashboard |
| Lead scoring | Basic scoring based on completeness, responsiveness, financial qualification |
| Survey management | Surveyor mobile module, checklist-based assessment, feasibility scoring |
| Automated reminders | Configurable reminder sequences for incomplete actions |
| Digital signatures | Certified PSrE integration for MoU signing |
| E-Materai | PERURI integration for stamp duty on agreements |

### Priority 3 — Advanced / Future

| Feature | Description |
|---|---|
| Proximity analysis | Automated distance check to existing Indomaret stores |
| Demographic overlay | Population/income data overlay on location map |
| AI chatbot | FAQ bot for common franchise questions (WhatsApp + in-app) |
| Analytics dashboard | Conversion funnel, time-in-stage, regional performance, rejection analysis |
| Payment installment plans | Structured multi-milestone payment schedules with due dates |
| Bulk operations | Admin batch processing for document verification and approvals |
| IKD integration | Future-proofing for Indonesia's digital identity card |

### Technical Architecture Considerations

- **Web app (PWA)** for applicants — no app store download friction, works on low-end devices, offline-capable for document uploads
- **Responsive admin dashboard** — web-based, optimized for desktop but usable on tablet
- **API-first architecture** — separate frontend and backend, enabling future native mobile apps or third-party integrations
- **Cloud-hosted** — AWS (ap-southeast-1, Jakarta region available) or GCP (asia-southeast2, Jakarta) for low latency and data residency compliance
- **Key integrations**: eKYC provider (Verihubs/GLAIR), payment gateway (Midtrans/Xendit), WhatsApp Business API (Mekari Qontak/Twilio), digital signature (PrivyID/Vida), e-Materai (Mekari Sign/Peruri Digital Security), maps (Google Maps Platform)

---

## 10. Sources

### Franchise Application Systems & Best Practices
- Franchise Digital Workplace Priorities for 2026 — Claromentis
- Comparing the Best Franchise Management Software in 2025 — Claromentis
- 19 Must-Have Franchise Software Features — Claromentis
- Franchise Management Software Development Guide — Octal Software
- Franchise Lead Generation Best Practices — Franchise Insights
- UX Tactics That Lift Franchise Form Submissions in 2026 — 1851 Franchise

### Franchise CRM & Pipeline Management
- FranConnect — Engage Candidates in Franchise Sales Cycle
- FranConnect — 7 Features Your Franchise Dev Software Must Have
- FranConnect GO Launch
- FranchiseSoft — Franchise Sales CRM
- BrandWide — Franchise Management Platform
- ClientTether — Franchise Management Software
- FranFunnel — Franchise Sales Pipeline Software
- SeoSamba — Franchise CRM

### Document Verification & eKYC in Indonesia
- Verihubs — eKYC Verification
- Verihubs — OCR Extraction
- Verihubs — Top 5 KYC Software in Indonesia 2025
- GLAIR AI — eKYC
- Arya.ai — Indonesia KTP ID Extraction API
- uqudo — Indonesia KYC & AML Services
- eKYC in Indonesia: Ultimate Guide — Fintech Singapore
- Top 5 Document Verification Software in Indonesia — Fintelite
- Iluma AI — API Reference
- DataZoo — How to Verify an Indonesian Citizen

### Indonesia OSS & Business Permits
- OSS System in Indonesia — Cekindo
- OSS RBA Guide — Let's Move Indonesia
- OSS RBA 2025 — Bizindo
- OSS Key 2025 Changes — ILA Global Consulting

### Payment Integration in Indonesia
- Xendit — Payment Gateway Indonesia
- Xendit — Virtual Account Documentation
- Xendit — Payouts via API
- Midtrans — Payment Gateway
- DOKU — Payment Gateway
- Indonesia 2025 Ecommerce & Payments Trends — PaymentsCMI
- Indonesia Real-Time Payments (BI-FAST) — Lightspark
- Top 12 Payment Gateways in Indonesia — FinQfy

### Milestone & Escrow Payments
- Castler — Milestone Escrow
- Escrow.com — Milestone Transactions
- Fynk — Milestone Payment Schedule

### Admin Approval Workflows
- Atlassian — Approval Process Workflow
- Wrike — Understanding Approval Workflows
- Moxo — Multi-Level Approval Workflows Guide
- Kissflow — Multi-Level Approval Workflows
- Cflow — Parallel Pathways and Multi-Level Approvals

### GIS & Location Feasibility
- GrowthFactor — Geographic Information Systems for Franchise
- Esri — Retail Site Suitability & Selection
- GIS Navigator — GIS in Site Selection
- Tango Analytics — Site Selection Software
- Area Development — AI 101 for Site Selection

### UX & Form Design
- Progressive Disclosure — Interaction Design Foundation
- Progressive Disclosure — Nielsen Norman Group
- Multi-Step Form Best Practices — Webstacks
- Progressive Disclosure for Mobile Apps — UX Planet

### Digital Signatures & E-Materai in Indonesia
- eSignature Legality in Indonesia — Certinal
- Electronic Signature Laws Indonesia — Adobe
- Digital Signatures Legal in Indonesia — Vida
- Digital Signature on E-Stamp Duty in Indonesia — Double-M
- eSignature Indonesia — DocuSign
- Mekari Sign — Digital Signature & E-Materai Platform
- Mekari Sign — E-Materai API
- Peruri Digital Security — E-Materai

### WhatsApp Business API in Indonesia
- Mekari Qontak — WhatsApp Business API Indonesia
- WhatsApp Business Platform — Meta
- WhatsApp API Indonesia — ControlHippo
- WhatsApp Business API Use Cases — Sinch
- WhatsApp Business API Notifications — Clickatell

### Communication & In-App Messaging
- In-App Messaging — Airship
- In-App Messaging Best Practices — CleverTap
- In-App Messaging for Retention — Intercom
- In-App Notification Software — OneSignal
