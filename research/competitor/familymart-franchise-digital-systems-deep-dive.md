# FamilyMart Franchise Application & Management Digital Systems - Deep Dive

**Date:** 2026-02-26
**Purpose:** Detailed research on FamilyMart's franchise application processes, digital tools, and technology stack across key markets (Japan, Taiwan, Thailand) to benchmark against the EDTS Indomaret Franchise Application project.

---

## Table of Contents

1. [Japan Market (Headquarters)](#1-japan-market-headquarters)
2. [Taiwan Market](#2-taiwan-market)
3. [Thailand Market (Historical - Now Exited)](#3-thailand-market)
4. [Digital Tools for Franchisees](#4-digital-tools-for-franchisees)
5. [Application Flow - Step by Step](#5-application-flow---step-by-step)
6. [Technology Stack & Partnerships](#6-technology-stack--partnerships)
7. [Best Practices & Notable Features](#7-best-practices--notable-features)
8. [Implications for EDTS Indomaret Project](#8-implications-for-edts-indomaret-project)

---

## 1. Japan Market (Headquarters)

### Overview

FamilyMart operates approximately 16,000+ stores across Japan, the vast majority of which are franchise-operated. As a subsidiary of ITOCHU Corporation (a major Japanese trading company), FamilyMart leverages significant corporate resources for digital infrastructure.

### Franchise Recruitment Portal

- **Official URL:** [https://www.family.co.jp/company/fc.html](https://www.family.co.jp/company/fc.html)
- **Support Details:** [https://www.family.co.jp/company/fc/support/system.html](https://www.family.co.jp/company/fc/support/system.html)
- The recruitment site provides information on franchise types, financial requirements, support systems, and an online inquiry/application form.
- FamilyMart promotes "owner support" online, highlighting support for first-time business owners and a low barrier to entry (as little as 1.5 million yen / ~US$9,500).

### 2024 Franchise Agreement Reforms

In June 2024, FamilyMart made a significant policy change: they now allow people **with no store experience** to open new stores as **sole franchise owners** (the "hitori kamei seido" / single-owner system). Previously, a married couple or parent-child pair was required. This was driven by Japan's declining population and increasing single-person households.

Key terms:
- **Minimum investment:** 1.5 million yen (~US$9,500) at contract time
- **Guaranteed gross income:** 20 million yen (~US$127,400/year) for 24/7 stores
- **Utility support:** HQ pays 90% of actual utility costs (up to 3.6 million yen/year)
- **Waste loss sharing:** HQ bears 10-25% of waste loss costs
- **Eligible age:** 18-54 years old for single-owner system

### Digitized Operations

FamilyMart Japan has digitized core franchise operations:
- **Product ordering** is handled through digital systems (not paper-based)
- **Staff shift management** is conducted via digital tools
- **Supervisor visits** are supplemented by real-time data sharing platforms
- **Daily management reports** are delivered digitally for store performance monitoring

---

## 2. Taiwan Market

### Overview

FamilyMart Taiwan is one of the largest convenience store chains in Taiwan with **4,100+ stores** and is a major digital innovation leader within the FamilyMart network.

### Digital Transformation Timeline

| Year | Milestone |
|------|-----------|
| **1988** | First FamilyMart store opens in Taiwan |
| **2016** | Digital transformation begins - membership system digitized, mobile app launched |
| **2018** | Concept store launched with Fujitsu - robots, blockchain, IoT, 3D cameras |
| **2021** | Google Cloud partnership begins for data analytics |
| **2024** | App reaches 18 million members; Vertex AI Search deployed |
| **2025** | AnyMind Group partnership for retail media ecosystem; plans to add 150 stores |

### FC Academy (Franchise Training)

- FamilyMart Taiwan invests **NT$10 million annually** into franchisee training
- **FC Academy** provides structured courses on store management
- Multiple-unit franchising is supported for experienced franchisees to expand
- Training covers store manager responsibilities, register operations, planning/execution/verification cycles

### Smart Inventory System

FamilyMart Taiwan introduced a **smart inventory system** deployed across all domestic stores:
- Allows users to set **10 parameters** to calculate recommended product order quantities
- Improves inventory management efficiency significantly
- Reduces human error in ordering decisions

### Franchise Information Portal

- **URL:** [https://www.family.com.tw/web_enterprise/page/advantage_en.aspx](https://www.family.com.tw/web_enterprise/page/advantage_en.aspx)
- Provides information on management advantages, franchise support, and training programs
- The corporate office provides extensive support for store managers after opening

### Consumer-Facing App (Relevant for Understanding Digital Maturity)

- **Google Play:** [FamilyMart Taiwan App](https://play.google.com/store/apps/details?id=grasea.familife)
- Features: Per-order service, payment, package tracking, social media, e-commerce
- **18 million members** as of October 2024 (in a country of ~24 million people)
- Integrated loyalty program and digital coupons

---

## 3. Thailand Market

### Important Note: FamilyMart Has Exited Thailand

- FamilyMart opened its first Thailand store in **1993** as a joint venture with Central Group.
- In **May 2020**, Central Retail Corporation (CRC) acquired the remaining 49% stake, becoming sole owner.
- The **franchise rights expired at the end of 2023**, and FamilyMart exited Thailand.
- All former FamilyMart stores have been **rebranded as "Tops Daily"** under Central's own brand.

### Historical Franchise Process (Pre-Exit)

Before exiting, the Thailand franchise operated as follows:
- **Investment:** Starting from ~440,000 baht
- **Guaranteed income:** 30,000 baht/month for one year (if targets met)
- **Contract duration:** 6 years
- **Application:** Primarily phone/email-based (02-836-5884-8 or [email protected])
- **No own location required** - Central provided store locations
- Training by professionals with ongoing consultation throughout contract

### Relevance for Benchmarking

The Thailand market is less relevant for digital benchmarking since FamilyMart no longer operates there. However, Central Group's decision to rebrand to Tops Daily and integrate operations into their own digital ecosystem is noteworthy - it demonstrates the importance of **owning your own technology stack** rather than depending on a foreign licensor's systems.

---

## 4. Digital Tools for Franchisees

### 4.1 POS System

| Component | Details |
|-----------|---------|
| **Hardware** | Fujitsu TeamPoS 2000M terminals |
| **POS Software** | StoreNext ISS45 (open-platform POS) |
| **Mobile Companion** | PocketOffice mobile app on iPad |
| **History** | POS introduced Oct 1997; rolled out to all stores by Sept 1998 |
| **Integration** | POS data filtered to headquarters for real-time analysis |

### 4.2 Store Management Suite (StoreNext Connected Services)

FamilyMart uses **StoreNext Connected Services**, a suite of internet-based management applications:
- **Connected Store Analysis & Reporting** - daily management reports
- **Connected Item Hosting** - product catalog and pricing management
- **Connected Direct Store Delivery (DSD)** - supplier delivery management
- Critical for providing franchisees with **daily operational insights**

### 4.3 Self-Checkout & Cashier-less Technology

- **TOUCH TO GO partnership:** Opened Japan's first stand-alone cashier-less convenience store near Tokyo Station
- **Technology:** 40+ ceiling cameras with shelf sensors (similar to Amazon Go)
- **Recognition rate:** 95% accuracy for automatic item identification
- **No app required** - customers check out by walking through a payment zone
- Costs are automatically calculated

### 4.4 Digital Signage Network (FamilyMartVision)

FamilyMart has deployed one of the largest retail media networks in Japan:
- **10,000 stores** equipped with digital signage across all 47 prefectures
- Operated by **Gate One Co., Ltd.** (subsidiary established with ITOCHU in Sept 2021)
- **AI camera-based visibility analysis** launched March 2024
- **Segmented distribution plans** by region and store characteristics
- 60% of advertisers come from outside FamilyMart's supplier network
- Generates significant revenue as a retail media business

### 4.5 In-Store Kiosks (Taiwan)

- **Wincor Nixdorf** kiosks installed in 90%+ of Taiwan stores (2,000+ kiosks)
- Services: Bill payment, specialty food ordering, loyalty point management
- FamiPort multi-function kiosk is a core service touchpoint

### 4.6 Mobile Applications

**MY FamilyMart App (Malaysia market example):**
- Famideals vouchers
- Famipoints rewards collection
- Famidelivery service (30-minute delivery)
- Available on [Google Play](https://play.google.com/store/apps/details?id=com.maxincome.fmcrm)

**FamilyMart Taiwan App:**
- Per-order, payment, package tracking
- Social media integration
- E-commerce services
- 18 million members

---

## 5. Application Flow - Step by Step

### Japan Market (Reconstructed from Multiple Sources)

| Step | Activity | Channel |
|------|----------|---------|
| 1 | **Online Inquiry** | Web form on [family.co.jp/company/fc.html](https://www.family.co.jp/company/fc.html) |
| 2 | **Initial Consultation** | Recruitment staff contacts applicant; provides detailed business explanation |
| 3 | **Information Session / Briefing** | In-person or virtual; covers franchise types, financials, support systems |
| 4 | **Application Submission** | Formal application with financial documentation (min 1.5M yen proof of funds) |
| 5 | **Interview & Assessment** | Meeting with franchise management; evaluation of suitability |
| 6 | **Store Assignment / Location Selection** | HQ identifies available store locations; applicant reviews |
| 7 | **Contract Signing** | Franchise agreement execution |
| 8 | **Pre-Opening Training** | Store manager training at FamilyMart training facilities covering register operations, basic procedures, management responsibilities |
| 9 | **Training at Directly Managed Store** | Hands-on training at an existing corporate-run store |
| 10 | **Store Preparation & Setup** | Store fit-out, inventory loading, systems setup |
| 11 | **Grand Opening** | Store opens with HQ trainer support on-site |
| 12 | **Post-Opening Support** | Regular supervisor visits, ongoing training, digital reporting access |

### Philippines Market (Documented Process - Indicative of Regional Approach)

1. Submission of required documents
2. Franchise orientation attendance
3. Application form completion
4. Interview with FamilyMart franchise manager
5. 1-day on-the-job evaluation
6. 1-month training program (operations and environment exposure)
7. Store opening

---

## 6. Technology Stack & Partnerships

### Core Technology Partners

| Partner | Role | Details |
|---------|------|---------|
| **Fujitsu** | POS, Store Systems, Smart Store | TeamPoS 2000M hardware; digital concept store in Taiwan (2018); IoT/blockchain field trials |
| **Google Cloud** | Cloud Infrastructure, AI/ML | BigQuery for data warehouse; Vertex AI Search for retail; Google Analytics integration |
| **Dynacloud** | Google Cloud Implementation | Technical partner for Vertex AI Search and BigQuery implementation |
| **ITOCHU Corporation** | Parent Company, Retail Media | Co-established Gate One for FamilyMartVision digital signage |
| **Gate One** | Retail Media Network | Operates FamilyMartVision across 10,000 stores; AI camera analytics |
| **Panasonic** | Smart Store Technology | Next-gen store demo at Saedo Store, Yokohama (2019); IoT sensors, AI restocking |
| **TOUCH TO GO** | Cashier-less Technology | Camera/sensor-based checkout; 95% recognition rate |
| **StoreNext (Fujitsu subsidiary)** | Store Management Software | ISS45 POS, Connected Services suite, PocketOffice mobile |
| **Wincor Nixdorf** | In-Store Kiosks | 2,000+ multi-function kiosks in Taiwan stores |
| **AnyMind Group** | Retail Media (Taiwan) | Partnership announced Sept 2025 for retail media ecosystem development |
| **Innolux** | Smart Display Technology | Partnered for smart store displays in Taiwan |

### Cloud & Data Architecture (Taiwan, Applicable to Broader Network)

```
[Consumer App (18M users)]
        |
        v
[Google Analytics] --> [BigQuery Data Warehouse]
        |                       |
        v                       v
[Vertex AI Search]      [Business Analytics]
        |                       |
        v                       v
[Personalized Search    [Real-time Product
 Results in App]         Recommendations]
```

Key technical details:
- **BigQuery** dramatically shortened data query times vs. previous parallel computing architecture
- **Vertex AI Search for retail** selected as providing the most relevant search results
- Real-time recommendation generation improved from 2-3 minutes to near-instant
- Data security compliance ensured via Google Cloud's Taiwan data center

---

## 7. Best Practices & Notable Features

### 7.1 Low Barrier to Entry with Strong Digital Support

FamilyMart's 2024 reform allowing solo owners with no experience (and only ~US$9,500 investment) is supported by comprehensive digital tools that reduce the operational complexity for new franchisees. Digitized ordering, shift management, and daily reporting mean new owners can focus on customer service rather than manual administrative tasks.

### 7.2 Integrated Retail Media as Revenue Stream

The FamilyMartVision network (10,000 stores) demonstrates how a franchise network's physical footprint can be monetized digitally. Gate One's profitability shows this is not just a cost center but a genuine revenue stream, with 60% of advertisers being external to FamilyMart's supply chain.

### 7.3 Data-Driven Franchise Operations

- **Smart inventory system** with 10 configurable parameters replaces guesswork in ordering
- **BigQuery analytics** enable real-time business intelligence for franchisees
- **AI-powered recommendations** personalize the consumer experience, driving sales
- **Supervisor visits** are supplemented (not replaced) by digital reporting tools

### 7.4 Omnichannel Ecosystem

FamilyMart Taiwan's approach of blending e-commerce with physical stores (click-and-collect across 4,100 stores) creates a unified ecosystem where franchise stores serve as both retail outlets and logistics nodes. This increases store traffic and revenue potential for franchisees.

### 7.5 Technology-Forward Concept Stores as Innovation Labs

FamilyMart uses concept stores (like the Taiwan Fujitsu collaboration) to test emerging technologies before broader rollout:
- Robots for customer guidance
- 3D cameras for visitor counting
- Electronic price tags with automatic stock updates
- IoT, RFID, NFC, and blockchain integration
- This "test and scale" approach reduces risk for franchisees

### 7.6 Comprehensive Training Infrastructure

The FC Academy model with NT$10M annual investment in Taiwan demonstrates commitment to franchisee capability building. The SQ&C (Service, Quality, Cleanliness) standard with proficiency-level-based training ensures consistent service across thousands of franchise locations.

---

## 8. Implications for EDTS Indomaret Project

### Key Lessons from FamilyMart

| FamilyMart Practice | Relevance to Indomaret Project |
|---------------------|-------------------------------|
| Online franchise inquiry form leading to structured consultation flow | Core requirement - web-based application portal |
| Digitized ordering and shift management for franchisees | Post-opening tools should be considered in the platform roadmap |
| Smart inventory with configurable parameters | Could inform Indomaret's value proposition to prospective franchisees |
| FC Academy structured training with certification | Training management could be integrated into franchise application workflow |
| Google Cloud + BigQuery for analytics | Technology stack consideration for Indomaret's platform |
| Low barrier to entry (solo owner, no experience needed) | Application process should accommodate varying applicant profiles |
| Supervisor + digital reporting hybrid model | Post-opening support tracking in franchise management system |
| Retail media network monetization | Long-term digital revenue opportunity for franchise network |

### Gaps to Note

1. **FamilyMart does NOT appear to have a fully digital, end-to-end online franchise application system** - their process still relies on web inquiry forms followed by phone/in-person consultations
2. **No evidence of a dedicated franchisee self-service portal** for application status tracking (unlike what modern SaaS franchise platforms offer)
3. **Thailand exit** shows risks of not owning your own digital infrastructure - Central Group chose to rebrand and build their own systems
4. **Application process is still semi-manual** - the digital focus has been more on store operations than on franchise recruitment/onboarding

This represents an **opportunity for Indomaret/EDTS**: building a fully digital franchise application and management platform would be more advanced than what FamilyMart currently offers publicly.

---

## Sources

- [FamilyMart Franchise Recruitment (Japan)](https://www.family.co.jp/company/fc.html)
- [FamilyMart Franchise Support System (Japan)](https://www.family.co.jp/company/fc/support/system.html)
- [FamilyMart Google Cloud Case Study](https://cloud.google.com/customers/familymart)
- [FamilyMart Taiwan Management Advantages](https://www.family.com.tw/web_enterprise/page/advantage_en.aspx)
- [FamilyMart Wikipedia](https://en.wikipedia.org/wiki/FamilyMart)
- [FamilyMart Retail Media Strategy - Martner Japan](https://www.martnerjapan.com/2024/11/04/success-in-familymarts-retail-media-strategy-digital-signage-advertising-expands/)
- [ITOCHU - FamilyMart In-Store Media Business](https://www.itochu.co.jp/en/business/8th/project/04.html)
- [FamilyMart Digital Signage - Japan Today](https://japantoday.com/category/tech/coming-soon-to-japan%E2%80%99s-familymart-convenience-stores-a-whole-lot-of-digital-signage)
- [FamilyMart Digitalization & Sustainability](https://www.family.co.jp/english/sustainability/material_issues/needs/digital.html)
- [FamilyMart Cashier-less Store - TimeOut Tokyo](https://www.timeout.com/tokyo/news/familymart-opens-a-self-checkout-convenience-store-with-no-cashier-040621)
- [FamilyMart Cashier-less Store - Japan Insider](https://japaninsider.com/familymart-opens-first-stand-alone-cashier-less-convenience-store/)
- [FamilyMart x Panasonic Next-Gen Store](https://news.panasonic.com/global/topics/5264)
- [Fujitsu x Taiwan FamilyMart Concept Store](https://www.fujitsu.com/global/about/resources/news/press-releases/2018/0329-03.html)
- [FamilyMart Taiwan Smart Store - Taipei Times](https://www.taipeitimes.com/News/biz/archives/2018/04/02/2003690497)
- [FamilyMart Taiwan Hybrid Retail - The Drum](https://www.thedrum.com/insight/2022/02/23/taiwan-s-familymart-why-hybrid-model-the-future-retail-the-post-pandemic-era)
- [FamilyMart Taiwan x AnyMind Retail Media](https://anymindgroup.com/news/press-release/familymart-taiwan-2025)
- [FamilyMart Taiwan 150 New Stores 2025](https://www.mobilityplaza.org/news/41296)
- [Wincor Nixdorf Kiosks at Taiwan FamilyMart](https://ixtenso.com/technology/wincor-nixdorf-breaks-the-2-000-mark-for-installed-kiosks-at-taiwan-familymart.html)
- [FamilyMart Franchisee Sustainability](https://www.family.co.jp/english/sustainability/joint_growth.html)
- [Demography & FamilyMart Franchise Recruitment](https://realgaijin.substack.com/p/demography-bites-familymart-desperately)
- [FamilyMart Thailand Franchise - Techsauce](https://techsauce.co/en/pr-news/familymart-welcomes-new-franchises)
- [FamilyMart Thailand Exit - Inside Retail Asia](https://insideretail.asia/2023/08/13/familymart-brand-to-withdraw-from-thailand/)
- [FamilyMart Thailand Rebrand - Nation Thailand](https://www.nationthailand.com/business/trading-investment/40030050)
- [FAMIMA US Technology - CSP Daily News](https://www.cspdailynews.com/technologyservices/famima-techs)
- [StoreNext Store Systems - Supermarket News](https://www.supermarketnews.com/supplier-news/expanding-famima-chain-selects-storenext-store-systems)
- [FamilyMart Philippines Franchise](https://www.franchisemarket.ph/franchise-listing/familymart)
- [FamilyMart Beyond Convenience - The Worldfolio](https://www.theworldfolio.com/interviews/where-every-stop-feels-like-mini-win/7207/)
- [FamilyMart Digital Technology Impact Study - Springer](https://link.springer.com/chapter/10.1007/978-981-97-3698-0_29)
