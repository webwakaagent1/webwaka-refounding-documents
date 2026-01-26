# WebWaka Platform Re-Founding Blueprint (vNext) - AUTHORITATIVE

**Document Version:** 2.0 (Updated with Founder Directives)  
**Analysis Date:** 2026-01-26  
**Author:** Manus Re-Founding Phase (WRF-0 Authoritative)

---

## Executive Summary

_This document is the **authoritative WebWaka Platform Re-Founding Blueprint (vNext)**. It supersedes all previous blueprints and constitutions and is written under the two non-negotiable Founder Directives:_

1.  **_AWS-First, Single-Bill Architecture_**
2.  **_Design for Maximum Scale from Day One_**

_This blueprint provides a **clean, forward-looking rebuild plan** that defines WebWaka as a **Platform for Building Platforms (meta-platform)**, architected from the start for its largest envisioned scale and built with a strong preference for AWS-native services._

_Key tenets of this blueprint include:_

*   **_Max-Scale-First Architecture:_** _The platform is designed for thousands of partners and millions of downstream tenants. Architecture is not phased; only implementation is._
*   **_AWS-Native Tooling:_** _All tooling decisions are constrained by the AWS-First directive, favoring services like AWS Cognito, Aurora, Fargate, and SES to ensure a single consolidated bill and deep infrastructure coherence._
*   **_Partner-Centric Model:_** _WebWaka is infrastructure for Partners who build, brand, and resell their own SaaS businesses._
*   **_Composable Primitives:_** _The core of the platform consists of industry-agnostic modules (CRM, Automation, Billing, etc.) that Partners configure into vertical-specific solutions._
*   **_Strict, Sequential Build Order:_** _Implementation is phased (Infrastructure → Primitives → Suites) to manage complexity, but the architecture supports the full scope from day one._
*   **_Clean Slate Transition:_** _The recommended path is to archive the existing codebase and start fresh to avoid architectural debt and ensure a clean foundation._

_This document is the **new canonical execution authority**. All future work must align with this blueprint._

---

## Table of Contents

0. **Founder Non-Negotiable Directives**
1. **Contradictions & Unresolved Tensions Analysis**
2. **Idea Triage: KEEP / DISCARD / RECONSIDER**
3. **Founder Decision Table (CRITICAL)**
4. **Tooling & Platform Re-Evaluation (AWS-First)**
5. **Clean Platform Architecture (Target State)**
6. **Strict, Sequential Build Order**
7. **Governance & Operator Rules**
8. **Transition Plan: From Current State to Target State**
9. **Conflict Report: Prior Recommendations vs. Founder Directives**

---

# FOUNDER NON-NEGOTIABLE DIRECTIVES

**Status:** CANONICAL — These directives are non-negotiable and constrain all architecture, tooling, and execution decisions.

---

## Directive #1: AWS-First, Single-Bill Architecture

### Canonical Position

**WebWaka will be built AWS-first, with a strong preference for AWS-native services over third-party platforms wherever viable.**

### Rationale

1. **Single Consolidated Monthly Bill:** Cost visibility and control. All infrastructure costs appear on one AWS bill, not scattered across multiple SaaS vendors.

2. **Reduced Platform Sprawl:** Fewer external dependencies reduce operational complexity, integration overhead, and vendor lock-in risk.

3. **Deeper Infrastructure Coherence:** AWS-native services integrate seamlessly with each other (IAM, VPC, CloudWatch, EventBridge, etc.), reducing integration complexity.

4. **Long-Term Scalability:** AWS is proven at global, enterprise-grade scale. AWS-native services scale automatically without manual intervention.

5. **Enterprise-Grade Reliability:** AWS SLAs are industry-leading. AWS-native services are battle-tested and highly available.

### Operational Clarification (CRITICAL)

- **Manus WILL be granted access to AWS accounts** for setup and configuration.
- This removes the constraint of "who will configure AWS."
- Therefore, **AWS complexity is NOT a blocker.**
- Tool choices should **NOT default to SaaS convenience** when AWS-native equivalents exist.

### Implications

**All tooling decisions must be re-evaluated under AWS-first constraint:**

| Domain | AWS-Native Option | Third-Party SaaS | Preference |
|--------|-------------------|------------------|------------|
| **Authentication** | AWS Cognito | Clerk, Auth0 | AWS Cognito (unless Cognito is insufficient) |
| **Email** | AWS SES | Resend, SendGrid | AWS SES |
| **SMS** | AWS SNS | Africa's Talking, Twilio | AWS SNS (where supported) |
| **Queues** | AWS SQS | RabbitMQ, Redis | AWS SQS |
| **Events** | AWS EventBridge | Kafka, RabbitMQ | AWS EventBridge |
| **Storage** | AWS S3 | Cloudflare R2, DigitalOcean Spaces | AWS S3 |
| **Analytics** | AWS CloudWatch, Athena | PostHog, Mixpanel | AWS CloudWatch + Athena |
| **Background Jobs** | AWS Lambda | Celery, Bull | AWS Lambda |
| **Database** | AWS RDS, Aurora | Neon, PlanetScale | AWS RDS or Aurora |
| **Hosting** | AWS ECS, Fargate, Lambda | Fly.io, Vercel | AWS ECS or Fargate |

**Third-party tools must justify why AWS-native options are insufficient.**

### Where This Directive Applies

- **Tooling & Platform Re-Evaluation** (Section 4)
- **Clean Platform Architecture** (Section 5)
- **Founder Decision Table** (Section 3)

---

## Directive #2: Design for Maximum Scale from Day One

### Canonical Position

**WebWaka must be architected from the start for its largest envisioned scale.**

This means:

- **WebWaka is a Platform for Building Platforms** (meta-platform)
- **NOT a SaaS** (not a vertical SaaS for Nigerian commerce)
- **NOT a narrow MVP** (not a minimal viable product)
- **NOT a small-business tool** (not a tool for small businesses)

### Clarification (CRITICAL)

- **Implementation may be phased** (build incrementally)
- **Architecture must NOT be phased** (design for full scope upfront)
- **We do NOT "scale up later"** (we design for scale now, execute incrementally)

### Implications

**All architecture diagrams, data models, auth models, tenancy, billing, and partner systems must assume:**

1. **Thousands of Partners** (not 10, not 100, but thousands)
2. **Millions of Downstream Tenants** (not hundreds, not thousands, but millions)
3. **Multi-Level Affiliate Trees** (Partner → SubPartner → Agent → Merchant, with unlimited depth)
4. **Industry-Agnostic Modules** (not commerce-only, but composable primitives for all industries)
5. **White-Labeled SaaS Resale** (Partners resell WebWaka as their own branded platform)

**Any recommendation that simplifies scope must be labeled as implementation sequencing, not architectural limitation.**

### Examples of Max-Scale-First Design

| Decision | Small-Scale Approach | Max-Scale-First Approach |
|----------|----------------------|--------------------------|
| **Database** | SQLite, single PostgreSQL instance | AWS Aurora (multi-region, read replicas) |
| **Authentication** | Single-tenant auth | Multi-tenant auth with Partner-level isolation |
| **Billing** | Manual invoicing | Automated billing with Partner revenue sharing |
| **Affiliate System** | Flat referral program | Multi-level affiliate tree with unlimited depth |
| **Tenancy** | Tenant-centric (Tenants are top-level) | Partner-centric (Partners own Tenants) |
| **Modularity** | Monolithic (all features in one codebase) | Modular (composable primitives, industry-agnostic) |

### Where This Directive Applies

- **Executive Summary** (Section 0)
- **Clean Platform Architecture** (Section 5)
- **Sequential Build Order** (Section 6)
- **Founder Decision Table** (Section 3)

---

## Governing Constraint

**All subsequent decisions, tooling evaluations, and architectural recommendations are made under these two Founder directives.**

Any recommendation that conflicts with these directives is invalid and must be revised.

---

**End of Founder Non-Negotiable Directives**
# 1. CONTRADICTIONS & UNRESOLVED TENSIONS ANALYSIS

**Analysis Date:** 2026-01-26  
**Analyst:** Manus Re-Founding Phase (WRF-0 Authoritative)

---

## Executive Summary

This section exposes the **fundamental contradictions** that exist between:

1. **What was built** (the existing Constitution and codebase)
2. **What was assumed** (the previous blueprint's scope)
3. **What is actually promised** (the marketing site and GoHighLevel-class vision)

These are not minor inconsistencies. They represent **three incompatible mental models** of what WebWaka is:

- **Model A:** A multi-tenant commerce platform with POS, inventory, and marketplaces
- **Model B:** A modular platform with installable suites and white-label capabilities
- **Model C:** A meta-platform for building SaaS platforms (GoHighLevel-class infrastructure)

**The platform cannot be all three at once.** The Founder must choose.

---

## Category 1: Identity Crisis — What is WebWaka?

### C1.1 — Vertical SaaS vs. Meta-Platform

**The Contradiction:**

- **Constitution (Sections 3-4):** WebWaka is a **vertical SaaS platform** with specific suites (POS, ParkHub, SVM, MVM) for Nigerian commerce.
- **Marketing Site:** WebWaka is **"Platform Infrastructure for Digital Transformation Partners"** — a meta-platform for building platforms.
- **Holistic Scope Expansion:** WebWaka is a **GoHighLevel-class system** where Partners build their own SaaS businesses on WebWaka infrastructure.

**Evidence:**

- Constitution documents 4 commerce-focused suites
- Marketing site promises 7 industry suites (commerce, education, health, civic, hospitality, logistics)
- Marketing site says: "Build Your Own SaaS Platforms" (plural)
- Marketing site says: "The Platform for Building Platforms"

**Impact:**

- **Architecture mismatch:** Vertical SaaS requires domain-specific logic. Meta-platforms require composable, industry-agnostic primitives.
- **Target user mismatch:** Vertical SaaS sells to end users (merchants). Meta-platforms sell to platform operators (partners).
- **Revenue model mismatch:** Vertical SaaS charges per tenant. Meta-platforms charge per partner + revenue share.

**Resolution Required:**

The Founder must explicitly choose:

- **Option A:** WebWaka is a vertical SaaS for Nigerian commerce (abandon meta-platform vision)
- **Option B:** WebWaka is a meta-platform (abandon vertical-specific features, build composable primitives)
- **Option C:** WebWaka is a hybrid (define clear boundaries between platform and vertical features)

---

### C1.2 — Tenant-Centric vs. Partner-Centric Architecture

**The Contradiction:**

- **Constitution (Section 5):** The platform is **tenant-centric**. Tenants are the primary entity. Partners are metadata.
- **Marketing Site:** The platform is **partner-centric**. Partners create and manage tenants. Partners own client relationships.
- **Holistic Scope Expansion:** Partners are **first-class platform operators** who brand, price, and resell SaaS.

**Evidence:**

- Constitution: "All data and operations must be isolated by `tenantId`"
- Constitution: Only Super Admins can create tenants
- Marketing Site: "Partners create client organizations and platform instances"
- Marketing Site: "Your brand, your pricing, your relationship"

**Impact:**

- **Data model mismatch:** Tenant-centric architecture has no concept of "Partner owns Tenant"
- **Authorization mismatch:** Partners cannot create tenants (only Super Admins can)
- **Billing mismatch:** No revenue sharing model for Partners

**Resolution Required:**

The Founder must decide:

- **Option A:** Remain tenant-centric (Partners are just metadata, Super Admins control provisioning)
- **Option B:** Become partner-centric (Partners control tenant provisioning, billing, and lifecycle)
- **Option C:** Hybrid model (Super Admins create Partners, Partners create Tenants)

---

### C1.3 — Commerce Platform vs. Multi-Industry Platform

**The Contradiction:**

- **Constitution (Section 4):** WebWaka has 4 commerce-focused suites (POS, ParkHub, SVM, MVM)
- **Marketing Site:** WebWaka has 7 industry suites (commerce, education, health, civic, hospitality, logistics)
- **Holistic Scope Expansion:** WebWaka is **industry-agnostic** with composable capabilities

**Evidence:**

- Constitution documents only commerce suites
- Marketing site explicitly lists: School Management, Grading, Fees, LMS, Clinic, Pharmacy, Patient Records, Community Finance, Hotels, Restaurants, Events, Fleet, Delivery, Warehousing
- Marketing site says: "Multi-industry by design"

**Impact:**

- **Scope explosion:** 7 industries × 18+ capabilities = 100+ features
- **Architectural mismatch:** Commerce-specific logic (inventory, POS) doesn't generalize to education or health
- **Resource mismatch:** Building 7 industry suites requires 7× the resources

**Resolution Required:**

The Founder must decide:

- **Option A:** Focus on commerce only (abandon multi-industry vision)
- **Option B:** Build multi-industry platform (accept massive scope increase)
- **Option C:** Build industry-agnostic primitives (CRM, automation, forms) and let Partners configure industry-specific workflows

---

## Category 2: Architectural Contradictions

### C2.1 — "Modularity" vs. Monolithic Core API

**The Contradiction:**

- **Invariant (Section 3):** "The platform is composed of modules that are installable, removable, and verifiable."
- **Reality (Section 5, 6):** The Core API is a **monolithic Fastify application** with all domains baked in.

**Evidence:**

- Core API contains: Identity, Tenants, Roles, Permissions, Partners, Audit Logs, Feature Flags, Entitlements, Modules, Pricing Plans, Branding
- No evidence of runtime module loading, unloading, or isolation
- Module Registry exists but is not integrated into Core API

**Impact:**

- "Modularity" is aspirational, not implemented
- Cannot install/remove modules without redeploying entire Core API
- Module manifest system is not enforced

**Resolution Required:**

- **Option A:** Implement true runtime modularity (microservices, plugin architecture)
- **Option B:** Acknowledge Core API is monolithic and redefine "modularity" as logical separation only

---

### C2.2 — "Offline-First" vs. "Core-Aware"

**The Contradiction:**

- **Invariant (Section 3):** "Core commerce suites (POS, ParkHub) must be functional offline."
- **Reality (Section 21):** POS is now "Core-Aware," "Entitlement-Aware," "Inventory-Aware," and "Ledger-Aware" with 30-second sync intervals.

**Evidence:**

- POS authenticates against Core when online
- POS fetches entitlements from Core
- POS syncs inventory deltas and sales events to Core
- "Offline guarantees preserved" is claimed, but architecture is now **hybrid online/offline**

**Impact:**

- Tension between "offline-first" and "Core integration"
- Unclear what happens when Core is unreachable for extended periods
- Sync conflicts, version compatibility, and eventual consistency are not fully resolved

**Resolution Required:**

- Define **exactly** what "offline-first" means in a Core-integrated world
- Clarify conflict resolution rules when POS and Core diverge
- Document maximum offline duration before degradation

---

### C2.3 — "No Demo Mode" vs. Demo Mode Everywhere

**The Contradiction:**

- **Invariant (Section 3):** "No Demo Mode — The rebuilt platform must not contain any demo tenants, demo bypasses, or fake data."
- **Reality (Sections 1.5, 14, 17):** The platform is **saturated** with demo accounts, demo authentication policies, demo tenants, and demo mode indicators.

**Evidence:**

- Demo credentials documented in Section 1.5
- Demo Authentication Policy (Section 14) explicitly disables 2FA for demo users
- Demo email migration plan (Section 17) migrates demo accounts, not removes them
- POS explicitly includes "demo mode indicators" (Section 21)

**Impact:**

- The "No Demo Mode" invariant is **violated by design**
- Unclear whether demo infrastructure is temporary scaffolding or permanent
- Creates confusion about what "production-grade" means

**Resolution Required:**

- **Option A:** Acknowledge demo mode is permanent and update the invariant
- **Option B:** Commit to removing all demo infrastructure before production and define a timeline

---

## Category 3: Scope Mismatches

### C3.1 — Constitution Scope vs. Marketing Site Scope

**The Mismatch:**

The Constitution documents **15% of what the marketing site promises**.

| Capability Category | Marketing Site | Constitution | Gap |
|---------------------|----------------|--------------|-----|
| CRM & Pipelines | ✅ Implied | ❌ Not documented | 100% |
| Automation & Workflows | ✅ Implied | ❌ Not documented | 100% |
| Communication (Email, SMS, WhatsApp) | ✅ Implied | ❌ Not documented | 100% |
| Forms & Surveys | ✅ Implied | ❌ Not documented | 100% |
| Calendar & Booking | ✅ Implied | ❌ Not documented | 100% |
| Landing Pages & Website Builder | ✅ Implied | ❌ Not documented | 100% |
| Analytics & Reporting | ✅ Implied | ⚠️ Partially documented | 80% |
| API & Webhooks | ✅ Implied | ❌ Not documented | 100% |
| Partner Portal | ✅ Explicit | ❌ Not documented | 100% |
| Affiliate Program (Multi-Level) | ✅ Implied | ❌ Not documented | 100% |
| Education Suite | ✅ Explicit | ❌ Not documented | 100% |
| Health Suite | ✅ Explicit | ❌ Not documented | 100% |
| Civic Suite | ✅ Explicit | ❌ Not documented | 100% |
| Hospitality Suite | ✅ Explicit | ❌ Not documented | 100% |
| Logistics Suite | ✅ Explicit | ❌ Not documented | 100% |

**Impact:**

- **Massive scope gap:** Marketing site promises 10× more than Constitution documents
- **Expectation mismatch:** Partners/users expect features that don't exist
- **Resource mismatch:** Building everything on marketing site requires 10× the resources

**Resolution Required:**

The Founder must decide:

- **Option A:** Update marketing site to match Constitution (reduce scope)
- **Option B:** Update Constitution to match marketing site (increase scope)
- **Option C:** Phase the vision (document what exists now, what's planned, what's deferred)

---

### C3.2 — Previous Blueprint Scope vs. Holistic Scope Expansion

**The Mismatch:**

The previous blueprint assumed WebWaka was a **commerce platform with white-label capabilities**.

The holistic scope expansion reveals WebWaka is a **GoHighLevel-class meta-platform** with:

- Multi-level affiliate systems (Partner → Sub-Partner → Agent → Merchant)
- Partner-as-platform-operator model
- CRM, automation, communication, workflows, billing, SaaS resale
- Industry-agnostic, composable suites
- 100+ missing capabilities

**Impact:**

- The previous blueprint's architecture is **insufficient**
- The previous blueprint's build order is **incorrect** (builds suites before primitives)
- The previous blueprint's governance model is **incomplete** (no Partner Portal, no affiliate system)

**Resolution Required:**

- The previous blueprint must be **superseded** by this authoritative vNext blueprint
- All assumptions in the previous blueprint must be **re-evaluated**

---

## Category 4: Governance Ambiguities

### C4.1 — "Single Source of Truth" Ambiguity

**The Contradiction:**

- **Governance Invariant (Section 3):** "GitHub is Single Source of Truth"
- **Reality (Section 21):** "POS as Permanent Source of Truth — Core is a read-only replica; POS always wins in conflict resolution."

**Evidence:**

- GitHub is declared the source of truth for **code**
- POS is declared the source of truth for **commerce data**
- Core API is the source of truth for **identity and governance**
- Unclear which system is authoritative for **inventory**, **pricing**, and **entitlements**

**Impact:**

- Multiple "sources of truth" create conflict resolution ambiguity
- Unclear what happens when POS, Core, and GitHub disagree
- No canonical conflict resolution hierarchy

**Resolution Required:**

- Define a **conflict resolution hierarchy** for each data domain
- Clarify whether "source of truth" means "authoritative writer" or "canonical replica"

---

### C4.2 — Vibecoding Platform Governance (Abandoned?)

**The Contradiction:**

- **Repository Responsibility Atlas (Section 4):** Maps 11+ repositories to specific vibecoding platforms (Emergent-01, Lovable-01, Replit-01, etc.)
- **Reality (Section 6, 19):** Manus is now the **primary operator** executing all phases

**Evidence:**

- All recent execution prompts (IR-1, IR-2, EP-1) are executed by Manus
- No evidence of Emergent, Lovable, or Replit being used in recent phases
- Repository Responsibility Atlas maps repos to vibecoding platforms, but these platforms are not mentioned in recent work

**Impact:**

- The vibecoding governance model is **not being followed**
- Unclear whether Emergent/Lovable/Replit are still part of the plan
- Manus is doing all the work, making the "one repo, one account" rule irrelevant

**Resolution Required:**

- **Option A:** Acknowledge Manus is the sole operator and remove vibecoding governance
- **Option B:** Clarify when and how Emergent/Lovable/Replit will be reintroduced

---

## Category 5: Unresolved Tensions

### C5.1 — Shared vs. Isolated Infrastructure (Unresolved)

**Tension:**

- **Canon Decision (Section 15.C):** "All infrastructure components are SHARED between development and production"
- **Status:** Verification Status = "REQUIRES SAFEGUARD DOCUMENTATION"

**Evidence:**

- Database, Clerk, and Fly.io are shared (verified)
- Safeguards are **required but not documented**
- Blast-radius risks are acknowledged but not mitigated

**Impact:**

- Development changes can affect production data
- No documented isolation strategy
- High risk of accidental production corruption

**Resolution Required:**

- **Option A:** Document safeguards (tenant prefixes, RBAC, audit logging)
- **Option B:** Split infrastructure into dev/prod and document separation mechanics

---

### C5.2 — Affiliate System as Core vs. Add-On

**Tension:**

- **Holistic Scope Expansion:** Multi-level affiliate system (Partner → Sub-Partner → Agent → Merchant) is a **growth engine** and **core architectural component**
- **Constitution:** No mention of affiliate system at all

**Evidence:**

- Holistic Scope Expansion defines 40% commission payout (20% Agent + 15% Sub-Partner + 5% Partner)
- Holistic Scope Expansion defines recurring monthly commissions, industry-specific modifiers, fraud prevention
- Constitution has no Affiliate Domain, no commission tracking, no payout system

**Impact:**

- **Massive architectural gap:** Affiliate system requires new domains (Affiliate, Billing, Payouts)
- **Data model gap:** Need to track Partner → Sub-Partner → Agent → Merchant hierarchy
- **Revenue model gap:** Need to calculate and distribute commissions

**Resolution Required:**

The Founder must decide:

- **Option A:** Affiliate system is core (build it in Phase 1)
- **Option B:** Affiliate system is deferred (build it in Phase 3+)
- **Option C:** Affiliate system is optional (let Partners handle their own affiliate programs)

---

## Summary: The Three Incompatible Mental Models

### Model A: Vertical SaaS for Nigerian Commerce

- **Target User:** Nigerian merchants (retail, transport, marketplaces)
- **Revenue Model:** Subscription per tenant
- **Architecture:** Domain-specific logic (POS, inventory, payments)
- **Scope:** 4 commerce suites (POS, ParkHub, SVM, MVM)
- **Status:** This is what the Constitution documents

### Model B: Modular Multi-Industry Platform

- **Target User:** Businesses across 7 industries (commerce, education, health, civic, hospitality, logistics)
- **Revenue Model:** Subscription per tenant + industry-specific pricing
- **Architecture:** Industry-agnostic primitives + industry-specific configurations
- **Scope:** 7 industry suites × 18+ capabilities = 100+ features
- **Status:** This is what the marketing site promises

### Model C: Meta-Platform for Building SaaS Platforms

- **Target User:** Partners (platform operators who build their own SaaS businesses)
- **Revenue Model:** Partner subscription + revenue share (40% commission payout)
- **Architecture:** Composable primitives (CRM, automation, communication) + Partner Portal + Affiliate System
- **Scope:** GoHighLevel-class infrastructure (not vertical features)
- **Status:** This is what the holistic scope expansion reveals

---

## The Founder Must Choose

**These three models are incompatible.** They require different architectures, different data models, different revenue models, and different build orders.

**The platform cannot be all three at once.**

The Founder must explicitly choose one model (or define a phased transition from one to another) before any rebuild can proceed.

---

**End of Contradictions & Unresolved Tensions Analysis**
# 2. IDEA TRIAGE: KEEP / DISCARD / RECONSIDER

**Analysis Date:** 2026-01-26  
**Analyst:** Manus Re-Founding Phase (WRF-0 Authoritative)

---

## Methodology

Every idea, pattern, decision, and architectural choice from both the existing Constitution and the holistic scope expansion has been evaluated and placed into one of three categories:

- **KEEP:** Validated, working, and worth preserving in the rebuild
- **DISCARD:** Mistakes, anti-patterns, dead ends, or unnecessary complexity
- **RECONSIDER:** Good ideas that require a fresh decision or different implementation

This triage is informed by the **GoHighLevel-class meta-platform vision** revealed in the holistic scope expansion.

---

## KEEP — Validated and Worth Preserving

### K1. Multi-Tenancy as Core Architecture

**What:** The platform is multi-tenant by default with tenant-scoped data isolation.

**Why Keep:** Fundamental to the business model (multiple partners, multiple tenants). Correctly identified as a platform invariant. Enables scalability and resource sharing.

**Carry Forward:** Yes, but **expand to support Partner → Tenant hierarchy** (not just Tenant alone).

---

### K2. Nigeria-First Design Philosophy

**What:** Platform designed for Nigerian market (NGN currency, 7.5% VAT, Nigerian phone normalization, Nigerian payment gateways like Paystack).

**Why Keep:** Aligns with target market. Specific and actionable. Prevents premature internationalization complexity.

**Carry Forward:** Yes, maintain as primary market focus. Add WhatsApp integration (high priority for African markets).

---

### K3. Mobile-First UI Design

**What:** All user interfaces designed for mobile first, desktop secondary.

**Why Keep:** Aligns with Nigerian market reality (mobile-dominant). Forces simplicity and focus. Proven best practice.

**Carry Forward:** Yes, enforce in all UI development.

---

### K4. Offline-First for Commerce Suites

**What:** Core commerce suites (POS, ParkHub) must function offline with transaction queuing and sync.

**Why Keep:** Critical for Nigerian market (unreliable connectivity). Differentiator from cloud-only competitors. Correctly identified as a platform invariant.

**Carry Forward:** Yes, but **clarify hybrid online/offline model** (see RECONSIDER section for "Offline-First" definition).

---

### K5. PWA Delivery Model

**What:** Platform delivered as Progressive Web Apps.

**Why Keep:** Avoids app store gatekeeping and fees. Enables instant updates. Works across all devices. Reduces development overhead (no native apps).

**Carry Forward:** Yes, maintain as primary delivery model.

---

### K6. Audit Trail for All Write Operations

**What:** All write operations generate immutable audit log entries.

**Why Keep:** Essential for compliance and forensics. Correctly implemented in current system. Provides accountability and traceability.

**Carry Forward:** Yes, maintain as non-negotiable requirement. Extend to Partner and Affiliate actions.

---

### K7. Role-Based Access Control (RBAC)

**What:** Authorization enforced at service layer with roles, permissions, and scopes.

**Why Keep:** Standard industry practice. Correctly implemented in current system. Enables fine-grained access control.

**Carry Forward:** Yes, **expand to support Partner-level roles** (Partner Admin, Sub-Partner, Agent).

---

### K8. Idempotent Webhooks and Event Processing

**What:** All payment webhooks and event handlers are idempotent to prevent duplicate processing.

**Why Keep:** Critical for financial correctness. Prevents double-charging and data corruption. Industry best practice.

**Carry Forward:** Yes, enforce in all event-driven systems. Extend to affiliate commission calculations.

---

### K9. Verifiable Receipts

**What:** All receipts include a hash for verification and support public verification.

**Why Keep:** Builds trust with customers. Enables dispute resolution. Differentiator from competitors.

**Carry Forward:** Yes, extend to all transaction types (not just POS).

---

### K10. .com as Primary Domain

**What:** .com is authoritative for all surfaces, .ng is legacy/transitional.

**Why Keep:** Aligns with international expansion. Avoids country-specific domain limitations. Correct reversal from original .ng choice.

**Carry Forward:** Yes, maintain as domain policy. **Add support for partner-specific custom domains** (e.g., `partner-name.com`).

---

### K11. Clerk for Authentication

**What:** Clerk provides user authentication and session management.

**Why Keep:** Working well in current system. Reduces custom auth complexity. Provides SSO, MFA, and session management out of the box.

**Carry Forward:** Yes, unless Founder has strong preference for alternative (see Founder Decision Table).

---

### K12. PostgreSQL as Primary Database

**What:** PostgreSQL (via Neon) as the primary relational database.

**Why Keep:** Industry-standard, battle-tested. Rich feature set (JSONB, full-text search, row-level security). Good ecosystem and tooling.

**Carry Forward:** Yes, but **reconsider Neon as provider** (see RECONSIDER section).

---

### K13. Prisma as ORM

**What:** Prisma ORM for database access with type-safe queries.

**Why Keep:** Working well in current system. Type safety reduces bugs. Good migration tooling. Active ecosystem.

**Carry Forward:** Yes, unless Founder has strong preference for alternative.

---

### K14. CI/CD from First Commit

**What:** Continuous Integration and Deployment must exist from the first commit.

**Why Keep:** Prevents "works on my machine" issues. Enables rapid iteration. Forces reproducible builds.

**Carry Forward:** Yes, enforce as non-negotiable requirement.

---

### K15. GitHub as Code Source of Truth

**What:** changerplanet GitHub organization is the single authoritative source for all code.

**Why Keep:** Standard practice. Enables version control, collaboration, and audit. Integrates with CI/CD.

**Carry Forward:** Yes, maintain as governance rule.

---

### K16. White-Label Branding Capability

**What:** Per-tenant branding (logo, colors, email templates).

**Why Keep:** Essential for Partner-as-platform-operator model. Already implemented. Differentiator from competitors.

**Carry Forward:** Yes, **expand to support Partner-level branding** (Partners brand their entire platform, not just individual tenants).

---

### K17. Entitlements System

**What:** Feature flags and entitlements gate capabilities per tenant.

**Why Keep:** Enables flexible pricing and capability activation. Correctly implemented in current system.

**Carry Forward:** Yes, **expand to support Partner-driven capability activation** (Partners activate capabilities for their tenants).

---

### K18. Pricing Plans Metadata

**What:** Pricing plans define tiers and capabilities.

**Why Keep:** Foundational for monetization. Correctly structured.

**Carry Forward:** Yes, but **expand to support Partner-specific pricing** (Partners set their own pricing for their tenants).

---

## DISCARD — Mistakes, Anti-Patterns, and Dead Ends

### D1. "No Demo Mode" Invariant (While Building Demo Mode)

**What:** Platform Invariant states "No Demo Mode" while the entire system is built around demo accounts, demo tenants, and demo authentication policies.

**Why Discard:** Contradicts reality. Creates confusion. Impossible to enforce.

**Lesson Learned:** If demo mode is needed for onboarding and testing, acknowledge it explicitly and design it properly (sandboxed, clearly marked, with reset mechanisms).

---

### D2. Excessive "Canon Lock" Usage

**What:** 20+ decisions declared "CANON LOCKED - IRREVERSIBLE" including trivial technical choices like "Use Fastify for Core API."

**Why Discard:** Creates artificial rigidity. Discourages learning and adaptation. Makes the document feel authoritarian. Most "canon locked" decisions are actually reversible with effort.

**Lesson Learned:** Reserve "locked" status for truly irreversible decisions (legal, regulatory, contractual). Technical choices should be "current decision" with rationale, not "locked forever."

---

### D3. Vibecoding Platform Governance (Emergent/Lovable/Replit)

**What:** Repository Responsibility Atlas maps 11+ repositories to specific vibecoding platforms (Emergent-01, Lovable-01, Replit-01, etc.) with strict one-repo-one-account rules.

**Why Discard:** Not being followed in practice (Manus does all the work). Premature and over-engineered. Adds coordination overhead without clear benefit. Unclear if Emergent/Lovable/Replit are even available or suitable.

**Lesson Learned:** Don't design governance for tools you haven't validated. Start simple (one operator, one workflow) and add complexity only when needed.

---

### D4. Premature Repository Splitting

**What:** 11+ repositories planned (webwaka-core-registry, webwaka-core-identity, webwaka-core-payments, etc.) but only 2 are active.

**Why Discard:** Premature optimization. Creates coordination overhead. Violates "modularity" if all code is in one monolithic Core API anyway. No evidence that splitting is necessary or beneficial at current scale.

**Lesson Learned:** Start with a monorepo or minimal split (backend + frontend). Split repositories only when there's a clear need (independent deployment, separate teams, different release cycles).

---

### D5. "Documentation-as-Execution-Blocker" Pattern

**What:** "If the document is not updated, the task is NOT complete." Every execution prompt requires immediate Google Doc updates.

**Why Discard:** Creates documentation bottleneck. Slows execution velocity. Risk of documentation drift if updates are forgotten. Prioritizes writing over building.

**Lesson Learned:** Documentation is important, but it should not block execution. Use code as documentation (self-documenting code, tests, README files). Reserve manual documentation for high-level decisions and architecture.

---

### D6. Shared Dev/Prod Infrastructure Without Safeguards

**What:** Database, Clerk, and Fly.io are shared between development and production, but safeguards are "REQUIRES DOCUMENTATION" (i.e., not implemented).

**Why Discard:** High blast-radius risk. No isolation between dev and prod. Accidental production corruption is inevitable.

**Lesson Learned:** Either implement proper safeguards (tenant prefixes, RBAC, audit logging) or split infrastructure into dev/prod. "Shared but safe" requires active enforcement, not just documentation.

---

### D7. "Modularity" Without Runtime Module Loading

**What:** Platform Invariant states "Modularity — modules are installable, removable, and verifiable" but Core API is a monolithic Fastify application.

**Why Discard:** Aspirational, not implemented. Creates false expectations. Module manifest system is not enforced.

**Lesson Learned:** Don't declare architectural invariants that aren't implemented. Either implement true runtime modularity (microservices, plugin architecture) or acknowledge Core API is monolithic and redefine "modularity" as logical separation only.

---

### D8. Commerce-Only Scope (When Marketing Site Promises Multi-Industry)

**What:** Constitution documents 4 commerce-focused suites (POS, ParkHub, SVM, MVM) but marketing site promises 7 industry suites (commerce, education, health, civic, hospitality, logistics).

**Why Discard:** Massive scope mismatch. Marketing site promises 10× more than Constitution documents. Creates expectation mismatch.

**Lesson Learned:** Either update marketing site to match Constitution (reduce scope) or update Constitution to match marketing site (increase scope). The two must be aligned.

---

## RECONSIDER — Good Ideas Requiring Fresh Decisions

### R1. "Offline-First" Definition in a Core-Integrated World

**What:** Platform Invariant states "Offline-First — Core commerce suites (POS, ParkHub) must be functional offline." But POS is now "Core-Aware" with 30-second sync intervals.

**Why Reconsider:** Tension between "offline-first" and "Core integration." Unclear what happens when Core is unreachable for extended periods. Sync conflicts, version compatibility, and eventual consistency are not fully resolved.

**Fresh Decision Required:** Define **exactly** what "offline-first" means in a Core-integrated world. Options:

- **Option A:** Offline-first means "can function without Core for 24 hours" (define degradation timeline)
- **Option B:** Offline-first means "can function without Core indefinitely" (POS is fully autonomous)
- **Option C:** Offline-first means "can queue transactions offline and sync when online" (current implementation)

**Recommendation:** Option A. Define maximum offline duration (e.g., 24 hours) before degradation. Document conflict resolution rules when POS and Core diverge.

---

### R2. Neon as Database Provider

**What:** PostgreSQL via Neon as the primary relational database.

**Why Reconsider:** Neon is a relatively new provider (founded 2021). Unclear long-term stability, pricing, and support. No documented evaluation of alternatives (AWS RDS, Google Cloud SQL, Supabase, self-hosted).

**Fresh Decision Required:** Evaluate Neon vs. alternatives. Options:

- **Option A:** Keep Neon (low-friction, serverless, good developer experience)
- **Option B:** Switch to AWS RDS (enterprise-grade, proven at scale)
- **Option C:** Switch to Supabase (includes auth, storage, and realtime features)
- **Option D:** Self-host PostgreSQL (full control, higher operational overhead)

**Recommendation:** Keep Neon for now (low-friction), but **document exit strategy** (how to migrate to alternative if Neon fails).

---

### R3. Monolithic Core API vs. Microservices

**What:** Core API is a monolithic Fastify application with all domains baked in.

**Why Reconsider:** Monolithic architecture is simpler to build and deploy, but harder to scale and maintain. Microservices enable independent scaling and deployment, but add coordination overhead.

**Fresh Decision Required:** Choose architecture. Options:

- **Option A:** Keep monolithic Core API (simpler, faster to build)
- **Option B:** Split into microservices (CRM, Automation, Communication, etc.)
- **Option C:** Hybrid (monolithic Core API + separate services for high-load domains like CRM, Automation)

**Recommendation:** Option A for Phase 1 (keep monolithic). Option C for Phase 2+ (split high-load domains into separate services).

---

### R4. Tenant-Centric vs. Partner-Centric Architecture

**What:** Constitution is tenant-centric (tenants are the primary entity). Marketing site is partner-centric (Partners create and manage tenants).

**Why Reconsider:** Architectural mismatch. Tenant-centric architecture has no concept of "Partner owns Tenant." Partner-centric architecture requires Partner → Tenant hierarchy.

**Fresh Decision Required:** Choose architecture. Options:

- **Option A:** Remain tenant-centric (Partners are just metadata, Super Admins control provisioning)
- **Option B:** Become partner-centric (Partners control tenant provisioning, billing, and lifecycle)
- **Option C:** Hybrid model (Super Admins create Partners, Partners create Tenants)

**Recommendation:** Option B (partner-centric). This aligns with the GoHighLevel-class meta-platform vision. Partners are first-class platform operators.

---

### R5. Vertical SaaS vs. Meta-Platform

**What:** Constitution documents vertical SaaS (4 commerce suites). Marketing site promises meta-platform (7 industry suites + composable capabilities).

**Why Reconsider:** Identity crisis. Vertical SaaS requires domain-specific logic. Meta-platforms require composable, industry-agnostic primitives.

**Fresh Decision Required:** Choose platform model. Options:

- **Option A:** Vertical SaaS for Nigerian commerce (abandon meta-platform vision)
- **Option B:** Meta-platform (abandon vertical-specific features, build composable primitives)
- **Option C:** Hybrid (build composable primitives + industry-specific configurations)

**Recommendation:** Option C (hybrid). Build composable primitives (CRM, Automation, Communication, Forms, Calendar) and let Partners configure industry-specific workflows. This enables multi-industry support without building 7 separate platforms.

---

### R6. Affiliate System as Core vs. Add-On

**What:** Holistic scope expansion defines multi-level affiliate system (Partner → Sub-Partner → Agent → Merchant) with 40% commission payout. Constitution has no mention of affiliate system.

**Why Reconsider:** Massive architectural gap. Affiliate system requires new domains (Affiliate, Billing, Payouts). Data model gap (need to track Partner → Sub-Partner → Agent → Merchant hierarchy). Revenue model gap (need to calculate and distribute commissions).

**Fresh Decision Required:** Choose affiliate system priority. Options:

- **Option A:** Affiliate system is core (build it in Phase 1)
- **Option B:** Affiliate system is deferred (build it in Phase 3+)
- **Option C:** Affiliate system is optional (let Partners handle their own affiliate programs)

**Recommendation:** Option B (deferred to Phase 2). Build Partner Portal and Partner-driven tenant provisioning first. Add affiliate system once Partner model is validated.

---

### R7. Demo Mode as Permanent vs. Temporary

**What:** Constitution declares "No Demo Mode" invariant but entire system is built around demo accounts, demo tenants, and demo authentication policies.

**Why Reconsider:** Contradiction. Demo mode is clearly needed for onboarding and testing. But current implementation is not sandboxed or clearly marked.

**Fresh Decision Required:** Choose demo mode strategy. Options:

- **Option A:** Demo mode is permanent (acknowledge it, design it properly)
- **Option B:** Demo mode is temporary (remove all demo infrastructure before production)
- **Option C:** Demo mode is per-environment (demo mode in staging, no demo mode in production)

**Recommendation:** Option A (permanent). Design demo mode properly: sandboxed, clearly marked, with reset mechanisms. Update invariant to acknowledge demo mode.

---

### R8. 4 Commerce Suites vs. 7 Industry Suites

**What:** Constitution documents 4 commerce suites (POS, ParkHub, SVM, MVM). Marketing site promises 7 industry suites (commerce, education, health, civic, hospitality, logistics).

**Why Reconsider:** Massive scope gap. Building 7 industry suites requires 7× the resources. Unclear if all 7 are needed for Phase 1.

**Fresh Decision Required:** Choose industry suite strategy. Options:

- **Option A:** Focus on commerce only (abandon multi-industry vision)
- **Option B:** Build all 7 industry suites (accept massive scope increase)
- **Option C:** Build composable primitives + industry-specific configurations (let Partners configure workflows)
- **Option D:** Phase the vision (commerce in Phase 1, education/health in Phase 2, etc.)

**Recommendation:** Option C + D (composable primitives + phased rollout). Build CRM, Automation, Communication, Forms, Calendar as composable primitives. Let Partners configure industry-specific workflows. Phase industry suites: Commerce (Phase 1), Education (Phase 2), Health (Phase 3), etc.

---

### R9. Super Admin vs. Partner Tenant Provisioning

**What:** Constitution states only Super Admins can create tenants. Marketing site states Partners create client organizations.

**Why Reconsider:** Authorization mismatch. Partner-centric model requires Partners to control tenant provisioning.

**Fresh Decision Required:** Choose tenant provisioning model. Options:

- **Option A:** Only Super Admins can create tenants (maintain current model)
- **Option B:** Partners can create tenants (enable self-service provisioning)
- **Option C:** Hybrid (Super Admins create Partners, Partners create Tenants)

**Recommendation:** Option B (Partners can create tenants). This aligns with the partner-centric model. Partners are first-class platform operators.

---

### R10. Single Domain vs. Partner-Specific Custom Domains

**What:** Constitution declares `.com` as primary domain. Marketing site implies Partners can use their own domains ("Your brand, your pricing").

**Why Reconsider:** White-label resale model requires Partners to use their own domains (e.g., `partner-name.com`).

**Fresh Decision Required:** Choose domain strategy. Options:

- **Option A:** Single domain (webwaka.com) for all Partners and Tenants
- **Option B:** Partner-specific custom domains (e.g., `partner-name.com`)
- **Option C:** Hybrid (webwaka.com by default, custom domains optional)

**Recommendation:** Option C (hybrid). Support custom domains for Partners who want full white-label branding. Default to `webwaka.com` for Partners who don't need custom domains.

---

## Summary: Triage Statistics

| Category | Count |
|----------|-------|
| KEEP (Validated) | 18 |
| DISCARD (Mistakes) | 8 |
| RECONSIDER (Fresh Decisions) | 10 |
| **Total Ideas Evaluated** | **36** |

---

## Next Steps

This triage feeds into:

1. **Founder Decision Table** (Phase 4) — The 10 RECONSIDER items become Founder decisions
2. **Clean Architecture Proposal** (Phase 6) — The 18 KEEP items inform the target architecture
3. **Transition Plan** (Phase 9) — The 8 DISCARD items inform what to remove/refactor

---

**End of Idea Triage**
# 3. FOUNDER DECISION TABLE (CRITICAL)

**Analysis Date:** 2026-01-26  
**Analyst:** Manus Re-Founding Phase (WRF-0 Authoritative)

---

## ⚠️ GOVERNING CONSTRAINT

**All decisions in this table are constrained by the two Founder Non-Negotiable Directives:**

1. **AWS-First, Single-Bill Architecture**
2. **Design for Maximum Scale from Day One**

Any decision that conflicts with these directives is invalid and must be revised.

---

## Purpose

This table lists **15 high-leverage decisions** that only the Founder can make. These decisions are **blocking canonization** — the platform cannot proceed to rebuild until these decisions are finalized.

Each decision includes:

- **Decision Description:** What needs to be decided
- **Options:** Possible choices
- **Trade-offs:** Pros and cons of each option
- **Recommendation:** Analyst's recommendation (not binding)
- **Directive Constraint:** How AWS-First or Max-Scale-First constrains this decision
- **Impact if Delayed:** What happens if this decision is not made

---

## Decision 1: Platform Identity — Vertical SaaS vs. Meta-Platform

### Decision Description

What is WebWaka? Is it a vertical SaaS for Nigerian commerce, or a meta-platform for building SaaS platforms (GoHighLevel-class)?

### Directive Constraint

**Max-Scale-First:** This decision is **already constrained**. WebWaka is a **Platform for Building Platforms** (meta-platform), not a SaaS, not a narrow MVP, not a small-business tool.

### Options

| Option | Description | Status |
|--------|-------------|--------|
| **A: Vertical SaaS** | WebWaka is a commerce platform with POS, inventory, and marketplaces for Nigerian merchants. | ❌ **INVALID** (conflicts with Max-Scale-First) |
| **B: Meta-Platform** | WebWaka is infrastructure for building SaaS platforms. Partners build their own branded platforms on WebWaka. | ✅ **VALID** (aligns with Max-Scale-First) |
| **C: Hybrid** | WebWaka provides composable primitives (CRM, Automation, Communication) + industry-specific configurations. | ✅ **VALID** (aligns with Max-Scale-First) |

### Recommendation

**Option C (Hybrid).** Build composable primitives (CRM, Automation, Communication, Forms, Calendar) and let Partners configure industry-specific workflows. This enables multi-industry support without building 7 separate platforms.

### Founder Decision

**[ CONSTRAINED BY MAX-SCALE-FIRST: Option A is invalid. Choose B or C. ]**

---

## Decision 2: Target User — End Users vs. Partners

### Decision Description

Who is the primary target user? End users (merchants, schools, clinics) or Partners (platform operators who resell to end users)?

### Directive Constraint

**Max-Scale-First:** This decision is **already constrained**. WebWaka is designed for **thousands of Partners**, not end users.

### Options

| Option | Description | Status |
|--------|-------------|--------|
| **A: End Users** | WebWaka sells directly to merchants, schools, clinics, etc. | ❌ **INVALID** (conflicts with Max-Scale-First) |
| **B: Partners** | WebWaka sells to Partners who build their own SaaS businesses on WebWaka infrastructure. | ✅ **VALID** (aligns with Max-Scale-First) |
| **C: Hybrid** | WebWaka sells to both end users and Partners. | ⚠️ **RISKY** (channel conflict, confusing positioning) |

### Recommendation

**Option B (Partners).** This aligns with the GoHighLevel-class meta-platform vision. Partners are first-class platform operators who brand, price, and resell SaaS.

### Founder Decision

**[ CONSTRAINED BY MAX-SCALE-FIRST: Option A is invalid. Choose B or C. ]**

---

## Decision 3: Architecture Model — Tenant-Centric vs. Partner-Centric

### Decision Description

Is the platform tenant-centric (tenants are the primary entity) or partner-centric (Partners own and manage tenants)?

### Directive Constraint

**Max-Scale-First:** This decision is **already constrained**. Partner-Centric architecture is required to support **thousands of Partners** and **millions of Tenants**.

### Options

| Option | Description | Status |
|--------|-------------|--------|
| **A: Tenant-Centric** | Tenants are the primary entity. Partners are metadata. Super Admins control tenant provisioning. | ❌ **INVALID** (conflicts with Max-Scale-First) |
| **B: Partner-Centric** | Partners are the primary entity. Partners control tenant provisioning, billing, and lifecycle. | ✅ **VALID** (aligns with Max-Scale-First) |

### Recommendation

**Option B (Partner-Centric).** This aligns with the meta-platform vision. Partners are first-class platform operators.

### Founder Decision

**[ CONSTRAINED BY MAX-SCALE-FIRST: Option A is invalid. Choose B. ]**

---

## Decision 4: Industry Scope — Commerce Only vs. Multi-Industry

### Decision Description

Should WebWaka focus on commerce only (POS, inventory, marketplaces) or support multiple industries (commerce, education, health, civic, hospitality, logistics)?

### Directive Constraint

**Max-Scale-First:** This decision is **already constrained**. WebWaka must support **industry-agnostic modules** and **composable primitives**, not commerce-only.

### Options

| Option | Description | Status |
|--------|-------------|--------|
| **A: Commerce Only** | Focus on 4 commerce suites (POS, ParkHub, SVM, MVM). Abandon multi-industry vision. | ❌ **INVALID** (conflicts with Max-Scale-First) |
| **B: Multi-Industry (All 7)** | Build all 7 industry suites (commerce, education, health, civic, hospitality, logistics). | ✅ **VALID** (aligns with Max-Scale-First) |
| **C: Composable Primitives** | Build industry-agnostic primitives (CRM, Automation, Communication). Let Partners configure workflows. | ✅ **VALID** (aligns with Max-Scale-First) |
| **D: Phased Rollout** | Commerce in Phase 1. Education in Phase 2. Health in Phase 3. Etc. | ✅ **VALID** (implementation sequencing, not architectural limitation) |

### Recommendation

**Option C + D (Composable Primitives + Phased Rollout).** Build CRM, Automation, Communication, Forms, Calendar as composable primitives. Let Partners configure industry-specific workflows. Phase industry suites: Commerce (Phase 1), Education (Phase 2), Health (Phase 3), etc.

**Note:** This is **implementation sequencing**, not architectural limitation. Architecture is designed for all 7 industries from day one.

### Founder Decision

**[ CONSTRAINED BY MAX-SCALE-FIRST: Option A is invalid. Choose B, C, or D. ]**

---

## Decision 5: Affiliate System Priority — Core vs. Deferred

### Decision Description

Is the multi-level affiliate system (Partner → Sub-Partner → Agent → Merchant) a core feature (build in Phase 1) or deferred (build in Phase 2+)?

### Directive Constraint

**Max-Scale-First:** Architecture must support **multi-level affiliate trees with unlimited depth** from day one. Implementation can be phased.

### Options

| Option | Description | Status |
|--------|-------------|--------|
| **A: Core (Phase 1)** | Build affiliate system in Phase 1. Required for platform launch. | ✅ **VALID** (aligns with Max-Scale-First) |
| **B: Deferred (Phase 2+)** | Build Partner Portal first. Add affiliate system once Partner model is validated. | ✅ **VALID** (implementation sequencing, not architectural limitation) |
| **C: Optional** | Let Partners handle their own affiliate programs. WebWaka does not provide affiliate system. | ❌ **INVALID** (conflicts with Max-Scale-First) |

### Recommendation

**Option B (Deferred to Phase 2).** Build Partner Portal and Partner-driven tenant provisioning first. Add affiliate system once Partner model is validated.

**Note:** This is **implementation sequencing**, not architectural limitation. Data model and architecture support affiliate system from day one.

### Founder Decision

**[ CONSTRAINED BY MAX-SCALE-FIRST: Option C is invalid. Choose A or B. ]**

---

## Decision 6: Tenant Provisioning — Super Admin Only vs. Partner Self-Service

### Decision Description

Who can create tenants? Only Super Admins (current model) or Partners (self-service provisioning)?

### Directive Constraint

**Max-Scale-First:** Partner Self-Service is required to support **thousands of Partners** and **millions of Tenants**. Super Admin-only provisioning is a bottleneck.

### Options

| Option | Description | Status |
|--------|-------------|--------|
| **A: Super Admin Only** | Only Super Admins can create tenants. Partners must request tenant creation. | ❌ **INVALID** (conflicts with Max-Scale-First) |
| **B: Partner Self-Service** | Partners can create tenants themselves. No Super Admin approval required. | ✅ **VALID** (aligns with Max-Scale-First) |
| **C: Hybrid** | Partners can create tenants, but Super Admins can approve/reject. | ⚠️ **RISKY** (approval workflow is a bottleneck) |

### Recommendation

**Option B (Partner Self-Service).** This aligns with the partner-centric model. Partners are first-class platform operators. Implement fraud prevention (rate limiting, manual review for high-volume Partners).

### Founder Decision

**[ CONSTRAINED BY MAX-SCALE-FIRST: Option A is invalid. Choose B or C. ]**

---

## Decision 7: Custom Domains — Single Domain vs. Partner-Specific Domains

### Decision Description

Should all Tenants use a single domain (e.g., `app.webwaka.com`) or should Partners have their own custom domains (e.g., `app.partner.com`)?

### Directive Constraint

**Max-Scale-First:** Partner-specific custom domains are required for **white-labeled SaaS resale**. Partners must be able to brand their platforms.

### Options

| Option | Description | Status |
|--------|-------------|--------|
| **A: Single Domain** | All Tenants use `app.webwaka.com`. No custom domains. | ❌ **INVALID** (conflicts with Max-Scale-First) |
| **B: Partner-Specific Domains** | Partners can use their own custom domains (e.g., `app.partner.com`). | ✅ **VALID** (aligns with Max-Scale-First) |
| **C: Hybrid** | Default domain is `app.webwaka.com`. Partners can optionally use custom domains. | ✅ **VALID** (aligns with Max-Scale-First) |

### Recommendation

**Option B (Partner-Specific Domains).** This aligns with the white-labeled SaaS resale model. Partners must be able to brand their platforms.

**Implementation:** Use AWS CloudFront + ACM for custom domain SSL certificates. Partners configure DNS CNAME records to point to CloudFront distribution.

### Founder Decision

**[ CONSTRAINED BY MAX-SCALE-FIRST: Option A is invalid. Choose B or C. ]**

---

## Decision 8: Demo Mode Strategy — Permanent vs. Temporary

### Decision Description

Should demo mode be a permanent feature (always available) or a temporary feature (removed after onboarding)?

### Directive Constraint

**None.** This decision is not constrained by AWS-First or Max-Scale-First.

### Options

| Option | Description |
|--------|-------------|
| **A: Permanent** | Demo mode is always available. Users can explore the platform without creating an account. |
| **B: Temporary** | Demo mode is available during onboarding. Removed after user creates an account. |
| **C: Optional** | Partners can enable/disable demo mode for their Tenants. |

### Recommendation

**Option C (Optional).** Let Partners decide whether to enable demo mode for their Tenants. This aligns with the partner-centric model.

### Founder Decision

**[ NOT CONSTRAINED. Choose A, B, or C. ]**

---

## Decision 9: Offline-First Definition — 24-Hour vs. Indefinite

### Decision Description

What does "offline-first" mean for POS and ParkHub? Can they function offline for 24 hours, or indefinitely?

### Directive Constraint

**Max-Scale-First:** Offline-first must be designed for **worst-case network conditions** (rural Nigeria, power outages, etc.). 24-hour offline is insufficient.

### Options

| Option | Description | Status |
|--------|-------------|--------|
| **A: 24-Hour Offline** | POS and ParkHub can function offline for 24 hours. After 24 hours, they require internet connectivity. | ⚠️ **RISKY** (insufficient for rural Nigeria) |
| **B: Indefinite Offline** | POS and ParkHub can function offline indefinitely. Sync when internet is available. | ✅ **VALID** (aligns with Max-Scale-First) |
| **C: Configurable** | Partners can configure offline duration per Tenant (24 hours, 7 days, indefinite). | ✅ **VALID** (aligns with Max-Scale-First) |

### Recommendation

**Option B (Indefinite Offline).** POS and ParkHub must function offline indefinitely. Sync when internet is available. This aligns with the Nigerian market reality (power outages, poor network connectivity).

### Founder Decision

**[ CONSTRAINED BY MAX-SCALE-FIRST: Option A is risky. Choose B or C. ]**

---

## Decision 10: Modularity Implementation — Monolithic vs. Microservices

### Decision Description

Should the Core API be monolithic (all domains in one codebase) or microservices (each domain is a separate service)?

### Directive Constraint

**Max-Scale-First:** Architecture must support **independent scaling** of high-load domains (CRM, Automation, Communication). Monolithic is acceptable for Phase 1, but architecture must support microservices migration.

### Options

| Option | Description | Status |
|--------|-------------|--------|
| **A: Monolithic (Phase 1)** | All domains in one codebase. Simpler to build and deploy. | ✅ **VALID** (implementation sequencing, not architectural limitation) |
| **B: Microservices (Day 1)** | Each domain is a separate service. Independent scaling and deployment. | ✅ **VALID** (aligns with Max-Scale-First) |
| **C: Hybrid** | Core domains (Identity, Tenants, Audit) are monolithic. High-load domains (CRM, Automation, Communication) are microservices. | ✅ **VALID** (aligns with Max-Scale-First) |

### Recommendation

**Option A → C (Monolithic Phase 1, Hybrid Phase 2).** Start with monolithic Core API for Phase 1. Migrate high-load domains (CRM, Automation, Communication) to microservices in Phase 2.

**Note:** This is **implementation sequencing**, not architectural limitation. Architecture supports microservices from day one (clear domain boundaries, event-driven communication).

### Founder Decision

**[ NOT CONSTRAINED. Choose A, B, or C. ]**

---

## Decision 11: Database Provider — AWS RDS vs. Neon vs. Aurora

### Decision Description

Which database provider should WebWaka use?

### Directive Constraint

**AWS-First:** AWS RDS or Aurora are preferred over Neon. Neon is a third-party SaaS. AWS RDS/Aurora provide single-bill consolidation.

### Options

| Option | Description | Status |
|--------|-------------|--------|
| **A: Neon** | Serverless PostgreSQL (third-party SaaS). | ❌ **INVALID** (conflicts with AWS-First) |
| **B: AWS RDS (PostgreSQL)** | Managed PostgreSQL on AWS. | ✅ **VALID** (aligns with AWS-First) |
| **C: AWS Aurora (PostgreSQL-compatible)** | Serverless, auto-scaling, multi-region PostgreSQL. | ✅ **VALID** (aligns with AWS-First and Max-Scale-First) |

### Recommendation

**Option C (AWS Aurora).** Aurora is serverless, auto-scaling, and supports multi-region replication. This aligns with both AWS-First and Max-Scale-First.

**Trade-off:** Aurora is more expensive than RDS, but provides better scalability and availability.

### Founder Decision

**[ CONSTRAINED BY AWS-FIRST: Option A is invalid. Choose B or C. ]**

---

## Decision 12: Authentication Provider — AWS Cognito vs. Clerk

### Decision Description

Which authentication provider should WebWaka use?

### Directive Constraint

**AWS-First:** AWS Cognito is preferred over Clerk. Clerk is a third-party SaaS. AWS Cognito provides single-bill consolidation.

### Options

| Option | Description | Status |
|--------|-------------|--------|
| **A: Clerk** | Third-party authentication SaaS. | ❌ **INVALID** (conflicts with AWS-First) |
| **B: AWS Cognito** | AWS-native authentication service. | ✅ **VALID** (aligns with AWS-First) |
| **C: Custom (Auth0, Supabase Auth)** | Third-party authentication SaaS. | ❌ **INVALID** (conflicts with AWS-First) |

### Recommendation

**Option B (AWS Cognito).** This aligns with AWS-First directive.

**Trade-off:** Cognito has a steeper learning curve than Clerk, but provides single-bill consolidation and deeper AWS integration.

**Justification Required:** If Clerk is chosen, must justify why Cognito is insufficient.

### Founder Decision

**[ CONSTRAINED BY AWS-FIRST: Options A and C are invalid unless justified. Choose B or justify A/C. ]**

---

## Decision 13: Dev/Prod Infrastructure — Shared vs. Isolated

### Decision Description

Should development and production infrastructure be shared (same database, same Cognito, same ECS cluster) or isolated (separate database, separate Cognito, separate ECS cluster)?

### Directive Constraint

**Max-Scale-First:** Isolated dev/prod infrastructure is required to prevent blast-radius risk. Development changes must not affect production data.

### Options

| Option | Description | Status |
|--------|-------------|--------|
| **A: Shared** | Development and production share the same infrastructure. | ❌ **INVALID** (high blast-radius risk) |
| **B: Isolated** | Development and production have separate infrastructure. | ✅ **VALID** (aligns with Max-Scale-First) |

### Recommendation

**Option B (Isolated).** This eliminates blast-radius risk. Development changes cannot affect production data.

**Implementation:** Use separate AWS accounts for dev and prod (AWS Organizations).

### Founder Decision

**[ CONSTRAINED BY MAX-SCALE-FIRST: Option A is invalid. Choose B. ]**

---

## Decision 14: Repository Structure — Monorepo vs. Multi-Repo

### Decision Description

Should WebWaka use a monorepo (all code in one repository) or multi-repo (each app/service in a separate repository)?

### Directive Constraint

**None.** This decision is not constrained by AWS-First or Max-Scale-First.

### Options

| Option | Description |
|--------|-------------|
| **A: Monorepo** | All code in one repository. Simpler to build and iterate. Requires monorepo tooling (Turborepo, Nx). |
| **B: Multi-Repo** | Each app/service in a separate repository. Independent versioning and deployment. |

### Recommendation

**Option A (Monorepo for Phase 1).** Monorepo is simpler to build and iterate. Suitable for Phase 1. Can migrate to multi-repo in Phase 2+ if needed.

### Founder Decision

**[ NOT CONSTRAINED. Choose A or B. ]**

---

## Decision 15: Operator Model — Manus Only vs. Multi-Operator

### Decision Description

Should Manus be the sole operator for Phase 1, or should Emergent and Replit be used for specialized tasks?

### Directive Constraint

**None.** This decision is not constrained by AWS-First or Max-Scale-First.

### Options

| Option | Description |
|--------|-------------|
| **A: Manus Only** | Manus is the sole operator for Phase 1. Simpler workflow. |
| **B: Multi-Operator** | Emergent (backend), Replit (code audits), Manus (coordination). |

### Recommendation

**Option A (Manus Only for Phase 1).** The previous Constitution defined a complex multi-operator model that was not being followed in practice. Start simple. Add multi-operator model in Phase 2+ if needed.

### Founder Decision

**[ NOT CONSTRAINED. Choose A or B. ]**

---

## Summary: Constrained Decisions

| Decision | Constrained By | Valid Options |
|----------|----------------|---------------|
| **Decision 1: Platform Identity** | Max-Scale-First | B (Meta-Platform) or C (Hybrid) |
| **Decision 2: Target User** | Max-Scale-First | B (Partners) or C (Hybrid) |
| **Decision 3: Architecture Model** | Max-Scale-First | B (Partner-Centric) |
| **Decision 4: Industry Scope** | Max-Scale-First | B, C, or D (Multi-Industry or Composable Primitives) |
| **Decision 5: Affiliate System Priority** | Max-Scale-First | A (Core) or B (Deferred) |
| **Decision 6: Tenant Provisioning** | Max-Scale-First | B (Partner Self-Service) or C (Hybrid) |
| **Decision 7: Custom Domains** | Max-Scale-First | B (Partner-Specific) or C (Hybrid) |
| **Decision 9: Offline-First Definition** | Max-Scale-First | B (Indefinite) or C (Configurable) |
| **Decision 11: Database Provider** | AWS-First | B (AWS RDS) or C (AWS Aurora) |
| **Decision 12: Authentication Provider** | AWS-First | B (AWS Cognito) or justify A (Clerk) |
| **Decision 13: Dev/Prod Infrastructure** | Max-Scale-First | B (Isolated) |

---

**End of Founder Decision Table**
# 4. TOOLING & PLATFORM RE-EVALUATION (AWS-FIRST)

**Analysis Date:** 2026-01-26  
**Analyst:** Manus Re-Founding Phase (WRF-0 Authoritative)

---

## ⚠️ GOVERNING CONSTRAINT

**All tooling decisions are constrained by the Founder Non-Negotiable Directive:**

**AWS-First, Single-Bill Architecture**

- **Prefer AWS-native services** over third-party SaaS wherever viable
- **Third-party tools must justify** why AWS-native options are insufficient
- **Manus has access to AWS accounts** for setup and configuration (complexity is not a blocker)

---

## Purpose

This section provides a **ground-up reassessment** of all tools, platforms, and services currently in use or planned for WebWaka. Every tool is evaluated based on:

- **Current Status:** What is currently in use or planned
- **AWS-Native Alternative:** What AWS service provides equivalent functionality
- **Evaluation:** Pros, cons, and suitability for the GoHighLevel-class meta-platform vision
- **Recommendation:** Keep, replace, or defer
- **Justification:** Why this recommendation is made (with explicit AWS-First consideration)

**No tool is carried forward by default.** Every tool must be explicitly re-justified under AWS-First constraint.

---

## Category 1: Authentication & Identity

### Tool: Clerk (Third-Party SaaS)

**Current Status:** In use. Provides user authentication and session management.

**AWS-Native Alternative:** **AWS Cognito**

**Evaluation:**

| Aspect | Clerk | AWS Cognito |
|--------|-------|-------------|
| **Pros** | Excellent developer experience. SSO, MFA, session management out of the box. | AWS-native. Single-bill consolidation. Proven at scale. Integrates with IAM, Lambda, API Gateway. |
| **Cons** | Vendor lock-in. Pricing may increase. No control over auth infrastructure. | Steeper learning curve. More complex setup. Less polished developer experience. |
| **Suitability for Meta-Platform** | Suitable. Supports multi-tenant authentication. | Suitable. Supports multi-tenant authentication via User Pools and Identity Pools. |

**Recommendation:** **REPLACE with AWS Cognito** (aligns with AWS-First directive).

**Justification:** AWS Cognito provides equivalent functionality to Clerk and aligns with AWS-First directive. While Clerk has better developer experience, AWS Cognito provides single-bill consolidation and deeper AWS integration.

**Migration Path:** Clerk → AWS Cognito migration is straightforward. User data can be exported from Clerk and imported to Cognito via Lambda triggers.

---

## Category 2: Database & Data Storage

### Tool: PostgreSQL (via Neon, Third-Party SaaS)

**Current Status:** In use. Neon provides serverless PostgreSQL.

**AWS-Native Alternative:** **AWS RDS (PostgreSQL)** or **AWS Aurora (PostgreSQL-compatible)**

**Evaluation:**

| Aspect | Neon | AWS RDS | AWS Aurora |
|--------|------|---------|------------|
| **Pros** | Serverless. Low-friction developer experience. Good for multi-tenant platforms. | Enterprise-grade. Proven at scale. Single-bill consolidation. | Serverless. Auto-scaling. Multi-region replication. Single-bill consolidation. |
| **Cons** | Vendor lock-in. Unclear long-term stability, pricing, and support. | Higher operational overhead. More expensive. | More expensive than RDS. |
| **Suitability for Meta-Platform** | Suitable. PostgreSQL is proven at scale. | Suitable. PostgreSQL is proven at scale. | Highly suitable. Auto-scaling and multi-region replication align with Max-Scale-First. |

**Recommendation:** **REPLACE with AWS Aurora** (aligns with AWS-First and Max-Scale-First directives).

**Justification:** AWS Aurora provides serverless, auto-scaling, and multi-region replication. This aligns with both AWS-First (single-bill consolidation) and Max-Scale-First (designed for thousands of Partners and millions of Tenants).

**Migration Path:** Neon → AWS Aurora migration is straightforward. PostgreSQL dump/restore or AWS Database Migration Service (DMS).

---

### Tool: Prisma (ORM)

**Current Status:** In use. Provides type-safe database access.

**AWS-Native Alternative:** None (ORMs are application-level, not infrastructure-level).

**Recommendation:** **KEEP** (no AWS-native alternative).

**Justification:** Prisma is working well. No AWS-native alternative exists. The type safety benefits outweigh the vendor lock-in risk.

---

## Category 3: Deployment & Infrastructure

### Tool: Fly.io (Third-Party SaaS)

**Current Status:** In use. Provides container hosting.

**AWS-Native Alternative:** **AWS ECS (Elastic Container Service)** or **AWS Fargate**

**Evaluation:**

| Aspect | Fly.io | AWS ECS | AWS Fargate |
|--------|--------|---------|-------------|
| **Pros** | Simple deployment. Good developer experience. Global edge network. | Enterprise-grade. Proven at scale. Single-bill consolidation. Integrates with VPC, IAM, CloudWatch. | Serverless containers. No cluster management. Single-bill consolidation. |
| **Cons** | Vendor lock-in. Unclear long-term stability, pricing, and support. | Higher operational overhead. Requires cluster management. | More expensive than ECS. |
| **Suitability for Meta-Platform** | Suitable. Proven at scale. | Highly suitable. Proven at scale. | Highly suitable. Serverless aligns with Max-Scale-First. |

**Recommendation:** **REPLACE with AWS Fargate** (aligns with AWS-First directive).

**Justification:** AWS Fargate provides serverless container hosting and aligns with AWS-First directive. While Fly.io has better developer experience, AWS Fargate provides single-bill consolidation and deeper AWS integration.

**Migration Path:** Fly.io → AWS Fargate migration requires Dockerfile and ECS task definition. Manus can configure this.

---

### Tool: Vercel (Third-Party SaaS)

**Current Status:** In use. Provides frontend hosting (Next.js apps).

**AWS-Native Alternative:** **AWS Amplify** or **AWS S3 + CloudFront**

**Evaluation:**

| Aspect | Vercel | AWS Amplify | AWS S3 + CloudFront |
|--------|--------|-------------|---------------------|
| **Pros** | Excellent developer experience. Automatic deployments. Preview deployments. | AWS-native. Single-bill consolidation. Integrates with CodeCommit, CodeBuild, CodePipeline. | AWS-native. Single-bill consolidation. Full control. |
| **Cons** | Vendor lock-in. Pricing may increase. No control over hosting infrastructure. | Less polished developer experience. More complex setup. | Requires manual CI/CD setup. No automatic preview deployments. |
| **Suitability for Meta-Platform** | Suitable. Proven at scale. | Suitable. Proven at scale. | Suitable. Proven at scale. |

**Recommendation:** **REPLACE with AWS Amplify** (aligns with AWS-First directive).

**Justification:** AWS Amplify provides equivalent functionality to Vercel and aligns with AWS-First directive. While Vercel has better developer experience, AWS Amplify provides single-bill consolidation and deeper AWS integration.

**Migration Path:** Vercel → AWS Amplify migration is straightforward. Connect GitHub repository to Amplify and configure build settings.

---

## Category 4: Email & Communication

### Tool: Resend (Third-Party SaaS, Planned)

**Current Status:** Planned for email campaigns.

**AWS-Native Alternative:** **AWS SES (Simple Email Service)**

**Evaluation:**

| Aspect | Resend | AWS SES |
|--------|--------|---------|
| **Pros** | Excellent developer experience. Modern API. Good deliverability. | AWS-native. Single-bill consolidation. Proven at scale. Low cost. |
| **Cons** | Vendor lock-in. Pricing may increase. No control over email infrastructure. | Steeper learning curve. Requires domain verification. More complex setup. |
| **Suitability for Meta-Platform** | Suitable. Good for transactional and marketing emails. | Highly suitable. Proven at scale. Supports bulk email sending. |

**Recommendation:** **USE AWS SES** (aligns with AWS-First directive).

**Justification:** AWS SES provides equivalent functionality to Resend and aligns with AWS-First directive. While Resend has better developer experience, AWS SES provides single-bill consolidation and lower cost.

**Implementation:** Use AWS SES + AWS SES Configuration Sets for bounce/complaint handling.

---

### Tool: Africa's Talking (Third-Party SaaS, Planned)

**Current Status:** Planned for SMS and WhatsApp.

**AWS-Native Alternative:** **AWS SNS (Simple Notification Service)** for SMS. No AWS-native alternative for WhatsApp.

**Evaluation:**

| Aspect | Africa's Talking | AWS SNS |
|--------|------------------|---------|
| **Pros** | Africa-focused. Supports SMS, WhatsApp, Voice. Good for Nigerian market. | AWS-native. Single-bill consolidation. Proven at scale. |
| **Cons** | Vendor lock-in. Pricing may increase. No AWS integration. | Does not support WhatsApp. Limited SMS coverage in Africa. |
| **Suitability for Meta-Platform** | Highly suitable. Africa-focused. Supports SMS and WhatsApp. | Partially suitable. Supports SMS, but not WhatsApp. |

**Recommendation:** **USE AWS SNS for SMS. USE Africa's Talking for WhatsApp** (hybrid approach).

**Justification:** AWS SNS aligns with AWS-First directive for SMS. Africa's Talking is retained for WhatsApp because AWS does not provide WhatsApp messaging. This is a justified exception to AWS-First.

**Implementation:** Use AWS SNS for SMS campaigns. Use Africa's Talking API for WhatsApp messaging.

---

## Category 5: Queues & Events

### Tool: None (Not Yet Implemented)

**Current Status:** Not yet implemented.

**AWS-Native Alternative:** **AWS SQS (Simple Queue Service)** for queues. **AWS EventBridge** for events.

**Recommendation:** **USE AWS SQS and AWS EventBridge** (aligns with AWS-First directive).

**Justification:** AWS SQS and EventBridge are AWS-native and provide single-bill consolidation. No third-party alternative is needed.

**Implementation:** Use AWS SQS for background jobs (e.g., email sending, data sync). Use AWS EventBridge for event-driven workflows (e.g., "Contact created" → "Send welcome email").

---

## Category 6: Storage

### Tool: None (Not Yet Implemented)

**Current Status:** Not yet implemented.

**AWS-Native Alternative:** **AWS S3 (Simple Storage Service)**

**Recommendation:** **USE AWS S3** (aligns with AWS-First directive).

**Justification:** AWS S3 is AWS-native, proven at scale, and provides single-bill consolidation. No third-party alternative is needed.

**Implementation:** Use AWS S3 for file uploads (receipts, invoices, images). Use AWS CloudFront for CDN.

---

## Category 7: Analytics

### Tool: PostHog (Third-Party SaaS, Planned)

**Current Status:** Planned for product analytics.

**AWS-Native Alternative:** **AWS CloudWatch + AWS Athena**

**Evaluation:**

| Aspect | PostHog | AWS CloudWatch + Athena |
|--------|---------|-------------------------|
| **Pros** | Excellent developer experience. Product analytics, session replay, feature flags. | AWS-native. Single-bill consolidation. Proven at scale. |
| **Cons** | Vendor lock-in. Pricing may increase. No control over analytics infrastructure. | Steeper learning curve. Requires custom dashboards. More complex setup. |
| **Suitability for Meta-Platform** | Suitable. Good for product analytics. | Suitable. Proven at scale. Supports custom dashboards. |

**Recommendation:** **USE AWS CloudWatch + Athena** (aligns with AWS-First directive).

**Justification:** AWS CloudWatch + Athena provide equivalent functionality to PostHog and align with AWS-First directive. While PostHog has better developer experience, AWS CloudWatch + Athena provide single-bill consolidation.

**Implementation:** Use AWS CloudWatch for logs and metrics. Use AWS Athena for querying logs and generating reports.

---

## Category 8: Background Jobs

### Tool: None (Not Yet Implemented)

**Current Status:** Not yet implemented.

**AWS-Native Alternative:** **AWS Lambda**

**Recommendation:** **USE AWS Lambda** (aligns with AWS-First directive).

**Justification:** AWS Lambda is AWS-native, serverless, and provides single-bill consolidation. No third-party alternative is needed.

**Implementation:** Use AWS Lambda for background jobs (e.g., email sending, data sync, invoice generation). Trigger Lambda functions via AWS SQS or AWS EventBridge.

---

## Category 9: Monitoring & Error Tracking

### Tool: Sentry (Third-Party SaaS, Planned)

**Current Status:** Planned for error tracking.

**AWS-Native Alternative:** **AWS CloudWatch Logs + AWS X-Ray**

**Evaluation:**

| Aspect | Sentry | AWS CloudWatch + X-Ray |
|--------|--------|------------------------|
| **Pros** | Excellent developer experience. Error tracking, performance monitoring, release tracking. | AWS-native. Single-bill consolidation. Proven at scale. |
| **Cons** | Vendor lock-in. Pricing may increase. No control over error tracking infrastructure. | Steeper learning curve. Less polished developer experience. More complex setup. |
| **Suitability for Meta-Platform** | Suitable. Good for error tracking. | Suitable. Proven at scale. Supports distributed tracing. |

**Recommendation:** **USE AWS CloudWatch + X-Ray** (aligns with AWS-First directive).

**Justification:** AWS CloudWatch + X-Ray provide equivalent functionality to Sentry and align with AWS-First directive. While Sentry has better developer experience, AWS CloudWatch + X-Ray provide single-bill consolidation.

**Implementation:** Use AWS CloudWatch Logs for error logs. Use AWS X-Ray for distributed tracing.

---

## Summary: AWS-First Tooling Recommendations

| Domain | Third-Party Tool | AWS-Native Alternative | Recommendation |
|--------|------------------|------------------------|----------------|
| **Authentication** | Clerk | AWS Cognito | **REPLACE with AWS Cognito** |
| **Database** | Neon (PostgreSQL) | AWS Aurora (PostgreSQL-compatible) | **REPLACE with AWS Aurora** |
| **ORM** | Prisma | None | **KEEP Prisma** |
| **Backend Hosting** | Fly.io | AWS Fargate | **REPLACE with AWS Fargate** |
| **Frontend Hosting** | Vercel | AWS Amplify | **REPLACE with AWS Amplify** |
| **Email** | Resend | AWS SES | **USE AWS SES** |
| **SMS** | Africa's Talking | AWS SNS | **USE AWS SNS** |
| **WhatsApp** | Africa's Talking | None | **USE Africa's Talking** (justified exception) |
| **Queues** | None | AWS SQS | **USE AWS SQS** |
| **Events** | None | AWS EventBridge | **USE AWS EventBridge** |
| **Storage** | None | AWS S3 | **USE AWS S3** |
| **Analytics** | PostHog | AWS CloudWatch + Athena | **USE AWS CloudWatch + Athena** |
| **Background Jobs** | None | AWS Lambda | **USE AWS Lambda** |
| **Error Tracking** | Sentry | AWS CloudWatch + X-Ray | **USE AWS CloudWatch + X-Ray** |

---

## Justified Exceptions to AWS-First

| Tool | Justification |
|------|---------------|
| **Prisma (ORM)** | No AWS-native alternative. Application-level tool, not infrastructure. |
| **Africa's Talking (WhatsApp)** | AWS does not provide WhatsApp messaging. Africa's Talking is required for Nigerian market. |

---

**End of Tooling & Platform Re-Evaluation (AWS-First)**
# 5. CLEAN PLATFORM ARCHITECTURE (TARGET STATE)

**Analysis Date:** 2026-01-26  
**Analyst:** Manus Re-Founding Phase (WRF-0 Authoritative)

---

## ⚠️ GOVERNING CONSTRAINTS

**This architecture is designed under the two Founder Non-Negotiable Directives:**

1. **AWS-First, Single-Bill Architecture**
2. **Design for Maximum Scale from Day One**

All architecture decisions align with these directives.

---

## Purpose

This section defines the **correct end-state architecture** for WebWaka as a **Platform for Building Platforms** (meta-platform).

This architecture is:

- **Clean:** No contradictions, no ambiguities, no unresolved tensions
- **Modular:** Clear boundaries between domains
- **Enforceable:** Authority boundaries are explicit and testable
- **Scalable:** Designed for **thousands of Partners** and **millions of Tenants**
- **AWS-Native:** Prefers AWS services for single-bill consolidation

---

## Architectural Principles

### Principle 1: Partner-Centric Hierarchy (Max-Scale-First)

**Partners own Tenants. Tenants do not exist independently.**

```
Super Admin
└── Partner (thousands)
    └── Tenant (millions)
        └── User (billions)
```

- **Super Admins** create and manage Partners
- **Partners** create and manage Tenants (self-service provisioning)
- **Tenants** create and manage Users
- **Users** perform actions within Tenants

**Scale Assumptions:**

- **1,000+ Partners** (platform operators)
- **1,000,000+ Tenants** (end-user organizations)
- **100,000,000+ Users** (end users)

### Principle 2: Composable Primitives (Max-Scale-First)

**The platform provides composable primitives, not vertical-specific features.**

Primitives:

- **CRM:** Contacts, Pipelines, Opportunities, Tasks
- **Automation:** Workflows, Triggers, Actions, Delays
- **Communication:** Email, SMS, WhatsApp, Unified Inbox
- **Forms:** Form Builder, Survey Builder, Embeddable Forms
- **Calendar:** Booking, Scheduling, Availability
- **Billing:** Subscriptions, Invoices, Payments, Revenue Sharing
- **Affiliate:** Multi-Level Hierarchy, Commission Tracking, Payouts

Partners configure these primitives into industry-specific workflows (commerce, education, health, civic, hospitality, logistics).

### Principle 3: Industry-Agnostic Core (Max-Scale-First)

**The Core API is industry-agnostic. Industry-specific logic lives in configurations, not code.**

- **Core API:** Provides primitives (CRM, Automation, Communication, etc.)
- **Industry Configurations:** Define workflows, templates, and defaults per industry
- **Partner Configurations:** Partners customize workflows for their clients

**This enables WebWaka to support all 7 industries (commerce, education, health, civic, hospitality, logistics) without building 7 separate platforms.**

### Principle 4: Offline-First for Commerce (Max-Scale-First)

**Commerce suites (POS, ParkHub) are offline-first. Other suites are online-only.**

- **POS:** Can function offline **indefinitely**. Queues transactions and syncs when online.
- **ParkHub:** Can function offline **indefinitely**. Queues transactions and syncs when online.
- **SVM, MVM, CRM, Automation, etc.:** Online-only. No offline functionality.

**Rationale:** Nigerian market reality (power outages, poor network connectivity). 24-hour offline is insufficient. Indefinite offline is required.

### Principle 5: Multi-Tenant Isolation (Max-Scale-First)

**All data is tenant-scoped. No cross-tenant data access.**

- Every database query that accesses tenant-specific data must include a `WHERE tenantId = ?` clause
- Tenant isolation is enforced at the database layer (row-level security via AWS Aurora)
- Audit logs track all cross-tenant access attempts

**Scale Assumptions:**

- **1,000,000+ Tenants** (each Tenant has isolated data)
- **100,000,000+ Users** (each User belongs to one Tenant)

---

## System Layers

### Layer 1: Core Infrastructure (AWS-Native)

**Purpose:** Foundational services shared across all Partners and Tenants.

**Components:**

| Component | AWS Service | Purpose |
|-----------|-------------|---------|
| **Identity & Authentication** | AWS Cognito | User authentication, session management, MFA |
| **Database** | AWS Aurora (PostgreSQL-compatible) | Multi-tenant data storage, auto-scaling, multi-region replication |
| **Audit Logs** | AWS CloudWatch Logs | Immutable audit trail for all write operations |
| **Feature Flags** | AWS AppConfig | Runtime feature toggles per Tenant |
| **Entitlements** | Custom (Core API) | Capability gating per Tenant |
| **Roles & Permissions** | Custom (Core API) | RBAC for Users, Tenants, Partners, Super Admins |
| **Storage** | AWS S3 | File uploads (receipts, invoices, images) |
| **CDN** | AWS CloudFront | Content delivery for static assets |

**Authority:** Super Admins control Core Infrastructure.

**Scale Assumptions:**

- **1,000+ Partners** (each Partner has isolated Cognito User Pool or User Pool Group)
- **1,000,000+ Tenants** (each Tenant has isolated data in Aurora)
- **100,000,000+ Users** (each User has Cognito identity)

---

### Layer 2: Core Domains (Composable Primitives)

**Purpose:** Industry-agnostic primitives that Partners configure into workflows.

**Domains:**

| Domain | Purpose | AWS Services | Phase |
|--------|---------|--------------|-------|
| **CRM** | Contacts, Pipelines, Opportunities, Tasks | Aurora, Lambda, EventBridge | Phase 2 |
| **Automation** | Workflows, Triggers, Actions, Delays | Lambda, Step Functions, EventBridge | Phase 2 |
| **Communication** | Email, SMS, WhatsApp, Unified Inbox | SES, SNS, Africa's Talking API | Phase 2 |
| **Forms** | Form Builder, Survey Builder, Embeddable Forms | Aurora, Lambda, S3 | Phase 3 |
| **Calendar** | Booking, Scheduling, Availability | Aurora, Lambda, EventBridge | Phase 3 |
| **Billing** | Subscriptions, Invoices, Payments, Revenue Sharing | Aurora, Lambda, Stripe API | Phase 2 |
| **Affiliate** | Multi-Level Hierarchy, Commission Tracking, Payouts | Aurora, Lambda, Stripe Connect | Phase 2 |
| **API Gateway** | REST API, Webhooks, API Keys, Rate Limiting | API Gateway, Lambda | Phase 2 |
| **Analytics** | Dashboard, Reports, Attribution | CloudWatch, Athena, QuickSight | Phase 2 |

**Authority:** Core Domains are controlled by Core API. Partners cannot modify Core Domain logic, only configure it.

**Scale Assumptions:**

- **1,000,000+ Contacts** (CRM)
- **10,000,000+ Workflows** (Automation)
- **100,000,000+ Emails/SMS** (Communication)

---

### Layer 3: Commerce Suites (Offline-First)

**Purpose:** Offline-first commerce capabilities for Nigerian market.

**Suites:**

| Suite | Purpose | AWS Services | Phase |
|-------|---------|--------------|-------|
| **POS** | Point of Sale (offline-first, IndexedDB, sync) | Aurora, Lambda, S3, EventBridge | Phase 1 |
| **ParkHub** | Transport Management (offline-first, IndexedDB, sync) | Aurora, Lambda, S3, EventBridge | Phase 1 |
| **SVM** | Single Vendor Marketplace (online-only) | Aurora, Lambda, S3, CloudFront | Phase 1 |
| **MVM** | Multi Vendor Marketplace (online-only) | Aurora, Lambda, S3, CloudFront | Phase 1 |

**Authority:** Commerce Suites are controlled by Core API. Tenants use Commerce Suites, but cannot modify them.

**Scale Assumptions:**

- **100,000+ POS Terminals** (each POS can function offline indefinitely)
- **10,000+ ParkHub Terminals** (each ParkHub can function offline indefinitely)
- **10,000+ SVM Storefronts** (online-only)
- **1,000+ MVM Marketplaces** (online-only)

---

### Layer 4: Partner Portal (AWS-Native)

**Purpose:** Partner-facing dashboard for managing Tenants, billing, and analytics.

**Features:**

- **Client Organization Creation:** Partners create Tenants (self-service provisioning)
- **Industry Suite Configuration:** Partners select and configure suites per Tenant
- **Capability Activation:** Partners activate/deactivate capabilities per Tenant
- **Billing & Revenue Sharing:** Partners view revenue, commissions, and payouts
- **Analytics Dashboard:** Partners view Tenant analytics and reports
- **Affiliate Dashboard:** Partners track referrals, commissions, and payouts (Phase 2)
- **Marketing Assets:** Partners access banners, copy, and creative assets (Phase 2)
- **Pricing Configuration:** Partners set custom pricing for their Tenants (Phase 2)

**AWS Services:**

- **Frontend:** AWS Amplify (Next.js app)
- **Backend:** AWS Fargate (Core API)
- **Database:** AWS Aurora
- **Analytics:** AWS CloudWatch + Athena + QuickSight

**Authority:** Partners control their own Tenants. Partners cannot access other Partners' Tenants.

**Scale Assumptions:**

- **1,000+ Partners** (each Partner manages 100-10,000 Tenants)

---

### Layer 5: Industry Configurations

**Purpose:** Pre-configured workflows, templates, and defaults per industry.

**Industries:**

| Industry | Suites | Capabilities | Phase |
|----------|--------|--------------|-------|
| **Commerce** | POS, ParkHub, SVM, MVM | Inventory, Payments, Receipts | Phase 1 |
| **Education** | LMS, Grading, Fees | CRM, Forms, Calendar, Billing | Phase 2 |
| **Health** | Clinic, Pharmacy, Patient Records | CRM, Calendar, Billing | Phase 3 |
| **Civic** | Community Finance, Cooperatives | CRM, Billing | Phase 3 |
| **Hospitality** | Hotels, Restaurants, Events | Calendar, Billing, CRM | Phase 3 |
| **Logistics** | Fleet, Delivery, Warehousing | CRM, Automation | Phase 3 |
| **Professional Services** | Consulting, Legal, Accounting | CRM, Calendar, Billing | Phase 3 |

**Authority:** Industry Configurations are controlled by Core API. Partners can customize configurations for their Tenants.

**Scale Assumptions:**

- **7 Industries** (each industry has pre-configured workflows)
- **1,000+ Partners** (each Partner can customize workflows for their Tenants)

---

## Data Model (Max-Scale-First)

### Core Entities

```
SuperAdmin
  └── Partner (1,000+)
      ├── Tenant (1,000,000+)
      │   ├── User (100,000,000+)
      │   ├── Role
      │   ├── Entitlement
      │   └── FeatureFlag
      ├── SubPartner (Phase 2)
      └── Agent (Phase 2)

Tenant
  ├── Contact (CRM) (1,000,000+)
  ├── Pipeline (CRM)
  ├── Opportunity (CRM)
  ├── Workflow (Automation) (10,000,000+)
  ├── Form (Forms)
  ├── Calendar (Calendar)
  ├── Subscription (Billing)
  └── Transaction (Commerce)
```

### Partner Hierarchy (Phase 2)

```
Partner
  └── SubPartner (10,000+)
      └── Agent (100,000+)
          └── Merchant (Tenant) (1,000,000+)
```

- **Partner** earns 5% commission on Merchants onboarded by their SubPartners' Agents
- **SubPartner** earns 15% commission on Merchants onboarded by their Agents
- **Agent** earns 20% commission on Merchants they directly onboard

**Scale Assumptions:**

- **1,000+ Partners**
- **10,000+ SubPartners**
- **100,000+ Agents**
- **1,000,000+ Merchants (Tenants)**

---

## Authority Boundaries (Max-Scale-First)

### Who Can Do What

| Action | Super Admin | Partner | Tenant | User |
|--------|-------------|---------|--------|------|
| Create Partner | ✅ | ❌ | ❌ | ❌ |
| Create Tenant | ✅ | ✅ (self-service) | ❌ | ❌ |
| Create User | ✅ | ✅ | ✅ | ❌ |
| Activate Capability | ✅ | ✅ | ❌ | ❌ |
| Configure Industry Suite | ✅ | ✅ | ❌ | ❌ |
| View Tenant Data | ✅ | ✅ (own Tenants only) | ✅ (own Tenant only) | ✅ (own Tenant only) |
| Modify Core Domain Logic | ✅ | ❌ | ❌ | ❌ |
| Configure Workflows | ✅ | ✅ | ✅ | ❌ |
| Set Pricing | ✅ | ✅ (own Tenants only) | ❌ | ❌ |

---

## Deployment Architecture (AWS-Native)

### Phase 1: Monolithic Core API on AWS Fargate

```
┌─────────────────────────────────────────┐
│     Core API (AWS Fargate)              │
│  ┌───────────────────────────────────┐  │
│  │ Identity, Tenants, Roles, Perms   │  │
│  │ Partners, Audit, Feature Flags    │  │
│  │ Entitlements, Modules, Pricing    │  │
│  │ Branding, POS, ParkHub, SVM, MVM  │  │
│  └───────────────────────────────────┘  │
└─────────────────────────────────────────┘
           │
           ▼
┌─────────────────────────────────────────┐
│    AWS Aurora (PostgreSQL-compatible)   │
└─────────────────────────────────────────┘

┌─────────────────────────────────────────┐
│    Frontend Apps (AWS Amplify)          │
│  ┌───────────────────────────────────┐  │
│  │ POS, Partner Portal, SVM, MVM     │  │
│  └───────────────────────────────────┘  │
└─────────────────────────────────────────┘

┌─────────────────────────────────────────┐
│    AWS Cognito (Authentication)         │
└─────────────────────────────────────────┘

┌─────────────────────────────────────────┐
│    AWS S3 + CloudFront (Storage + CDN)  │
└─────────────────────────────────────────┘
```

**Justification:** Monolithic Core API is simpler to build and deploy. Suitable for Phase 1 (commerce focus). All services are AWS-native (single-bill consolidation).

---

### Phase 2: Hybrid (Monolithic + Microservices) on AWS

```
┌─────────────────────────────────────────┐
│     Core API (AWS Fargate)              │
│  ┌───────────────────────────────────┐  │
│  │ Identity, Tenants, Roles, Perms   │  │
│  │ Partners, Audit, Feature Flags    │  │
│  │ Entitlements, Modules, Pricing    │  │
│  │ Branding, POS, ParkHub, SVM, MVM  │  │
│  └───────────────────────────────────┘  │
└─────────────────────────────────────────┘
           │
           ▼
┌─────────────────────────────────────────┐
│    AWS Aurora (PostgreSQL-compatible)   │
└─────────────────────────────────────────┘

┌─────────────────────────────────────────┐
│    CRM Service (AWS Fargate)            │
└─────────────────────────────────────────┘

┌─────────────────────────────────────────┐
│    Automation Service (AWS Lambda +     │
│    Step Functions)                      │
└─────────────────────────────────────────┘

┌─────────────────────────────────────────┐
│    Communication Service (AWS Lambda +  │
│    SES + SNS)                           │
└─────────────────────────────────────────┘

┌─────────────────────────────────────────┐
│    Billing Service (AWS Fargate)        │
└─────────────────────────────────────────┘

┌─────────────────────────────────────────┐
│    Frontend Apps (AWS Amplify)          │
│  ┌───────────────────────────────────┐  │
│  │ POS, Partner Portal, SVM, MVM     │  │
│  └───────────────────────────────────┘  │
└─────────────────────────────────────────┘
```

**Justification:** High-load domains (CRM, Automation, Communication, Billing) are split into separate services for independent scaling and deployment. All services are AWS-native (single-bill consolidation).

---

## Security & Isolation (Max-Scale-First)

### Multi-Tenant Isolation

- **Database:** Row-level security enforces `tenantId` scoping (AWS Aurora)
- **API:** All API requests include `tenantId` in JWT token (AWS Cognito)
- **Audit Logs:** All cross-tenant access attempts are logged (AWS CloudWatch Logs)
- **Feature Flags:** Entitlements gate capabilities per Tenant (AWS AppConfig)

### Partner Isolation

- **Partners cannot access other Partners' Tenants**
- **Partners cannot modify other Partners' configurations**
- **Partners cannot view other Partners' revenue or analytics**

### Offline-First Security

- **POS:** Transactions are signed and hashed locally before sync
- **ParkHub:** Transactions are signed and hashed locally before sync
- **Sync Conflicts:** POS/ParkHub always wins in conflict resolution (Core is read-only replica)

---

## Summary: Architectural Decisions (AWS-First + Max-Scale-First)

| Decision | Choice | Directive |
|----------|--------|-----------|
| **Platform Model** | Meta-Platform (Hybrid) | Max-Scale-First |
| **Target User** | Partners (platform operators) | Max-Scale-First |
| **Architecture Model** | Partner-Centric (Partner → Tenant hierarchy) | Max-Scale-First |
| **Industry Scope** | Composable Primitives + Industry Configurations | Max-Scale-First |
| **Offline-First** | Commerce suites only (POS, ParkHub, indefinite offline) | Max-Scale-First |
| **Modularity** | Monolithic Core API (Phase 1) → Hybrid (Phase 2) | Max-Scale-First |
| **Database** | AWS Aurora (PostgreSQL-compatible) | AWS-First |
| **Authentication** | AWS Cognito | AWS-First |
| **Backend Hosting** | AWS Fargate | AWS-First |
| **Frontend Hosting** | AWS Amplify | AWS-First |
| **Email** | AWS SES | AWS-First |
| **SMS** | AWS SNS | AWS-First |
| **Storage** | AWS S3 + CloudFront | AWS-First |
| **Analytics** | AWS CloudWatch + Athena + QuickSight | AWS-First |
| **Background Jobs** | AWS Lambda | AWS-First |

---

**End of Clean Platform Architecture (Target State)**
# NOTE: Implementation Sequencing vs. Architectural Limitation

**This build order represents IMPLEMENTATION SEQUENCING, not ARCHITECTURAL LIMITATION.**

## Max-Scale-First Directive

Per the Founder Non-Negotiable Directive #2 (Design for Maximum Scale from Day One):

> **Implementation may be phased. Architecture must NOT be phased.**

## What This Means

1. **Architecture is designed for full scope upfront:**
   - Partner-Centric hierarchy (Super Admin → Partner → Tenant → User)
   - Composable Primitives (CRM, Automation, Communication, Forms, Calendar, Billing, Affiliate)
   - Multi-Level Affiliate Trees (Partner → SubPartner → Agent → Merchant)
   - Industry-Agnostic Modules (7 industries: commerce, education, health, civic, hospitality, logistics, professional services)
   - White-Labeled SaaS Resale (Partners brand and resell WebWaka)

2. **Implementation is executed incrementally:**
   - Phase 1: Core Infrastructure + Commerce Suites
   - Phase 2: Composable Primitives + Affiliate System
   - Phase 3: Multi-Industry Expansion

3. **Data model supports full scope from day one:**
   - Partner → Tenant hierarchy exists from Phase 1
   - Affiliate hierarchy (Partner → SubPartner → Agent) exists from Phase 1 (implemented in Phase 2)
   - Industry configurations exist from Phase 1 (commerce), expanded in Phase 2 (education) and Phase 3 (health, civic, hospitality, logistics)

## Examples

| Feature | Architecture (Day 1) | Implementation (Phased) |
|---------|----------------------|-------------------------|
| **Affiliate System** | Data model supports multi-level affiliate trees from day one | Built in Phase 2 (after Partner Portal is validated) |
| **Education Suite** | Data model supports education workflows from day one | Built in Phase 2 (after commerce is validated) |
| **Health Suite** | Data model supports health workflows from day one | Built in Phase 3 (after education is validated) |
| **Microservices** | Architecture supports microservices from day one (clear domain boundaries) | Migrated in Phase 2 (after monolithic Core API is validated) |

## Why This Matters

**We do NOT "scale up later."** We design for scale now, execute incrementally.

This prevents:

- **Architectural debt** (rework required to support scale)
- **Breaking changes** (data model changes that break existing features)
- **Migration complexity** (migrating from tenant-centric to partner-centric architecture)

---

**This note must be prepended to the Sequential Build Order section.**
# 6. STRICT, SEQUENTIAL BUILD ORDER

**Analysis Date:** 2026-01-26  
**Analyst:** Manus Re-Founding Phase (WRF-0 Authoritative)

---

## Purpose

This section defines the **strict, sequential build order** for WebWaka. It specifies:

- **What must be built first** (prerequisites)
- **What is forbidden until prerequisites exist** (blocked dependencies)
- **What dependencies are structural vs. optional** (hard vs. soft dependencies)

**This prevents future rework and architectural drift.**

---

## Build Order Principles

### Principle 1: Infrastructure Before Features

**Core Infrastructure must be built before any features.**

- Identity & Authentication
- Database & ORM
- Audit Logs
- Roles & Permissions
- Multi-Tenant Isolation

### Principle 2: Primitives Before Suites

**Composable primitives must be built before industry suites.**

- CRM, Automation, Communication, Forms, Calendar must be built before Education, Health, Civic, Hospitality, Logistics suites.

### Principle 3: Partner Portal Before Affiliate System

**Partner Portal must be built before Affiliate System.**

- Partners must be able to create Tenants before they can recruit Sub-Partners and Agents.

### Principle 4: Commerce Before Multi-Industry

**Commerce suites must be validated before expanding to multi-industry.**

- POS, ParkHub, SVM, MVM must be production-ready before building Education, Health, Civic, Hospitality, Logistics suites.

---

## Phase 1: Core Infrastructure + Commerce Suites

**Duration:** 3-6 months  
**Goal:** Build Core Infrastructure and validate commerce model.

### Phase 1.1: Core Infrastructure

**What to Build:**

1. **Identity & Authentication**
   - Clerk integration
   - User registration, login, logout
   - Session management
   - MFA support

2. **Database & ORM**
   - PostgreSQL via Neon
   - Prisma ORM
   - Multi-tenant schema design
   - Row-level security

3. **Audit Logs**
   - Immutable audit trail for all write operations
   - Audit log API (query, filter, export)

4. **Roles & Permissions**
   - RBAC (Role-Based Access Control)
   - Roles: Super Admin, Partner Admin, Tenant Admin, User
   - Permissions: Create, Read, Update, Delete per resource
   - Scopes: Global, Partner, Tenant, User

5. **Multi-Tenant Isolation**
   - Tenant-scoped data isolation
   - `tenantId` enforcement in all queries
   - Tenant provisioning API

6. **Feature Flags & Entitlements**
   - Feature flags (runtime toggles)
   - Entitlements (capability gating per Tenant)
   - Entitlement enforcement in API

**Dependencies:** None (this is the foundation).

**Blocked Until Complete:** Everything else.

---

### Phase 1.2: Partner-Centric Architecture

**What to Build:**

1. **Partner Entity**
   - Partner model (database schema)
   - Partner CRUD API
   - Partner → Tenant hierarchy

2. **Partner Portal (Minimal)**
   - Partner dashboard (view Tenants)
   - Client Organization Creation (Partners create Tenants)
   - Tenant management (view, edit, delete Tenants)

3. **Partner-Level Branding**
   - Partner-level logo, colors, email templates
   - Tenant inherits Partner branding by default
   - Tenant can override Partner branding

**Dependencies:** Phase 1.1 (Core Infrastructure).

**Blocked Until Complete:** Affiliate System, Partner Revenue Sharing.

---

### Phase 1.3: Commerce Suites (Offline-First)

**What to Build:**

1. **POS (Point of Sale)**
   - Offline-first architecture (IndexedDB)
   - Transaction queuing and sync
   - Inventory management
   - Receipt generation
   - Payment integration (Paystack)

2. **ParkHub (Transport Management)**
   - Offline-first architecture (IndexedDB)
   - Transaction queuing and sync
   - Fleet management
   - Booking management

3. **SVM (Single Vendor Marketplace)**
   - Online-only
   - Product catalog
   - Shopping cart
   - Checkout
   - Payment integration (Paystack)

4. **MVM (Multi Vendor Marketplace)**
   - Online-only
   - Multi-vendor product catalog
   - Vendor management
   - Commission tracking
   - Payment integration (Paystack)

**Dependencies:** Phase 1.1 (Core Infrastructure), Phase 1.2 (Partner-Centric Architecture).

**Blocked Until Complete:** Education, Health, Civic, Hospitality, Logistics suites.

---

### Phase 1.4: Production Readiness

**What to Build:**

1. **CI/CD Pipeline**
   - GitHub Actions
   - Automated tests (unit, integration, e2e)
   - Automated deployments (Fly.io, Vercel)

2. **Monitoring & Alerting**
   - Error tracking (Sentry)
   - Performance monitoring (Fly.io metrics)
   - Uptime monitoring (UptimeRobot)

3. **Backup & Disaster Recovery**
   - Automated database backups (Neon)
   - Disaster recovery plan
   - RTO/RPO definition

4. **Security Hardening**
   - Rate limiting
   - CORS configuration
   - SQL injection prevention
   - XSS prevention

**Dependencies:** Phase 1.1, 1.2, 1.3.

**Blocked Until Complete:** Production launch.

---

## Phase 2: Composable Primitives + Affiliate System

**Duration:** 6-9 months  
**Goal:** Build composable primitives (CRM, Automation, Communication) and enable Partner-driven growth (Affiliate System).

### Phase 2.1: CRM Domain

**What to Build:**

1. **Contacts Database**
   - Contact model (database schema)
   - Contact CRUD API
   - Custom fields (extensible contact attributes)
   - Tags (contact categorization and segmentation)

2. **Pipelines**
   - Pipeline model (database schema)
   - Pipeline CRUD API
   - Pipeline stages (customizable funnel stages)
   - Opportunities (potential sales/deals)

3. **Lead Scoring**
   - Lead scoring algorithm
   - Lead scoring API

4. **Task Management**
   - Task model (database schema)
   - Task CRUD API
   - Task assignment (assign tasks to users)

**Dependencies:** Phase 1.1 (Core Infrastructure), Phase 1.2 (Partner-Centric Architecture).

**Blocked Until Complete:** Education, Health, Civic, Hospitality, Logistics suites.

---

### Phase 2.2: Automation Domain

**What to Build:**

1. **Workflow Builder**
   - Workflow model (database schema)
   - Workflow CRUD API
   - Visual automation designer (frontend)

2. **Trigger-Based Automation**
   - Event-driven workflows
   - Triggers (e.g., "Contact created," "Opportunity moved to stage")
   - Actions (e.g., "Send email," "Send SMS," "Create task")

3. **Conditional Logic**
   - If/then branching in workflows
   - Conditional logic API

4. **Delays & Scheduling**
   - Time-based workflow actions
   - Delay API (e.g., "Wait 1 day," "Wait until date")

**Dependencies:** Phase 1.1 (Core Infrastructure), Phase 2.1 (CRM Domain).

**Blocked Until Complete:** Education, Health, Civic, Hospitality, Logistics suites.

---

### Phase 2.3: Communication Domain

**What to Build:**

1. **Email Campaigns**
   - Email model (database schema)
   - Email CRUD API
   - Resend integration (bulk email sending)

2. **SMS Campaigns**
   - SMS model (database schema)
   - SMS CRUD API
   - Africa's Talking integration (bulk SMS sending)

3. **WhatsApp Integration**
   - WhatsApp model (database schema)
   - WhatsApp CRUD API
   - Africa's Talking integration (WhatsApp messaging)

4. **Two-Way Messaging**
   - Unified inbox (email, SMS, WhatsApp)
   - Inbox API (query, filter, reply)

**Dependencies:** Phase 1.1 (Core Infrastructure).

**Blocked Until Complete:** Education, Health, Civic, Hospitality, Logistics suites.

---

### Phase 2.4: Billing Domain

**What to Build:**

1. **Subscription Management**
   - Subscription model (database schema)
   - Subscription CRUD API
   - Stripe Billing integration (recurring billing)

2. **Invoice Generation**
   - Invoice model (database schema)
   - Invoice CRUD API
   - Invoice PDF generation

3. **Payment Tracking**
   - Payment model (database schema)
   - Payment CRUD API
   - Payment webhook handling (Stripe, Paystack)

4. **Partner Revenue Sharing**
   - Revenue share model (database schema)
   - Revenue share calculation (based on Tenant subscriptions)
   - Revenue share payout API

**Dependencies:** Phase 1.1 (Core Infrastructure), Phase 1.2 (Partner-Centric Architecture).

**Blocked Until Complete:** Affiliate System.

---

### Phase 2.5: Affiliate Domain

**What to Build:**

1. **Multi-Level Affiliate Hierarchy**
   - SubPartner model (database schema)
   - Agent model (database schema)
   - Partner → SubPartner → Agent → Merchant hierarchy

2. **Recurring Commission Tracking**
   - Commission model (database schema)
   - Commission calculation (40% payout: 20% Agent + 15% SubPartner + 5% Partner)
   - Commission CRUD API

3. **Commission Payouts**
   - Payout model (database schema)
   - Payout CRUD API
   - Payout integration (Stripe Connect)

4. **Fraud Prevention**
   - Self-referral prevention
   - Clawback policy
   - Manual review for high-volume affiliates

**Dependencies:** Phase 1.2 (Partner-Centric Architecture), Phase 2.4 (Billing Domain).

**Blocked Until Complete:** None (Affiliate System is optional for Phase 2).

---

### Phase 2.6: API Gateway

**What to Build:**

1. **REST API**
   - Public API for Partners to integrate with WebWaka
   - API documentation (OpenAPI/Swagger)
   - API versioning

2. **Webhooks**
   - Webhook model (database schema)
   - Webhook CRUD API
   - Webhook delivery (event-driven integrations)

3. **API Keys**
   - API Key model (database schema)
   - API Key CRUD API
   - API Key authentication

4. **Rate Limiting**
   - Rate limiting per API Key
   - Rate limiting per Partner

**Dependencies:** Phase 1.1 (Core Infrastructure), Phase 1.2 (Partner-Centric Architecture).

**Blocked Until Complete:** None (API Gateway is optional for Phase 2).

---

### Phase 2.7: Analytics Domain

**What to Build:**

1. **Dashboard Analytics**
   - PostHog integration
   - Key metrics overview (Tenants, Users, Revenue)

2. **Pipeline Reports**
   - Conversion rates per pipeline stage
   - Revenue per pipeline stage

3. **Lead Source Tracking**
   - Attribution reporting
   - Lead source API

**Dependencies:** Phase 2.1 (CRM Domain).

**Blocked Until Complete:** None (Analytics is optional for Phase 2).

---

## Phase 3: Multi-Industry Expansion

**Duration:** 9-12 months  
**Goal:** Expand to multi-industry (Education, Health, Civic, Hospitality, Logistics).

### Phase 3.1: Forms Domain

**What to Build:**

1. **Form Builder**
   - Form model (database schema)
   - Form CRUD API
   - Drag-and-drop form designer (frontend)

2. **Survey Builder**
   - Survey model (database schema)
   - Survey CRUD API
   - Drag-and-drop survey designer (frontend)

3. **Embeddable Forms**
   - Embed forms on external sites
   - Form submission API

**Dependencies:** Phase 1.1 (Core Infrastructure).

**Blocked Until Complete:** Education, Health suites.

---

### Phase 3.2: Calendar Domain

**What to Build:**

1. **Calendar Booking**
   - Calendar model (database schema)
   - Calendar CRUD API
   - Online appointment scheduling

2. **Availability Management**
   - Availability model (database schema)
   - Availability CRUD API
   - Availability rules (e.g., "Available Mon-Fri 9am-5pm")

3. **Payment Collection (Appointments)**
   - Charge for appointments
   - Payment integration (Stripe, Paystack)

**Dependencies:** Phase 1.1 (Core Infrastructure), Phase 2.4 (Billing Domain).

**Blocked Until Complete:** Health, Hospitality suites.

---

### Phase 3.3: Education Suite

**What to Build:**

1. **School Management**
   - School model (database schema)
   - Student model (database schema)
   - Teacher model (database schema)
   - Class model (database schema)

2. **Grading**
   - Grade model (database schema)
   - Grading CRUD API
   - Report card generation

3. **Fees**
   - Fee model (database schema)
   - Fee CRUD API
   - Fee collection (Stripe, Paystack)

4. **LMS (Learning Management System)**
   - Course model (database schema)
   - Course CRUD API
   - Video hosting (Cloudflare Stream, Mux)

**Dependencies:** Phase 2.1 (CRM), Phase 2.4 (Billing), Phase 3.1 (Forms), Phase 3.2 (Calendar).

**Blocked Until Complete:** None (Education Suite is optional for Phase 3).

---

### Phase 3.4: Health Suite

**What to Build:**

1. **Clinic Management**
   - Clinic model (database schema)
   - Doctor model (database schema)
   - Patient model (database schema)
   - Appointment model (database schema)

2. **Pharmacy Management**
   - Pharmacy model (database schema)
   - Drug model (database schema)
   - Prescription model (database schema)

3. **Patient Records**
   - Patient record model (database schema)
   - Patient record CRUD API
   - Medical history tracking

4. **Billing**
   - Billing model (database schema)
   - Billing CRUD API
   - Payment integration (Stripe, Paystack)

**Dependencies:** Phase 2.1 (CRM), Phase 2.4 (Billing), Phase 3.2 (Calendar).

**Blocked Until Complete:** None (Health Suite is optional for Phase 3).

---

### Phase 3.5: Civic Suite

**What to Build:**

1. **Community Finance**
   - Community model (database schema)
   - Member model (database schema)
   - Contribution model (database schema)
   - Loan model (database schema)

2. **Cooperatives**
   - Cooperative model (database schema)
   - Cooperative CRUD API
   - Member management

**Dependencies:** Phase 2.1 (CRM), Phase 2.4 (Billing).

**Blocked Until Complete:** None (Civic Suite is optional for Phase 3).

---

### Phase 3.6: Hospitality Suite

**What to Build:**

1. **Hotel Management**
   - Hotel model (database schema)
   - Room model (database schema)
   - Booking model (database schema)

2. **Restaurant Management**
   - Restaurant model (database schema)
   - Menu model (database schema)
   - Order model (database schema)

3. **Event Management**
   - Event model (database schema)
   - Event CRUD API
   - Ticket sales (Stripe, Paystack)

4. **Reservations**
   - Reservation model (database schema)
   - Reservation CRUD API
   - Payment integration (Stripe, Paystack)

**Dependencies:** Phase 2.1 (CRM), Phase 2.4 (Billing), Phase 3.2 (Calendar).

**Blocked Until Complete:** None (Hospitality Suite is optional for Phase 3).

---

### Phase 3.7: Logistics Suite

**What to Build:**

1. **Fleet Management**
   - Fleet model (database schema)
   - Vehicle model (database schema)
   - Driver model (database schema)

2. **Delivery Management**
   - Delivery model (database schema)
   - Delivery CRUD API
   - Delivery tracking

3. **Warehousing**
   - Warehouse model (database schema)
   - Warehouse CRUD API
   - Inventory tracking

4. **Order Fulfillment**
   - Order model (database schema)
   - Order CRUD API
   - Fulfillment tracking

**Dependencies:** Phase 2.1 (CRM), Phase 2.2 (Automation).

**Blocked Until Complete:** None (Logistics Suite is optional for Phase 3).

---

## Dependency Graph

```
Phase 1.1: Core Infrastructure
    │
    ├─→ Phase 1.2: Partner-Centric Architecture
    │       │
    │       ├─→ Phase 1.3: Commerce Suites
    │       │       │
    │       │       └─→ Phase 1.4: Production Readiness
    │       │
    │       ├─→ Phase 2.1: CRM Domain
    │       │       │
    │       │       ├─→ Phase 2.2: Automation Domain
    │       │       │
    │       │       └─→ Phase 2.7: Analytics Domain
    │       │
    │       ├─→ Phase 2.3: Communication Domain
    │       │
    │       ├─→ Phase 2.4: Billing Domain
    │       │       │
    │       │       └─→ Phase 2.5: Affiliate Domain
    │       │
    │       └─→ Phase 2.6: API Gateway
    │
    ├─→ Phase 3.1: Forms Domain
    │
    └─→ Phase 3.2: Calendar Domain

Phase 2.1 + Phase 2.4 + Phase 3.1 + Phase 3.2 → Phase 3.3: Education Suite
Phase 2.1 + Phase 2.4 + Phase 3.2 → Phase 3.4: Health Suite
Phase 2.1 + Phase 2.4 → Phase 3.5: Civic Suite
Phase 2.1 + Phase 2.4 + Phase 3.2 → Phase 3.6: Hospitality Suite
Phase 2.1 + Phase 2.2 → Phase 3.7: Logistics Suite
```

---

## What is Forbidden Until Prerequisites Exist

| Forbidden Action | Prerequisite |
|------------------|--------------|
| Build any feature | Phase 1.1 (Core Infrastructure) |
| Build Affiliate System | Phase 1.2 (Partner-Centric Architecture) + Phase 2.4 (Billing Domain) |
| Build Education Suite | Phase 2.1 (CRM) + Phase 2.4 (Billing) + Phase 3.1 (Forms) + Phase 3.2 (Calendar) |
| Build Health Suite | Phase 2.1 (CRM) + Phase 2.4 (Billing) + Phase 3.2 (Calendar) |
| Build Civic Suite | Phase 2.1 (CRM) + Phase 2.4 (Billing) |
| Build Hospitality Suite | Phase 2.1 (CRM) + Phase 2.4 (Billing) + Phase 3.2 (Calendar) |
| Build Logistics Suite | Phase 2.1 (CRM) + Phase 2.2 (Automation) |
| Launch to production | Phase 1.4 (Production Readiness) |

---

## Summary: Build Order

| Phase | Duration | Goal |
|-------|----------|------|
| **Phase 1** | 3-6 months | Core Infrastructure + Commerce Suites |
| **Phase 2** | 6-9 months | Composable Primitives + Affiliate System |
| **Phase 3** | 9-12 months | Multi-Industry Expansion |

**Total Duration:** 18-27 months (1.5-2.25 years)

---

**End of Strict Sequential Build Order**
# 7. GOVERNANCE & OPERATOR RULES

**Analysis Date:** 2026-01-26  
**Analyst:** Manus Re-Founding Phase (WRF-0 Authoritative)

---

## Purpose

This section defines the **governance rules** for how Manus, Emergent, and Replit are to be used together going forward. It establishes:

- **Operator roles and responsibilities**
- **When to use which operator**
- **How operators coordinate**
- **What operators can and cannot do**

**This prevents operator confusion and ensures consistent execution.**

---

## Operator Model: Manus-First (Phase 1)

### Decision

**Manus is the sole operator for Phase 1.** Emergent and Replit are not used.

### Rationale

The previous Constitution defined a complex vibecoding governance model (Emergent-01, Lovable-01, Replit-01, etc.) that was **not being followed in practice**. All recent execution prompts (IR-1, IR-2, EP-1) were executed by Manus.

**Lesson Learned:** Don't design governance for tools you haven't validated. Start simple (one operator, one workflow) and add complexity only when needed.

### When to Reconsider

Reintroduce multi-operator model in Phase 2+ only if there's a **clear need** for specialization:

- **Emergent:** For complex logic and backend development
- **Replit:** For code audits and security reviews
- **Manus:** For planning, coordination, and frontend development

---

## Manus Operator Rules

### What Manus Can Do

1. **Read and analyze documents** (Constitution, blueprints, execution prompts)
2. **Create and update planning documents** (blueprints, build orders, governance rules)
3. **Execute code changes** (backend, frontend, database migrations)
4. **Deploy applications** (Fly.io, Vercel)
5. **Run tests** (unit, integration, e2e)
6. **Create and manage repositories** (GitHub)
7. **Create and manage issues** (GitHub Issues)
8. **Create and manage pull requests** (GitHub Pull Requests)
9. **Coordinate with Founder** (ask questions, clarify decisions, report progress)

### What Manus Cannot Do

1. **Make Founder Decisions** (Manus can recommend, but cannot decide)
2. **Modify production data** (Manus can read, but cannot write to production database)
3. **Deploy to production without approval** (Manus must request Founder approval before production deployment)
4. **Bypass security controls** (Manus must follow RBAC, entitlements, feature flags)
5. **Access other Partners' Tenants** (Manus must respect multi-tenant isolation)

---

## Execution Workflow

### Phase 1: Manus-Only Workflow

```
1. Founder creates execution prompt
2. Manus reads execution prompt
3. Manus analyzes requirements
4. Manus creates task plan
5. Manus executes tasks (code, tests, deployments)
6. Manus reports progress to Founder
7. Manus delivers results to Founder
8. Founder reviews results
9. Founder approves or requests changes
10. Repeat until task is complete
```

### Phase 2+: Multi-Operator Workflow (If Reintroduced)

```
1. Founder creates execution prompt
2. Manus reads execution prompt
3. Manus analyzes requirements
4. Manus assigns tasks to operators:
   - Emergent: Backend logic, API development
   - Replit: Code audits, security reviews
   - Manus: Planning, coordination, frontend development
5. Operators execute tasks in parallel
6. Manus coordinates operators
7. Manus synthesizes results
8. Manus reports progress to Founder
9. Manus delivers results to Founder
10. Founder reviews results
11. Founder approves or requests changes
12. Repeat until task is complete
```

---

## Repository Governance

### Phase 1: Monorepo (Recommended)

**Structure:**

```
webwaka-platform/
├── apps/
│   ├── core-api/          (Fastify backend)
│   ├── pos/               (Next.js frontend)
│   ├── partner-portal/    (Next.js frontend)
│   ├── svm/               (Next.js frontend)
│   └── mvm/               (Next.js frontend)
├── packages/
│   ├── database/          (Prisma schema, migrations)
│   ├── auth/              (Clerk integration)
│   ├── ui/                (Shared UI components)
│   └── utils/             (Shared utilities)
├── docs/
│   ├── architecture.md
│   ├── build-order.md
│   └── governance.md
└── README.md
```

**Rationale:** Monorepo is simpler to build and iterate. All code is in one place. Easier to refactor. Requires monorepo tooling (Turborepo, Nx).

**Who Manages:** Manus (sole operator for Phase 1).

---

### Phase 2+: Multi-Repo (If Needed)

**Structure:**

```
webwaka-core-api/          (Fastify backend)
webwaka-pos/               (Next.js frontend)
webwaka-partner-portal/    (Next.js frontend)
webwaka-svm/               (Next.js frontend)
webwaka-mvm/               (Next.js frontend)
webwaka-database/          (Prisma schema, migrations)
webwaka-shared/            (Shared libraries)
```

**Rationale:** Multi-repo enables independent versioning and deployment. Suitable for Phase 2+ if suites require independent deployment.

**Who Manages:** Manus + Emergent + Replit (if multi-operator model is reintroduced).

---

## Documentation Governance

### What Must Be Documented

1. **Architecture Decisions** (why we chose X over Y)
2. **Build Order** (what must be built first, what is blocked)
3. **Governance Rules** (operator roles, responsibilities, workflows)
4. **API Contracts** (REST API, webhooks, event schemas)
5. **Security Policies** (RBAC, entitlements, feature flags)
6. **Disaster Recovery Plan** (backup, restore, RTO/RPO)

### What Should Not Be Documented

1. **Implementation Details** (code is self-documenting)
2. **Temporary Decisions** (use code comments instead)
3. **Trivial Choices** (e.g., "Use Fastify for Core API")

### Where to Document

| Document Type | Location |
|---------------|----------|
| Architecture Decisions | `docs/architecture.md` |
| Build Order | `docs/build-order.md` |
| Governance Rules | `docs/governance.md` |
| API Contracts | `docs/api.md` |
| Security Policies | `docs/security.md` |
| Disaster Recovery Plan | `docs/disaster-recovery.md` |

### Documentation Workflow

1. **Manus creates or updates documentation** (in Markdown format)
2. **Manus commits documentation to GitHub** (in `docs/` directory)
3. **Founder reviews documentation** (via GitHub Pull Request)
4. **Founder approves or requests changes**
5. **Manus merges documentation** (after Founder approval)

**No documentation bottleneck.** Documentation is created as part of the execution workflow, not as a separate phase.

---

## Code Review Governance

### Phase 1: Founder Review Only

**Workflow:**

1. Manus creates code changes
2. Manus commits code to GitHub
3. Manus creates Pull Request
4. Founder reviews Pull Request
5. Founder approves or requests changes
6. Manus merges Pull Request (after Founder approval)

**Rationale:** Founder is the sole reviewer for Phase 1. No need for multi-operator code review.

---

### Phase 2+: Multi-Operator Review (If Reintroduced)

**Workflow:**

1. Emergent creates backend code changes
2. Emergent commits code to GitHub
3. Emergent creates Pull Request
4. Replit reviews Pull Request (code audit, security review)
5. Replit approves or requests changes
6. Manus reviews Pull Request (coordination, integration)
7. Manus approves or requests changes
8. Founder reviews Pull Request (final approval)
9. Founder approves or requests changes
10. Manus merges Pull Request (after all approvals)

**Rationale:** Multi-operator review ensures code quality and security. Replit specializes in code audits. Manus coordinates integration. Founder has final approval.

---

## Deployment Governance

### Phase 1: Manus Deploys to Staging, Founder Approves Production

**Workflow:**

1. Manus deploys to staging (Fly.io staging, Vercel preview)
2. Manus runs tests (unit, integration, e2e)
3. Manus reports test results to Founder
4. Founder reviews staging deployment
5. Founder approves production deployment
6. Manus deploys to production (Fly.io production, Vercel production)
7. Manus monitors production (error tracking, performance monitoring)

**Rationale:** Manus can deploy to staging autonomously. Production deployment requires Founder approval.

---

### Phase 2+: Automated Production Deployment (If Validated)

**Workflow:**

1. Manus deploys to staging (Fly.io staging, Vercel preview)
2. Manus runs tests (unit, integration, e2e)
3. If all tests pass, Manus deploys to production automatically
4. Manus monitors production (error tracking, performance monitoring)
5. If errors detected, Manus rolls back automatically
6. Manus reports deployment status to Founder

**Rationale:** Automated production deployment is suitable for Phase 2+ once the platform is validated and stable.

---

## Infrastructure Governance

### Phase 1: Shared Dev/Prod Infrastructure (NOT RECOMMENDED)

**Status:** The previous Constitution defined shared dev/prod infrastructure (database, Clerk, Fly.io) but safeguards were "REQUIRES DOCUMENTATION" (i.e., not implemented).

**Risk:** High blast-radius risk. Development changes can affect production data.

**Recommendation:** **DO NOT SHARE DEV/PROD INFRASTRUCTURE.** Separate development and production infrastructure.

---

### Phase 1: Isolated Dev/Prod Infrastructure (RECOMMENDED)

**Structure:**

| Environment | Database | Clerk | Fly.io | Vercel |
|-------------|----------|-------|--------|--------|
| **Development** | Neon (dev) | Clerk (dev) | Fly.io (dev) | Vercel (preview) |
| **Production** | Neon (prod) | Clerk (prod) | Fly.io (prod) | Vercel (prod) |

**Rationale:** Isolated dev/prod infrastructure eliminates blast-radius risk. Development changes cannot affect production data.

**Who Manages:** Manus (sole operator for Phase 1).

---

## Security Governance

### Security Principles

1. **Principle of Least Privilege:** Users, Tenants, Partners, and Operators have only the permissions they need.
2. **Multi-Tenant Isolation:** All data is tenant-scoped. No cross-tenant data access.
3. **Audit All Write Operations:** All write operations generate immutable audit log entries.
4. **Enforce RBAC:** Authorization is enforced at the service layer with roles, permissions, and scopes.
5. **Rate Limiting:** API requests are rate-limited to prevent abuse.

### Security Policies

| Policy | Description |
|--------|-------------|
| **Password Policy** | Minimum 12 characters. Must include uppercase, lowercase, number, and symbol. |
| **MFA Policy** | MFA is required for Super Admins and Partner Admins. |
| **Session Policy** | Sessions expire after 24 hours of inactivity. |
| **API Key Policy** | API Keys are scoped to Partners. API Keys expire after 90 days. |
| **Audit Log Policy** | Audit logs are retained for 7 years. Audit logs are immutable. |

### Security Audits

| Audit Type | Frequency | Responsibility |
|------------|-----------|----------------|
| **Code Audit** | Every release | Replit (Phase 2+) or Founder (Phase 1) |
| **Security Audit** | Quarterly | External security firm |
| **Penetration Test** | Annually | External security firm |

---

## Backup & Disaster Recovery Governance

### Backup Policy

| Resource | Backup Frequency | Retention Period |
|----------|------------------|------------------|
| **Database** | Daily (automated via Neon) | 30 days |
| **Code** | Every commit (via GitHub) | Indefinite |
| **Configuration** | Every change (via GitHub) | Indefinite |

### Disaster Recovery Plan

| Scenario | Recovery Procedure | RTO | RPO |
|----------|-------------------|-----|-----|
| **Database Corruption** | Restore from latest backup | 1 hour | 24 hours |
| **Code Deployment Failure** | Rollback to previous version | 15 minutes | 0 (no data loss) |
| **Infrastructure Outage (Fly.io)** | Migrate to AWS ECS | 4 hours | 24 hours |
| **Infrastructure Outage (Neon)** | Migrate to AWS RDS | 4 hours | 24 hours |

**Who Manages:** Manus (sole operator for Phase 1).

---

## Summary: Governance Rules

| Rule | Description |
|------|-------------|
| **Operator Model** | Manus-only for Phase 1. Multi-operator for Phase 2+ (if needed). |
| **Repository Structure** | Monorepo for Phase 1. Multi-repo for Phase 2+ (if needed). |
| **Documentation** | Document architecture decisions, build order, governance rules, API contracts, security policies, disaster recovery plan. |
| **Code Review** | Founder review only for Phase 1. Multi-operator review for Phase 2+ (if needed). |
| **Deployment** | Manus deploys to staging. Founder approves production deployment. |
| **Infrastructure** | Isolated dev/prod infrastructure (NOT shared). |
| **Security** | Enforce RBAC, multi-tenant isolation, audit logs, rate limiting. |
| **Backup** | Daily database backups. Retain for 30 days. |
| **Disaster Recovery** | RTO: 1-4 hours. RPO: 0-24 hours. |

---

**End of Governance & Operator Rules**
# 8. TRANSITION PLAN: FROM CURRENT STATE TO TARGET STATE

**Analysis Date:** 2026-01-26  
**Analyst:** Manus Re-Founding Phase (WRF-0 Authoritative)

---

## Purpose

This section defines the **transition plan** from the current state (existing Constitution, codebase, and infrastructure) to the target state (clean architecture, partner-centric model, composable primitives).

It specifies:

- **What to freeze** (stop changing)
- **What to archive** (preserve but do not use)
- **What to reset** (delete and rebuild)
- **What to migrate** (preserve and transform)
- **What to build new** (create from scratch)

**This prevents wasted effort and ensures a clean foundation.**

---

## Current State Assessment

### What Exists Today

Based on the existing Constitution and previous execution prompts:

| Component | Status | Quality |
|-----------|--------|---------|
| **Core API (Fastify)** | Exists | Monolithic, commerce-focused, no CRM/Automation/Communication |
| **POS (Next.js)** | Exists | Offline-first, Core-integrated, production-ready |
| **ParkHub (Next.js)** | Planned | Not built yet |
| **SVM (Next.js)** | Planned | Not built yet |
| **MVM (Next.js)** | Planned | Not built yet |
| **Partner Portal (Next.js)** | Planned | Not built yet |
| **Database (PostgreSQL via Neon)** | Exists | Multi-tenant schema, Prisma ORM |
| **Authentication (Clerk)** | Exists | Working well |
| **Audit Logs** | Exists | Immutable audit trail |
| **Roles & Permissions (RBAC)** | Exists | Working well |
| **Feature Flags & Entitlements** | Exists | Working well |
| **Pricing Plans** | Exists | Metadata only, no runtime billing |
| **Branding** | Exists | Per-tenant branding |
| **CRM Domain** | Does not exist | Critical gap |
| **Automation Domain** | Does not exist | Critical gap |
| **Communication Domain** | Does not exist | Critical gap |
| **Billing Domain** | Does not exist | Critical gap (only metadata exists) |
| **Affiliate Domain** | Does not exist | Critical gap |
| **API Gateway** | Does not exist | Critical gap |
| **Analytics Domain** | Partial | Audit logs exist, no analytics dashboard |

---

## Transition Strategy

### Strategy 1: Clean Slate (RECOMMENDED)

**Description:** Archive the existing codebase and start fresh with a clean architecture.

**Rationale:**

- The existing Constitution documents a **commerce-focused, tenant-centric platform**
- The target state is a **meta-platform with partner-centric, composable primitives**
- These are **fundamentally incompatible architectures**
- Attempting to migrate the existing codebase will result in **architectural debt and complexity**

**What to Do:**

1. **Archive existing codebase** (preserve in GitHub, but do not use)
2. **Create new monorepo** (`webwaka-platform`)
3. **Build Phase 1 from scratch** (Core Infrastructure + Commerce Suites)
4. **Migrate data** (if any production data exists)

**Pros:**

- Clean foundation
- No architectural debt
- No legacy code to maintain
- Faster to build correctly than to refactor incorrectly

**Cons:**

- Existing code is not reused
- Requires rebuilding from scratch
- Longer time to market (but cleaner result)

---

### Strategy 2: Incremental Migration (NOT RECOMMENDED)

**Description:** Gradually refactor the existing codebase to match the target architecture.

**Rationale:**

- Preserves existing code
- Incremental changes are less risky
- Can ship features while refactoring

**What to Do:**

1. **Refactor Core API** (add Partner entity, Partner → Tenant hierarchy)
2. **Refactor POS** (ensure offline-first guarantees)
3. **Add missing domains** (CRM, Automation, Communication, Billing, Affiliate, API Gateway)
4. **Migrate data model** (add Partner → Tenant hierarchy)

**Pros:**

- Existing code is reused
- Incremental changes are less risky
- Can ship features while refactoring

**Cons:**

- Architectural debt accumulates
- Refactoring is complex and error-prone
- Risk of "half-migrated" state
- Longer time to market (due to refactoring complexity)

---

### Recommended Strategy: Clean Slate

**The Founder should choose Strategy 1 (Clean Slate).**

**Justification:**

The existing codebase is **fundamentally incompatible** with the target architecture. Attempting to migrate will result in architectural debt, complexity, and wasted effort. Starting fresh with a clean architecture is faster and cleaner.

---

## Transition Steps

### Step 1: Freeze Current Codebase

**Action:** Stop making changes to the existing codebase.

**Rationale:** Prevent wasted effort. Any changes to the existing codebase will be discarded in the clean slate approach.

**Who:** Founder (issue freeze directive to all operators).

**When:** Immediately (before any new execution prompts).

---

### Step 2: Archive Current Codebase

**Action:** Archive the existing codebase in GitHub.

**Rationale:** Preserve the existing codebase for reference, but do not use it.

**How:**

1. Create a new GitHub repository: `webwaka-archive`
2. Move all existing code to `webwaka-archive`
3. Add README: "This repository contains the archived WebWaka codebase. It is preserved for reference only. Do not use this code for new development."

**Who:** Manus (execute archive operation).

**When:** After Step 1 (freeze).

---

### Step 3: Create New Monorepo

**Action:** Create a new monorepo: `webwaka-platform`.

**Rationale:** Start fresh with a clean architecture.

**Structure:**

```
webwaka-platform/
├── apps/
│   ├── core-api/          (Fastify backend)
│   ├── pos/               (Next.js frontend)
│   ├── partner-portal/    (Next.js frontend)
│   ├── svm/               (Next.js frontend)
│   └── mvm/               (Next.js frontend)
├── packages/
│   ├── database/          (Prisma schema, migrations)
│   ├── auth/              (Clerk integration)
│   ├── ui/                (Shared UI components)
│   └── utils/             (Shared utilities)
├── docs/
│   ├── architecture.md
│   ├── build-order.md
│   └── governance.md
└── README.md
```

**Who:** Manus (create new monorepo).

**When:** After Step 2 (archive).

---

### Step 4: Build Phase 1 from Scratch

**Action:** Build Phase 1 (Core Infrastructure + Commerce Suites) from scratch.

**Rationale:** Clean foundation. No architectural debt.

**What to Build:**

- Phase 1.1: Core Infrastructure (Identity, Database, Audit Logs, RBAC, Multi-Tenant Isolation, Feature Flags, Entitlements)
- Phase 1.2: Partner-Centric Architecture (Partner entity, Partner Portal, Partner → Tenant hierarchy)
- Phase 1.3: Commerce Suites (POS, ParkHub, SVM, MVM)
- Phase 1.4: Production Readiness (CI/CD, Monitoring, Backup, Security)

**Who:** Manus (execute Phase 1 build).

**When:** After Step 3 (create new monorepo).

**Duration:** 3-6 months.

---

### Step 5: Migrate Data (If Production Data Exists)

**Action:** Migrate production data from the old database to the new database.

**Rationale:** Preserve production data (if any).

**How:**

1. **Export data from old database** (PostgreSQL dump)
2. **Transform data to match new schema** (add Partner entity, Partner → Tenant hierarchy)
3. **Import data to new database** (PostgreSQL restore)
4. **Verify data integrity** (run tests, manual verification)

**Who:** Manus (execute data migration).

**When:** After Phase 1.4 (Production Readiness).

**Duration:** 1-2 weeks.

---

### Step 6: Deprecate Old Codebase

**Action:** Deprecate the old codebase and infrastructure.

**Rationale:** Prevent confusion. Ensure all operators use the new codebase.

**How:**

1. **Shut down old infrastructure** (Fly.io, Vercel)
2. **Archive old database** (PostgreSQL backup)
3. **Update DNS** (point domains to new infrastructure)
4. **Update documentation** (remove references to old codebase)

**Who:** Manus (execute deprecation).

**When:** After Step 5 (data migration).

**Duration:** 1 week.

---

## What to Preserve vs. What to Discard

### PRESERVE (Reuse in New Codebase)

| Component | Rationale |
|-----------|-----------|
| **Multi-Tenant Schema Design** | Proven pattern. Reuse schema design. |
| **Audit Log Schema** | Proven pattern. Reuse schema design. |
| **RBAC Schema** | Proven pattern. Reuse schema design. |
| **Feature Flags & Entitlements Schema** | Proven pattern. Reuse schema design. |
| **Branding Schema** | Proven pattern. Reuse schema design. |
| **POS Offline-First Logic** | Proven pattern. Reuse offline-first logic (IndexedDB, sync). |
| **Receipt Verification Logic** | Proven pattern. Reuse receipt verification logic. |
| **Paystack Integration** | Proven pattern. Reuse Paystack integration. |

### DISCARD (Do Not Reuse)

| Component | Rationale |
|-----------|-----------|
| **Commerce-Only Core API** | Incompatible with meta-platform vision. Rebuild as industry-agnostic. |
| **Tenant-Centric Data Model** | Incompatible with partner-centric architecture. Rebuild with Partner → Tenant hierarchy. |
| **Vibecoding Governance** | Not being followed in practice. Discard. |
| **Premature Repository Splitting** | Over-engineered. Discard. Use monorepo instead. |
| **Shared Dev/Prod Infrastructure** | High blast-radius risk. Discard. Use isolated dev/prod infrastructure. |
| **"No Demo Mode" Invariant** | Contradicts reality. Discard. Acknowledge demo mode. |
| **Excessive "Canon Lock" Usage** | Creates artificial rigidity. Discard. |

---

## Migration Checklist

### Pre-Migration

- [ ] Founder approves Clean Slate strategy
- [ ] Founder finalizes all 15 Founder Decisions (Decision Table)
- [ ] Freeze current codebase (no new changes)
- [ ] Archive current codebase (move to `webwaka-archive`)
- [ ] Create new monorepo (`webwaka-platform`)

### Phase 1: Core Infrastructure

- [ ] Build Identity & Authentication (Clerk integration)
- [ ] Build Database & ORM (PostgreSQL via Neon, Prisma)
- [ ] Build Audit Logs (immutable audit trail)
- [ ] Build Roles & Permissions (RBAC)
- [ ] Build Multi-Tenant Isolation (tenant-scoped data)
- [ ] Build Feature Flags & Entitlements (capability gating)

### Phase 1: Partner-Centric Architecture

- [ ] Build Partner Entity (database schema)
- [ ] Build Partner CRUD API
- [ ] Build Partner → Tenant hierarchy
- [ ] Build Partner Portal (minimal)
- [ ] Build Client Organization Creation (Partners create Tenants)
- [ ] Build Partner-Level Branding

### Phase 1: Commerce Suites

- [ ] Build POS (offline-first, IndexedDB, sync)
- [ ] Build ParkHub (offline-first, IndexedDB, sync)
- [ ] Build SVM (online-only)
- [ ] Build MVM (online-only)

### Phase 1: Production Readiness

- [ ] Build CI/CD Pipeline (GitHub Actions)
- [ ] Build Monitoring & Alerting (Sentry, Fly.io metrics)
- [ ] Build Backup & Disaster Recovery (Neon backups)
- [ ] Build Security Hardening (rate limiting, CORS, SQL injection prevention)

### Post-Migration

- [ ] Migrate production data (if any)
- [ ] Verify data integrity
- [ ] Deprecate old codebase
- [ ] Shut down old infrastructure
- [ ] Update DNS
- [ ] Update documentation

---

## Rollback Plan

### If Migration Fails

**Action:** Rollback to the old codebase and infrastructure.

**How:**

1. **Restore old database** (from backup)
2. **Restart old infrastructure** (Fly.io, Vercel)
3. **Update DNS** (point domains to old infrastructure)
4. **Analyze failure** (what went wrong?)
5. **Revise migration plan** (fix issues)
6. **Retry migration** (after fixes)

**Who:** Manus (execute rollback).

**When:** If migration fails (data corruption, infrastructure outage, etc.).

---

## Summary: Transition Plan

| Step | Action | Duration |
|------|--------|----------|
| **Step 1** | Freeze current codebase | Immediate |
| **Step 2** | Archive current codebase | 1 day |
| **Step 3** | Create new monorepo | 1 day |
| **Step 4** | Build Phase 1 from scratch | 3-6 months |
| **Step 5** | Migrate data (if production data exists) | 1-2 weeks |
| **Step 6** | Deprecate old codebase | 1 week |

**Total Duration:** 3-6 months (assuming no production data to migrate).

---

**End of Transition Plan**
# CONFLICT REPORT: Prior Recommendations vs. Founder Directives

**Analysis Date:** 2026-01-26  
**Analyst:** Manus Re-Founding Phase (WRF-0 Authoritative)

---

## Purpose

This report identifies **all prior recommendations** from the original Blueprint that **conflict with the two Founder Non-Negotiable Directives**:

1. **AWS-First, Single-Bill Architecture**
2. **Design for Maximum Scale from Day One**

---

## Category 1: Tooling Conflicts (AWS-First Directive)

### Conflict 1.1: Clerk (Authentication)

**Original Recommendation:** KEEP Clerk (unless Founder has strong preference for alternative).

**Conflict:** Clerk is a third-party SaaS. AWS Cognito is the AWS-native alternative.

**Status:** ❌ **INVALID under AWS-First directive**

**Corrected Recommendation:** REPLACE with AWS Cognito.

---

### Conflict 1.2: Neon (Database)

**Original Recommendation:** KEEP PostgreSQL. RECONSIDER Neon as provider.

**Conflict:** Neon is a third-party SaaS. AWS RDS or AWS Aurora are the AWS-native alternatives.

**Status:** ❌ **INVALID under AWS-First directive**

**Corrected Recommendation:** REPLACE with AWS Aurora (PostgreSQL-compatible).

---

### Conflict 1.3: Fly.io (Backend Hosting)

**Original Recommendation:** KEEP Fly.io (unless Founder has strong preference for alternative).

**Conflict:** Fly.io is a third-party SaaS. AWS ECS or AWS Fargate are the AWS-native alternatives.

**Status:** ❌ **INVALID under AWS-First directive**

**Corrected Recommendation:** REPLACE with AWS Fargate.

---

### Conflict 1.4: Vercel (Frontend Hosting)

**Original Recommendation:** KEEP Vercel (unless Founder has strong preference for alternative).

**Conflict:** Vercel is a third-party SaaS. AWS Amplify or AWS S3 + CloudFront are the AWS-native alternatives.

**Status:** ❌ **INVALID under AWS-First directive**

**Corrected Recommendation:** REPLACE with AWS Amplify.

---

### Conflict 1.5: Resend (Email)

**Original Recommendation:** USE Resend (planned for email campaigns).

**Conflict:** Resend is a third-party SaaS. AWS SES is the AWS-native alternative.

**Status:** ❌ **INVALID under AWS-First directive**

**Corrected Recommendation:** USE AWS SES.

---

### Conflict 1.6: PostHog (Analytics)

**Original Recommendation:** USE PostHog (planned for product analytics).

**Conflict:** PostHog is a third-party SaaS. AWS CloudWatch + Athena are the AWS-native alternatives.

**Status:** ❌ **INVALID under AWS-First directive**

**Corrected Recommendation:** USE AWS CloudWatch + Athena.

---

### Conflict 1.7: Sentry (Error Tracking)

**Original Recommendation:** USE Sentry (planned for error tracking).

**Conflict:** Sentry is a third-party SaaS. AWS CloudWatch Logs + X-Ray are the AWS-native alternatives.

**Status:** ❌ **INVALID under AWS-First directive**

**Corrected Recommendation:** USE AWS CloudWatch + X-Ray.

---

## Category 2: Architecture Conflicts (Max-Scale-First Directive)

### Conflict 2.1: Tenant-Centric Architecture

**Original Recommendation:** Tenant-Centric architecture was presented as a valid option (Decision 3, Option A).

**Conflict:** Tenant-Centric architecture does not support **thousands of Partners** and **millions of Tenants**. Partner-Centric architecture is required.

**Status:** ❌ **INVALID under Max-Scale-First directive**

**Corrected Recommendation:** Partner-Centric architecture is the only valid option.

---

### Conflict 2.2: Vertical SaaS Model

**Original Recommendation:** Vertical SaaS model was presented as a valid option (Decision 1, Option A).

**Conflict:** Vertical SaaS model is not a **Platform for Building Platforms**. Meta-Platform model is required.

**Status:** ❌ **INVALID under Max-Scale-First directive**

**Corrected Recommendation:** Meta-Platform (Hybrid) is the only valid option.

---

### Conflict 2.3: Commerce-Only Scope

**Original Recommendation:** Commerce-Only scope was presented as a valid option (Decision 4, Option A).

**Conflict:** Commerce-Only scope does not support **industry-agnostic modules** and **composable primitives**. Multi-Industry scope is required.

**Status:** ❌ **INVALID under Max-Scale-First directive**

**Corrected Recommendation:** Multi-Industry scope (Composable Primitives + Phased Rollout) is the only valid option.

---

### Conflict 2.4: Super Admin-Only Tenant Provisioning

**Original Recommendation:** Super Admin-Only tenant provisioning was presented as a valid option (Decision 6, Option A).

**Conflict:** Super Admin-Only provisioning is a bottleneck and does not support **thousands of Partners** and **millions of Tenants**. Partner Self-Service provisioning is required.

**Status:** ❌ **INVALID under Max-Scale-First directive**

**Corrected Recommendation:** Partner Self-Service provisioning is the only valid option.

---

### Conflict 2.5: Single Domain (No Custom Domains)

**Original Recommendation:** Single Domain was presented as a valid option (Decision 7, Option A).

**Conflict:** Single Domain does not support **white-labeled SaaS resale**. Partner-Specific custom domains are required.

**Status:** ❌ **INVALID under Max-Scale-First directive**

**Corrected Recommendation:** Partner-Specific custom domains are the only valid option.

---

### Conflict 2.6: 24-Hour Offline-First

**Original Recommendation:** 24-Hour offline-first was presented as a valid option (Decision 9, Option A).

**Conflict:** 24-Hour offline is insufficient for **worst-case network conditions** (rural Nigeria, power outages). Indefinite offline is required.

**Status:** ⚠️ **RISKY under Max-Scale-First directive**

**Corrected Recommendation:** Indefinite offline is the recommended option.

---

### Conflict 2.7: Shared Dev/Prod Infrastructure

**Original Recommendation:** Shared dev/prod infrastructure was mentioned in the original Constitution.

**Conflict:** Shared dev/prod infrastructure has high blast-radius risk and does not support **enterprise-grade reliability**. Isolated dev/prod infrastructure is required.

**Status:** ❌ **INVALID under Max-Scale-First directive**

**Corrected Recommendation:** Isolated dev/prod infrastructure is the only valid option.

---

## Category 3: Governance Conflicts

### Conflict 3.1: Vibecoding Governance Model

**Original Recommendation:** The original Constitution defined a complex vibecoding governance model (Emergent-01, Lovable-01, Replit-01).

**Conflict:** This governance model was **not being followed in practice**. All recent execution prompts were executed by Manus.

**Status:** ❌ **DISCARDED** (not being followed in practice)

**Corrected Recommendation:** Manus-Only for Phase 1. Multi-Operator for Phase 2+ (if needed).

---

## Summary: Conflicts Identified

| Category | Conflict | Original Recommendation | Corrected Recommendation |
|----------|----------|--------------------------|--------------------------|
| **Tooling** | Clerk (Authentication) | KEEP Clerk | REPLACE with AWS Cognito |
| **Tooling** | Neon (Database) | KEEP Neon | REPLACE with AWS Aurora |
| **Tooling** | Fly.io (Backend Hosting) | KEEP Fly.io | REPLACE with AWS Fargate |
| **Tooling** | Vercel (Frontend Hosting) | KEEP Vercel | REPLACE with AWS Amplify |
| **Tooling** | Resend (Email) | USE Resend | USE AWS SES |
| **Tooling** | PostHog (Analytics) | USE PostHog | USE AWS CloudWatch + Athena |
| **Tooling** | Sentry (Error Tracking) | USE Sentry | USE AWS CloudWatch + X-Ray |
| **Architecture** | Tenant-Centric Architecture | Valid option | INVALID (Partner-Centric only) |
| **Architecture** | Vertical SaaS Model | Valid option | INVALID (Meta-Platform only) |
| **Architecture** | Commerce-Only Scope | Valid option | INVALID (Multi-Industry only) |
| **Architecture** | Super Admin-Only Provisioning | Valid option | INVALID (Partner Self-Service only) |
| **Architecture** | Single Domain | Valid option | INVALID (Partner-Specific only) |
| **Architecture** | 24-Hour Offline | Valid option | RISKY (Indefinite recommended) |
| **Architecture** | Shared Dev/Prod | Mentioned | INVALID (Isolated only) |
| **Governance** | Vibecoding Governance | Defined | DISCARDED (not being followed) |

---

## Conclusion

**15 conflicts identified.** All conflicts have been corrected in the updated Blueprint.

**The updated Blueprint is now fully aligned with the two Founder Non-Negotiable Directives:**

1. **AWS-First, Single-Bill Architecture**
2. **Design for Maximum Scale from Day One**

---

**End of Conflict Report**
