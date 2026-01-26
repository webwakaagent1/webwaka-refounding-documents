# Founder Decision Questions (Pre-Implementation Lock) v2

**Document Version:** 2.0 (Revised with Founder Clarifications)  
**Date:** 2026-01-26  
**Author:** Manus AI  
**Purpose:** Final decision gate before WebWaka Platform Re-Founding execution begins

---

## 🔒 FOUNDATIONAL CONSTRAINTS (ALREADY DECIDED)

The following are **canonically locked** and are NOT re-asked in this document:

### 1. AWS-First, Single-Bill Strategy
- Prefer AWS-native services
- Third-party tools only if strictly necessary
- Manus will have AWS access for setup and configuration

### 2. Design for Maximum Scale Upfront
- WebWaka = Platform for Building Platforms (meta-platform)
- Architecture designed for full global scale (1,000+ Partners, 1,000,000+ Tenants)
- Implementation may be phased, architecture may NOT be phased

---

## 🧩 CRITICAL CLARIFICATIONS (LOCKED)

The following clarifications are **canonically locked** and fundamentally shape this document:

### Clarification 1: Configurable Multi-Level Affiliate Logic

The Multi-Level Affiliate System must be **fully configurable at runtime**, not hardcoded.

**This means:**
- Commission rules, eligibility, and behavior MUST be configurable:
  - Per individual partner
  - Per group of partners
  - Per contract
  - Per use case
- The system MUST NOT assume one global affiliate configuration
- The architecture must support different partners having different:
  - Commission percentages
  - Level depths
  - Qualification rules
  - Payout timing
  - Revenue sources

### Clarification 2: Level Depth Up to 10 (Hard Limit)

The affiliate hierarchy MUST support **up to 10 levels**.

**Important nuances:**
- This does NOT mean all partners must use 10 levels
- It means the system must be **capable** of supporting up to 10 levels
- Level depth must be **configurable per contract / per org / per use case**

**Architectural implications:**
- Data models must support variable depth
- Computation must be efficient and safe at max depth
- Payout logic must not assume shallow trees

### Clarification 3: Affiliate System as Platform Primitive (Recursive Usage)

The Affiliate System is **NOT a "Partner-only" feature**. It is a **PLATFORM-WIDE PRIMITIVE**, just like CRM, Automation, Site Builder, Messaging, and Identity.

**Recursive Usage Model (MANDATORY):**

Every system in WebWaka — including the Affiliate System — must be usable **recursively across levels**:

| Level | Can Use the System For |
|-------|------------------------|
| Super Admin | Partners |
| Partner | Their own clients |
| Client | Their own users / affiliates |
| Sub-Client | Same pattern |

**This implies:**
- The system must be **tenant-agnostic**
- It must support **self-referential usage**
- It must not assume **"top-down only" control**

**Key Mental Model:**

> Anything WebWaka uses internally must be something Partners can use for their Clients, and Clients can use for their own ecosystems.

This rule applies to:
- Affiliate system
- CRM
- Automation
- Site builder
- Messaging
- Reporting
- Billing primitives

### Clarification 4: Partner-Controlled Pricing (Non-Negotiable)

Partners must be able to **set their own pricing** for products and services offered to their clients.

**This is not optional and not limited to discounts or markups.**

**This means:**
- Partners can:
  - Define their own prices
  - Define pricing tiers
  - Bundle services differently
  - Offer custom plans to different clients
- Super Admin may define:
  - Base products
  - Cost floors
  - Revenue share rules
  - Platform fees
- …but must NOT enforce a single retail price across all partners

**Critical Architectural Implication:**

Pricing must be:
- **Decoupled from product definition**
- **Scoped by tenant** (partner → client)
- **Compatible with:**
  - Affiliate commissions
  - Revenue splits
  - Taxes
  - Invoicing

**Any design that assumes centralized pricing only is invalid.**

---

## 📋 DECISION SUMMARY

This document contains **15 critical decisions** across 8 categories:

| Category | Decisions |
|----------|-----------|
| **1. Multi-Level Affiliate / Partner System** | 4 decisions |
| **2. Industry-Specific Modules** | 1 decision |
| **3. White-Label & Reseller Model** | 2 decisions |
| **4. Billing & Monetization Architecture** | 3 decisions |
| **5. Identity, Tenancy & Organization Model** | 1 decision |
| **6. Data Ownership & Portability** | 1 decision |
| **7. Governance & Control** | 2 decisions |
| **8. GoHighLevel Parity & Beyond** | 1 decision |

---

## CATEGORY 1: MULTI-LEVEL AFFILIATE / PARTNER SYSTEM

---

### Decision #1 — Affiliate Hierarchy Data Model

**Category:** Partnerships / Affiliate System / Architecture

**Why This Decision Is Required Now:**

The data model must support variable-depth affiliate hierarchies (up to 10 levels) from day one. The choice between adjacency list, nested set, or closure table patterns affects query performance, write complexity, and AWS Aurora costs.

**Decision Question:**

Which data model pattern should WebWaka use to support variable-depth affiliate hierarchies (up to 10 levels)?

**Recommended Answer (Manus Proposal):**

**Option A: Closure Table Pattern**

- Store all ancestor-descendant relationships in a separate `affiliate_paths` table
- Each row represents a path from ancestor to descendant with depth
- Example:
  ```
  ancestor_id | descendant_id | depth
  ------------|---------------|------
  1           | 1             | 0
  1           | 2             | 1
  1           | 3             | 2
  2           | 2             | 0
  2           | 3             | 1
  3           | 3             | 0
  ```

**Justification:**

- **Efficient queries:** Finding all descendants or ancestors is a simple `JOIN` (no recursion required)
- **Variable depth support:** Supports up to 10 levels without schema changes
- **Configurable per partner:** Each partner can have different depth limits (enforced via application logic)
- **AWS-first alignment:** Simple `JOIN` queries reduce AWS Aurora query costs compared to recursive CTEs
- **Proven at scale:** Used by Salesforce, LinkedIn, and other multi-level hierarchy systems

**Alternative Options:**

- **Option B: Adjacency List** — Store only `parent_id` in each affiliate record. Requires recursive queries to find all descendants. Slower for deep hierarchies (up to 10 levels). Not recommended.
- **Option C: Nested Set** — Store `left` and `right` values for each affiliate. Fast reads, but slow writes (requires updating all descendants on insert). Not suitable for high-write affiliate systems.

**Impact of Founder Decision:**

- **If Option A (Closure Table):** Data model uses `affiliate_paths` table. Queries are fast and simple. Supports variable depth up to 10 levels. Implementation requires maintaining `affiliate_paths` table on affiliate creation/deletion.
- **If Option B (Adjacency List):** Data model uses `parent_id` column. Queries require recursive CTEs. Slower for deep hierarchies. Simpler writes.
- **If Option C (Nested Set):** Data model uses `left` and `right` columns. Fast reads, but slow writes. Not recommended for affiliate systems.

---

### Decision #2 — Affiliate Configuration Authority Hierarchy

**Category:** Partnerships / Affiliate System / Governance

**Why This Decision Is Required Now:**

Per Clarification 1, affiliate logic must be configurable per partner, per contract, per use case. The configuration authority hierarchy determines who can override affiliate settings and how conflicts are resolved.

**Decision Question:**

How should affiliate configuration authority be scoped and overridden across the hierarchy (Global → Partner → Contract → Org)?

**Recommended Answer (Manus Proposal):**

**Option A: Hierarchical Override Model (Global → Partner → Contract → Org)**

- **Global (Super Admin):** Defines default affiliate configuration (commission percentages, level depth limit, payout timing, qualification rules)
- **Partner:** Can override global defaults for their own affiliate tree
- **Contract:** Can override partner defaults for specific contracts (e.g., custom commission percentages for high-value partners)
- **Org (Client):** Can override contract defaults for their own sub-affiliates (if recursive usage is enabled)

**Conflict Resolution:** Most specific configuration wins (Org > Contract > Partner > Global)

**Example:**
- **Global:** 5% commission, 4-level depth
- **Partner A:** Overrides to 7% commission, 6-level depth
- **Contract B (under Partner A):** Overrides to 10% commission, 8-level depth
- **Org C (under Contract B):** Overrides to 12% commission, 10-level depth

**Justification:**

- **Aligns with Clarification 1:** Supports configurable affiliate logic per partner, per contract, per use case
- **Aligns with Clarification 3:** Supports recursive usage (Org can configure their own sub-affiliates)
- **Flexible:** Partners can customize affiliate logic for different contracts and clients
- **Predictable:** Clear conflict resolution rules (most specific wins)
- **AWS-first alignment:** Configuration stored in AWS Aurora. No external configuration service required.

**Alternative Options:**

- **Option B: Flat Configuration (Partner-Only)** — Only Partners can configure affiliate logic. No contract-level or org-level overrides. Less flexible. Does not support recursive usage.
- **Option C: Global-Only Configuration** — Only Super Admin can configure affiliate logic. No partner-level overrides. Does not align with Clarification 1.

**Impact of Founder Decision:**

- **If Option A (Hierarchical Override):** Configuration authority is scoped across 4 levels (Global → Partner → Contract → Org). Implementation requires configuration inheritance and conflict resolution logic.
- **If Option B (Flat Partner-Only):** Only Partners can configure affiliate logic. Simpler implementation, but less flexible.
- **If Option C (Global-Only):** Only Super Admin can configure affiliate logic. Does not align with Clarification 1.

---

### Decision #3 — Affiliate Commission Calculation Model

**Category:** Partnerships / Affiliate System / Billing

**Why This Decision Is Required Now:**

Per Clarification 1, commission percentages must be configurable per partner, per contract, per use case. The commission calculation model determines how commissions are calculated when multiple levels have different percentages.

**Decision Question:**

Should affiliate commissions be calculated as **fixed percentages of subscription revenue** or **cascading percentages** (each level takes a percentage of the remaining revenue)?

**Recommended Answer (Manus Proposal):**

**Option A: Fixed Percentages of Subscription Revenue**

- Each level receives a **fixed percentage of the Merchant's subscription revenue**
- Example (4-level hierarchy):
  - Merchant pays $100/month
  - Agent (Level 3) receives 20% of $100 = $20
  - SubPartner (Level 2) receives 15% of $100 = $15
  - Partner (Level 1) receives 5% of $100 = $5
  - Platform receives 60% of $100 = $60
- **Total commissions:** 20% + 15% + 5% = 40%
- **Platform revenue:** 60%

**Justification:**

- **Aligns with GoHighLevel model:** GoHighLevel uses fixed percentages (40% to Agency, 60% to Platform)
- **Simpler calculation:** Each level's commission is independent of other levels
- **Predictable:** Partners know upfront what they will earn
- **Configurable:** Each partner can have different commission percentages (via hierarchical override model)
- **AWS-first alignment:** Simple percentage calculations reduce AWS Lambda execution costs

**Alternative Options:**

- **Option B: Cascading Percentages** — Each level takes a percentage of the remaining revenue after upstream levels are paid. Example: Agent takes 20% of $100 = $20. SubPartner takes 15% of $80 = $12. Partner takes 5% of $68 = $3.40. Platform receives $64.60. More complex calculation. Less predictable.

**Impact of Founder Decision:**

- **If Option A (Fixed Percentages):** Each level receives a fixed percentage of subscription revenue. Simpler calculation. Predictable earnings. Implementation requires validating that total commissions do not exceed 100%.
- **If Option B (Cascading Percentages):** Each level takes a percentage of remaining revenue. More complex calculation. Less predictable earnings.

---

### Decision #4 — Affiliate Payout Responsibility

**Category:** Partnerships / Affiliate System / Billing

**Why This Decision Is Required Now:**

The payout responsibility determines who handles the financial transactions to pay out commissions. This affects the billing system, compliance requirements, and Partner experience.

**Decision Question:**

Should WebWaka handle all affiliate payouts centrally (Platform-Managed Payouts), or should Partners handle payouts to their own sub-affiliates (Partner-Managed Payouts)?

**Recommended Answer (Manus Proposal):**

**Option A: Platform-Managed Payouts (WebWaka Pays Everyone)**

- WebWaka collects subscription revenue from all Merchants
- WebWaka calculates commissions for all levels (Partners, SubPartners, Agents, etc.)
- WebWaka pays out commissions directly to all levels via Stripe Connect or Paystack

**Justification:**

- **Aligns with GoHighLevel model:** GoHighLevel handles all payouts centrally
- **Simpler for Partners:** Partners do not need to manage payout logistics, tax compliance, or banking integrations
- **Centralized compliance:** WebWaka handles all tax reporting, KYC/AML, and regulatory compliance
- **Better transparency:** All levels can view their earnings and payout history in the Partner Portal
- **AWS-first alignment:** Centralized payout logic can be implemented as a single AWS Lambda function triggered monthly

**Alternative Options:**

- **Option B: Partner-Managed Payouts** — Partners collect revenue from their Merchants and pay out commissions to their sub-affiliates. WebWaka only pays Partners. Higher operational overhead for Partners.

**Impact of Founder Decision:**

- **If Option A (Platform-Managed):** WebWaka integrates with Stripe Connect or Paystack to handle all payouts. Billing system calculates and disburses commissions monthly. Implementation requires payment gateway integration.
- **If Option B (Partner-Managed):** Partners handle payouts themselves. WebWaka only pays Partners. Implementation is simpler for WebWaka, but higher operational overhead for Partners.

---

## CATEGORY 2: INDUSTRY-SPECIFIC MODULES

---

### Decision #5 — Module Creation Authority

**Category:** Industry Modules / Marketplace / Governance

**Why This Decision Is Required Now:**

Per Clarification 3, all systems (including industry modules) must be usable recursively across levels. The module creation authority determines who can create new industry-specific modules and whether Partners can extend the platform.

**Decision Question:**

Should only WebWaka be able to create industry-specific modules (Platform-Only), or should Partners also be able to create and publish modules (Partner-Extensible)?

**Recommended Answer (Manus Proposal):**

**Option A: Platform-Only (WebWaka Creates All Modules) in Phase 1, Partner-Extensible in Phase 2+**

- **Phase 1:** Only WebWaka can create industry-specific modules (Commerce, Education, Health, Civic, Hospitality, Logistics, Professional Services)
- **Phase 2+:** Partners can create and publish custom modules to a marketplace
- **Recursive Usage:** Partners can use WebWaka-created modules for their clients. Clients can use Partner-created modules for their users.

**Justification:**

- **Aligns with Clarification 3:** Supports recursive usage (Partners can create modules for their clients)
- **Phased implementation:** Platform-Only in Phase 1 allows WebWaka to launch faster. Partner-Extensible in Phase 2+ adds extensibility.
- **Quality control:** WebWaka ensures all Phase 1 modules meet quality, security, and performance standards. Phase 2+ marketplace includes approval workflow.
- **Competitive differentiation:** Partner-extensible modules are a key selling point vs. GoHighLevel (which does not allow Agencies to create modules).
- **AWS-first alignment:** Phase 1 modules are built on AWS services. Phase 2+ marketplace requires module SDK and approval workflow.

**Alternative Options:**

- **Option B: Platform-Only (Permanent)** — Only WebWaka can create modules. Partners cannot extend the platform. Simpler implementation, but less flexible.
- **Option C: Partner-Extensible (Day One)** — Partners can create modules from day one. Requires module SDK and marketplace infrastructure. Higher implementation complexity. Slower time-to-market.

**Impact of Founder Decision:**

- **If Option A (Platform-Only Phase 1, Partner-Extensible Phase 2+):** WebWaka builds all modules in Phase 1. Partner-extensible marketplace added in Phase 2+. Implementation is phased.
- **If Option B (Platform-Only Permanent):** Only WebWaka can create modules. No marketplace infrastructure required. Simpler implementation, but less flexible.
- **If Option C (Partner-Extensible Day One):** Partners can create modules from day one. Higher implementation complexity. Slower time-to-market.

---

## CATEGORY 3: WHITE-LABEL & RESELLER MODEL

---

### Decision #6 — White-Label Branding Depth

**Category:** White-Label / Reseller

**Why This Decision Is Required Now:**

The white-label branding depth determines how deeply Partners can customize the platform's branding. This affects the Partner experience, platform architecture, and implementation complexity.

**Decision Question:**

Should Partners be able to white-label only the frontend (UI/UX), or should they also be able to white-label the backend (API, emails, SMS, webhooks)?

**Recommended Answer (Manus Proposal):**

**Option A: Full White-Label (Frontend + Backend)**

- Partners can customize:
  - **Frontend:** Logo, colors, fonts, domain (e.g., `app.partner.com`)
  - **Backend:** Email sender name/address, SMS sender ID, webhook URLs, API documentation branding

**Justification:**

- **Aligns with GoHighLevel model:** GoHighLevel allows full white-labeling (frontend + backend)
- **True SaaS resale:** Partners can present WebWaka as their own platform. End users never see "WebWaka" branding.
- **Higher Partner value:** Partners can charge premium prices for a fully branded platform.
- **Competitive differentiation:** Full white-labeling is a key selling point vs. competitors.
- **AWS-first alignment:** AWS SES supports custom sender domains. AWS API Gateway supports custom domains.

**Alternative Options:**

- **Option B: Frontend-Only White-Label** — Partners can customize frontend branding (logo, colors, domain), but backend services (emails, SMS, webhooks) still show "WebWaka" branding. Simpler implementation, but less valuable to Partners.

**Impact of Founder Decision:**

- **If Option A (Full White-Label):** Partners can fully brand the platform. Implementation requires custom domain support for AWS SES (email), AWS SNS (SMS), and AWS API Gateway (webhooks). Higher implementation complexity.
- **If Option B (Frontend-Only):** Partners can only brand the frontend. Backend services still show "WebWaka" branding. Simpler implementation, but lower Partner value.

---

### Decision #7 — Partner Data Isolation Model

**Category:** White-Label / Reseller / Data

**Why This Decision Is Required Now:**

The data isolation model determines how Partner data is stored and accessed. This affects security, compliance, performance, and cost.

**Decision Question:**

Should all Partners share a single AWS Aurora database with row-level security (Shared Database), or should each Partner have a dedicated AWS Aurora database (Dedicated Database Per Partner)?

**Recommended Answer (Manus Proposal):**

**Option A: Shared Database with Row-Level Security**

- All Partners and Tenants share a single AWS Aurora database
- Row-level security enforces `partnerId` and `tenantId` scoping
- All queries include `WHERE partnerId = ? AND tenantId = ?` clauses

**Justification:**

- **Aligns with GoHighLevel model:** GoHighLevel uses a shared database with row-level security
- **Lower cost:** Single AWS Aurora cluster is cheaper than 1,000+ dedicated clusters
- **Simpler operations:** Single database to backup, monitor, and maintain
- **Proven at scale:** Shared database with row-level security is a proven multi-tenant pattern (Salesforce, Zendesk, Stripe)
- **AWS-first alignment:** AWS Aurora supports row-level security via PostgreSQL policies

**Alternative Options:**

- **Option B: Dedicated Database Per Partner** — Each Partner has a dedicated AWS Aurora database. Higher cost (1,000+ Aurora clusters). Higher operational complexity (1,000+ backups, 1,000+ monitoring dashboards). Better isolation, but not necessary for most Partners.

**Impact of Founder Decision:**

- **If Option A (Shared Database):** All Partners share a single AWS Aurora database. Row-level security enforces isolation. Lower cost and simpler operations. Implementation requires careful query design to prevent cross-tenant data leaks.
- **If Option B (Dedicated Database):** Each Partner has a dedicated AWS Aurora database. Higher cost and operational complexity. Better isolation, but overkill for most Partners.

---

## CATEGORY 4: BILLING & MONETIZATION ARCHITECTURE

---

### Decision #8 — Pricing Authority Hierarchy

**Category:** Billing / Monetization / Governance

**Why This Decision Is Required Now:**

Per Clarification 4, Partners must be able to set their own pricing for products and services offered to their clients. The pricing authority hierarchy determines who can set prices and how pricing is inherited or overridden downstream.

**Decision Question:**

How should pricing authority be scoped and overridden across the hierarchy (Global → Partner → Client)?

**Recommended Answer (Manus Proposal):**

**Option A: Hierarchical Pricing Model (Global → Partner → Client)**

- **Global (Super Admin):** Defines base products, cost floors, and platform fees
- **Partner:** Sets retail prices for their clients (can markup or bundle differently)
- **Client (Tenant):** Uses Partner's pricing (cannot override)

**Example:**
- **Global:** Base product "CRM Module" has cost floor of $10/month. Platform fee is 60%.
- **Partner A:** Sets retail price of $50/month for "CRM Module" for their clients.
- **Client B (under Partner A):** Pays $50/month to Partner A. Partner A keeps $20/month (40%). WebWaka receives $30/month (60%).

**Justification:**

- **Aligns with Clarification 4:** Partners can set their own pricing
- **Prevents race-to-bottom:** Cost floors ensure Partners cannot sell below platform costs
- **Flexible:** Partners can bundle products, create custom plans, and offer different pricing to different clients
- **Compatible with affiliate commissions:** Affiliate commissions are calculated as percentages of Partner's retail price (not platform cost floor)
- **AWS-first alignment:** Pricing stored in AWS Aurora. No external pricing service required.

**Alternative Options:**

- **Option B: Centralized Pricing (Super Admin Only)** — Only Super Admin can set prices. Partners cannot customize pricing. Does not align with Clarification 4.
- **Option C: Fully Delegated Pricing (No Cost Floors)** — Partners can set any price, even below platform costs. Risk of Partners selling at a loss.

**Impact of Founder Decision:**

- **If Option A (Hierarchical Pricing):** Partners can set retail prices. Super Admin defines cost floors and platform fees. Implementation requires pricing inheritance and validation logic.
- **If Option B (Centralized Pricing):** Only Super Admin can set prices. Does not align with Clarification 4.
- **If Option C (Fully Delegated Pricing):** Partners can set any price. Risk of Partners selling at a loss.

---

### Decision #9 — Billing Model (Centralized vs. Delegated)

**Category:** Billing / Monetization

**Why This Decision Is Required Now:**

The billing model determines who collects subscription revenue from Merchants. This affects the Partner experience, compliance requirements, and revenue flow.

**Decision Question:**

Should WebWaka collect all subscription revenue centrally and pay out commissions to Partners (Centralized Billing), or should Partners collect revenue from their Merchants and pay WebWaka a platform fee (Delegated Billing)?

**Recommended Answer (Manus Proposal):**

**Option A: Centralized Billing (WebWaka Collects All Revenue)**

- WebWaka collects subscription revenue from all Merchants via Stripe or Paystack
- WebWaka calculates and pays out commissions to Partners, SubPartners, and Agents monthly
- Partners view their earnings and payout history in the Partner Portal

**Justification:**

- **Aligns with GoHighLevel model:** GoHighLevel uses centralized billing
- **Simpler for Partners:** Partners do not need to manage billing, invoicing, or payment processing
- **Centralized compliance:** WebWaka handles all tax reporting, KYC/AML, and regulatory compliance
- **Better transparency:** Partners can view real-time earnings and payout history
- **Compatible with Partner-Controlled Pricing:** WebWaka collects Partner's retail price from Merchants and pays Partner their share (after platform fees and affiliate commissions)
- **AWS-first alignment:** Centralized billing logic can be implemented as AWS Lambda functions triggered by Stripe/Paystack webhooks

**Alternative Options:**

- **Option B: Delegated Billing (Partners Collect Revenue)** — Partners collect subscription revenue from their Merchants and pay WebWaka a platform fee. Higher operational overhead for Partners.

**Impact of Founder Decision:**

- **If Option A (Centralized):** WebWaka integrates with Stripe or Paystack to collect all revenue. Billing system calculates and disburses commissions monthly. Implementation requires payment gateway integration.
- **If Option B (Delegated):** Partners handle billing themselves. WebWaka invoices Partners monthly for platform fees. Implementation is simpler for WebWaka, but higher operational overhead for Partners.

---

### Decision #10 — Multi-Currency Support

**Category:** Billing / Monetization

**Why This Decision Is Required Now:**

The multi-currency support determines whether WebWaka supports only Nigerian Naira (NGN) or multiple currencies (USD, GBP, EUR, etc.). This affects the billing system, payment gateway integration, and global expansion strategy.

**Decision Question:**

Should WebWaka support only Nigerian Naira (NGN) in Phase 1, or should it support multiple currencies (NGN, USD, GBP, EUR) from day one?

**Recommended Answer (Manus Proposal):**

**Option A: NGN-Only in Phase 1, Multi-Currency in Phase 2**

- Phase 1: Only Nigerian Naira (NGN) is supported
- Phase 2: Add USD, GBP, EUR, and other currencies

**Justification:**

- **Aligns with phased implementation:** Phase 1 focuses on Nigerian market (commerce suites). Phase 2 expands to global markets (education, health, etc.).
- **Simpler implementation:** NGN-only eliminates the need for currency conversion logic, multi-currency pricing, and foreign exchange rate handling.
- **Faster time-to-market:** WebWaka can launch Phase 1 faster without multi-currency complexity.
- **Compatible with Partner-Controlled Pricing:** Partners set prices in NGN in Phase 1. Multi-currency support added in Phase 2.
- **AWS-first alignment:** Simpler billing logic reduces AWS Lambda execution costs.

**Alternative Options:**

- **Option B: Multi-Currency from Day One** — WebWaka supports NGN, USD, GBP, EUR from day one. Requires currency conversion logic, multi-currency pricing, and foreign exchange rate handling. Higher implementation complexity.

**Impact of Founder Decision:**

- **If Option A (NGN-Only Phase 1):** Billing system only supports NGN. Simpler implementation. Faster time-to-market. Multi-currency support added in Phase 2.
- **If Option B (Multi-Currency Day One):** Billing system supports multiple currencies from day one. Higher implementation complexity. Slower time-to-market.

---

## CATEGORY 5: IDENTITY, TENANCY & ORGANIZATION MODEL

---

### Decision #11 — Cross-Platform User Identity

**Category:** Identity / Tenancy

**Why This Decision Is Required Now:**

The cross-platform user identity model determines whether a user can have a single identity across multiple Tenants (Global Identity) or must create separate accounts for each Tenant (Tenant-Scoped Identity). This affects the authentication system, user experience, and data portability.

**Decision Question:**

Should users have a **single global identity** across all Tenants (e.g., user@example.com can access Tenant A and Tenant B with the same login), or should users have **tenant-scoped identities** (e.g., user@example.com in Tenant A is separate from user@example.com in Tenant B)?

**Recommended Answer (Manus Proposal):**

**Option A: Tenant-Scoped Identity (Separate Accounts Per Tenant)**

- Users must create separate accounts for each Tenant
- `user@example.com` in Tenant A is separate from `user@example.com` in Tenant B
- No cross-tenant user portability

**Justification:**

- **Aligns with GoHighLevel model:** GoHighLevel uses tenant-scoped identities. Users must create separate accounts for each Sub-Account.
- **Simpler implementation:** No need to build cross-tenant user management, identity federation, or data portability features.
- **Better isolation:** Users in Tenant A cannot accidentally access Tenant B's data.
- **Faster time-to-market:** WebWaka can launch Phase 1 faster without cross-tenant identity complexity.
- **AWS-first alignment:** AWS Cognito supports tenant-scoped User Pools. No need for identity federation.

**Alternative Options:**

- **Option B: Global Identity (Single Account Across Tenants)** — Users have a single global identity across all Tenants. Requires identity federation, cross-tenant user management, and data portability features. Higher implementation complexity.

**Impact of Founder Decision:**

- **If Option A (Tenant-Scoped):** Users must create separate accounts for each Tenant. Simpler implementation. Faster time-to-market. No cross-tenant user portability.
- **If Option B (Global Identity):** Users have a single global identity across all Tenants. Higher implementation complexity. Slower time-to-market. Better user experience for users who access multiple Tenants.

---

## CATEGORY 6: DATA OWNERSHIP & PORTABILITY

---

### Decision #12 — Tenant Data Ownership & Export Rights

**Category:** Data Ownership / Portability / Compliance

**Why This Decision Is Required Now:**

The data ownership model determines who owns Tenant data (WebWaka, Partner, or Tenant) and what export rights Tenants have. This affects compliance (GDPR, NDPR), Partner agreements, and platform architecture.

**Decision Question:**

Who owns Tenant data, and what export rights do Tenants have?

**Recommended Answer (Manus Proposal):**

**Option A: Tenant Owns Data, Full Export Rights**

- **Tenant owns their data** (contacts, transactions, invoices, etc.)
- **Tenants can export all their data** at any time in JSON or CSV format
- **Partners can view Tenant data** but do not own it
- **WebWaka is a data processor**, not a data controller (GDPR/NDPR compliance)

**Justification:**

- **Aligns with GoHighLevel model:** GoHighLevel allows Sub-Accounts to export all their data
- **Compliance:** GDPR and NDPR require data portability. Tenants must be able to export their data.
- **Trust:** Tenants trust platforms that give them full control over their data.
- **Competitive differentiation:** Full data export rights are a key selling point vs. competitors.
- **AWS-first alignment:** AWS S3 can be used to generate and store data export files.

**Alternative Options:**

- **Option B: Partner Owns Data** — Partners own Tenant data. Tenants cannot export data without Partner approval. Higher risk of vendor lock-in. Lower trust.
- **Option C: WebWaka Owns Data** — WebWaka owns all Tenant data. Tenants cannot export data. Non-compliant with GDPR/NDPR. Not recommended.

**Impact of Founder Decision:**

- **If Option A (Tenant Owns Data):** Tenants can export all their data at any time. Implementation requires data export API and UI. Compliance with GDPR/NDPR.
- **If Option B (Partner Owns Data):** Partners own Tenant data. Tenants cannot export data without Partner approval. Higher risk of vendor lock-in.
- **If Option C (WebWaka Owns Data):** Non-compliant with GDPR/NDPR. Not recommended.

---

## CATEGORY 7: GOVERNANCE & CONTROL

---

### Decision #13 — Platform Kill-Switch Authority

**Category:** Governance / Control / Compliance

**Why This Decision Is Required Now:**

The kill-switch authority determines who can disable a Partner or Tenant account (e.g., for fraud, abuse, or non-payment). This affects compliance, risk management, and Partner autonomy.

**Decision Question:**

Should WebWaka have unilateral authority to disable Partner or Tenant accounts (Platform Kill-Switch), or should Partners have exclusive authority to disable their own Tenants (Partner Kill-Switch)?

**Recommended Answer (Manus Proposal):**

**Option A: Platform Kill-Switch (WebWaka Can Disable Anyone)**

- WebWaka can disable Partner or Tenant accounts for:
  - Fraud or abuse
  - Non-payment
  - Terms of Service violations
  - Legal or regulatory requirements
- Partners can disable their own Tenants
- Tenants cannot disable themselves (must request Partner or WebWaka)

**Justification:**

- **Aligns with GoHighLevel model:** GoHighLevel can disable any Agency or Sub-Account for fraud, abuse, or non-payment
- **Risk management:** WebWaka must be able to disable accounts to prevent fraud, abuse, or legal liability
- **Compliance:** Regulatory requirements may require WebWaka to disable accounts (e.g., AML/KYC violations)
- **Platform integrity:** WebWaka must be able to enforce Terms of Service
- **AWS-first alignment:** Kill-switch logic can be implemented as AWS Lambda functions triggered by admin actions

**Alternative Options:**

- **Option B: Partner Kill-Switch Only** — Partners have exclusive authority to disable their own Tenants. WebWaka cannot disable Tenants (only Partners). Higher risk of fraud or abuse.

**Impact of Founder Decision:**

- **If Option A (Platform Kill-Switch):** WebWaka can disable any Partner or Tenant account. Implementation requires admin UI and kill-switch logic. Lower risk of fraud or abuse.
- **If Option B (Partner Kill-Switch Only):** Partners have exclusive authority to disable their own Tenants. WebWaka cannot disable Tenants. Higher risk of fraud or abuse.

---

### Decision #14 — Recursive System Usage Enforcement

**Category:** Governance / Platform Primitives / Architecture

**Why This Decision Is Required Now:**

Per Clarification 3, all systems (CRM, Automation, Affiliate, etc.) must be usable recursively across levels. This decision determines how recursive usage is enforced and whether it applies to all systems or only specific systems.

**Decision Question:**

Should recursive system usage (Partners can use systems for their Clients, Clients can use systems for their Users) be enforced for **all platform primitives** or only **specific systems** (e.g., Affiliate, CRM)?

**Recommended Answer (Manus Proposal):**

**Option A: Recursive Usage for All Platform Primitives**

- **All platform primitives** (CRM, Automation, Affiliate, Site Builder, Messaging, Reporting, Billing) must be usable recursively across levels
- **Super Admin** uses systems to manage Partners
- **Partners** use systems to manage their Clients
- **Clients** use systems to manage their Users
- **Sub-Clients** use systems to manage their sub-users (if applicable)

**Justification:**

- **Aligns with Clarification 3:** "Anything WebWaka uses internally must be something Partners can use for their Clients, and Clients can use for their own ecosystems."
- **True platform-for-building-platforms:** WebWaka is not a SaaS with add-ons. It is a platform where every system is a reusable primitive.
- **Competitive differentiation:** Recursive usage is a key selling point vs. GoHighLevel (which does not allow Sub-Accounts to use all systems recursively).
- **Consistent architecture:** All systems follow the same recursive usage pattern. No special cases.
- **AWS-first alignment:** Recursive usage does not require additional AWS services. It is an architectural pattern.

**Alternative Options:**

- **Option B: Recursive Usage for Specific Systems Only** — Only specific systems (e.g., Affiliate, CRM) support recursive usage. Other systems (e.g., Site Builder, Messaging) are top-down only. Less consistent architecture.

**Impact of Founder Decision:**

- **If Option A (All Platform Primitives):** All systems support recursive usage. Implementation requires tenant-agnostic design for all systems. Higher implementation complexity, but more consistent architecture.
- **If Option B (Specific Systems Only):** Only specific systems support recursive usage. Simpler implementation, but less consistent architecture.

---

## CATEGORY 8: GOHIGHLEVEL PARITY & BEYOND

---

### Decision #15 — WebWaka vs. GoHighLevel Feature Parity Strategy

**Category:** Product Strategy / Competitive Positioning

**Why This Decision Is Required Now:**

The feature parity strategy determines whether WebWaka aims to match GoHighLevel feature-for-feature (Parity), exceed GoHighLevel with unique features (Differentiation), or focus on a subset of GoHighLevel features (Niche).

**Decision Question:**

Should WebWaka aim for full feature parity with GoHighLevel, or should it differentiate with unique features?

**Recommended Answer (Manus Proposal):**

**Option A: Differentiation (Exceed GoHighLevel in Key Areas)**

- **Match GoHighLevel on core primitives:** CRM, Automation, Communication, Forms, Calendar, Billing
- **Exceed GoHighLevel on platform features:**
  - **Recursive system usage** (Partners can use all systems for their Clients, Clients can use systems for their Users)
  - **Configurable multi-level affiliate system** (up to 10 levels, configurable per partner/contract)
  - **Partner-controlled pricing** (Partners set their own prices, not centralized pricing)
  - **Partner-extensible modules** (Partners can create and publish custom modules in Phase 2+)
- **Exceed GoHighLevel on African market features:**
  - Offline-first POS and ParkHub (GoHighLevel does not support offline)
  - Multi-currency support (NGN, USD, GBP, EUR)
  - Africa-focused integrations (Africa's Talking, Paystack, Flutterwave)
  - Multi-industry suites (Commerce, Education, Health, Civic, Hospitality, Logistics)
- **Do NOT replicate GoHighLevel's weak features:**
  - GoHighLevel's website builder is weak (use Webflow or Framer instead)
  - GoHighLevel's mobile app is weak (build native mobile apps instead)

**Justification:**

- **Competitive differentiation:** WebWaka cannot compete with GoHighLevel on feature parity alone. WebWaka must differentiate with unique features.
- **Platform-for-building-platforms positioning:** Recursive usage, configurable affiliate system, and partner-controlled pricing position WebWaka as a true platform-for-building-platforms (not a SaaS with add-ons).
- **African market focus:** WebWaka's offline-first POS, multi-industry suites, and Africa-focused integrations are unique selling points.
- **Faster time-to-market:** WebWaka can launch Phase 1 faster by focusing on core primitives + differentiation features.
- **AWS-first alignment:** Differentiation allows WebWaka to build features that leverage AWS services (e.g., offline-first with AWS AppSync, multi-currency with AWS Lambda).

**Alternative Options:**

- **Option B: Full Parity** — WebWaka aims to match GoHighLevel feature-for-feature. Requires replicating all GoHighLevel features (website builder, mobile app, etc.). Higher implementation complexity. Slower time-to-market. No competitive differentiation.
- **Option C: Niche** — WebWaka focuses on a subset of GoHighLevel features (e.g., only CRM and Automation). Lower implementation complexity. Faster time-to-market. Limited market appeal.

**Impact of Founder Decision:**

- **If Option A (Differentiation):** WebWaka matches GoHighLevel on core primitives and exceeds GoHighLevel on platform features and African market features. Implementation focuses on unique selling points. Faster time-to-market. Better competitive positioning.
- **If Option B (Full Parity):** WebWaka aims to match GoHighLevel feature-for-feature. Higher implementation complexity. Slower time-to-market. No competitive differentiation.
- **If Option C (Niche):** WebWaka focuses on a subset of GoHighLevel features. Lower implementation complexity. Faster time-to-market. Limited market appeal.

---

## ✅ DECISION SUMMARY TABLE

| # | Decision | Recommended Answer | Impact if Delayed |
|---|----------|-------------------|-------------------|
| **1** | Affiliate Hierarchy Data Model | Closure Table Pattern | Data model cannot be finalized. Affiliate system cannot be built. |
| **2** | Affiliate Configuration Authority Hierarchy | Hierarchical Override Model (Global → Partner → Contract → Org) | Affiliate configuration logic cannot be built. Partners cannot customize affiliate settings. |
| **3** | Affiliate Commission Calculation Model | Fixed Percentages of Subscription Revenue | Billing system cannot be finalized. Partner Portal cannot show earnings. |
| **4** | Affiliate Payout Responsibility | Platform-Managed Payouts | Payout system cannot be built. Partners cannot receive commissions. |
| **5** | Module Creation Authority | Platform-Only Phase 1, Partner-Extensible Phase 2+ | Module development roadmap cannot be finalized. Partner autonomy is unclear. |
| **6** | White-Label Branding Depth | Full White-Label (Frontend + Backend) | Partner Portal cannot be built. Partners cannot fully brand the platform. |
| **7** | Partner Data Isolation Model | Shared Database with Row-Level Security | Database architecture cannot be finalized. AWS Aurora setup cannot proceed. |
| **8** | Pricing Authority Hierarchy | Hierarchical Pricing Model (Global → Partner → Client) | Pricing logic cannot be built. Partners cannot set their own prices. |
| **9** | Billing Model | Centralized Billing | Billing system cannot be built. Revenue flow is unclear. |
| **10** | Multi-Currency Support | NGN-Only in Phase 1, Multi-Currency in Phase 2 | Billing system cannot be finalized. Global expansion strategy is unclear. |
| **11** | Cross-Platform User Identity | Tenant-Scoped Identity | Authentication system cannot be finalized. AWS Cognito setup cannot proceed. |
| **12** | Tenant Data Ownership & Export Rights | Tenant Owns Data, Full Export Rights | Data export API cannot be built. GDPR/NDPR compliance is unclear. |
| **13** | Platform Kill-Switch Authority | Platform Kill-Switch | Admin UI cannot be built. Risk management strategy is unclear. |
| **14** | Recursive System Usage Enforcement | Recursive Usage for All Platform Primitives | Platform architecture cannot be finalized. System design is inconsistent. |
| **15** | WebWaka vs. GoHighLevel Feature Parity Strategy | Differentiation (Exceed GoHighLevel in Key Areas) | Product roadmap cannot be finalized. Competitive positioning is unclear. |

---

## 🔐 NEXT STEPS

Once the Founder responds to these 15 decisions:

1. **Decisions are LOCKED** — No re-interpretation is allowed
2. **Architecture is FINALIZED** — Data model, API design, and AWS infrastructure are locked
3. **Execution documents become authoritative** — Phase 1.1 execution prompt can be issued
4. **Implementation begins** — Manus can start building Core Infrastructure

---

**End of Founder Decision Questions (Pre-Implementation Lock) v2**
