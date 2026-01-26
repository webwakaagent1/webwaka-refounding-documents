# SECTION 0: FOUNDATIONAL ASSUMPTIONS

**Status:** 🔒 CANONICALLY LOCKED — These assumptions are non-negotiable and govern all architecture, tooling, and execution decisions.

---

## Overview

WebWaka is a **Platform for Building Platforms** (meta-platform). It is infrastructure that enables Partners to build, brand, and resell their own SaaS businesses across multiple industries.

This section establishes the foundational assumptions that underpin the entire WebWaka architecture. These assumptions are derived from the 15 Canonically Locked Founder Decisions and the two Founder Directives.

---

## Assumption #1: AWS-First, Single-Bill Architecture

### Statement

**WebWaka will be built AWS-first, with a strong preference for AWS-native services over third-party platforms wherever viable.**

### Rationale

1. **Single Consolidated Monthly Bill:** Cost visibility and control
2. **Reduced Platform Sprawl:** Fewer external dependencies
3. **Deeper Infrastructure Coherence:** Seamless AWS service integration
4. **Long-Term Scalability:** Proven at global, enterprise-grade scale
5. **Enterprise-Grade Reliability:** Industry-leading SLAs

### Implications

- All tooling decisions must justify why AWS-native options are insufficient
- Manus WILL be granted access to AWS accounts for setup and configuration
- AWS complexity is NOT a blocker

### Locked Decisions

- ✅ **AWS Cognito** for authentication
- ✅ **AWS Aurora** for database
- ✅ **AWS Fargate** for backend hosting
- ✅ **AWS Amplify** for frontend hosting
- ✅ **AWS SES** for email
- ✅ **AWS SNS** for SMS
- ✅ **AWS S3 + CloudFront** for storage
- ✅ **AWS CloudWatch + Athena** for analytics
- ✅ **AWS Lambda** for background jobs
- ✅ **AWS SQS** for queues
- ✅ **AWS EventBridge** for events

---

## Assumption #2: Max-Scale-First Design

### Statement

**The platform is designed for maximum scale from day one. Architecture is not phased; only implementation is.**

### Scale Assumptions

| Entity | Target Scale |
|--------|--------------|
| **Partners** | 1,000+ |
| **Tenants** | 1,000,000+ |
| **End Users** | 100,000,000+ |
| **Transactions/day** | 10,000,000+ |
| **API Requests/second** | 100,000+ |

### Implications

- No "we'll scale later" decisions
- All data models must support max scale
- All APIs must be stateless and horizontally scalable
- All databases must support sharding and replication
- All queues must support high throughput

### Locked Decisions

- ✅ **Partner-Centric Hierarchy** (not tenant-centric)
- ✅ **Shared Database + Row-Level Security** (not separate databases per partner)
- ✅ **Closure Table** for affiliate hierarchy (supports up to 10 levels)
- ✅ **Hierarchical Override** for configuration (Global → Partner → Contract → Org)

---

## Assumption #3: Platform-for-Platforms Vision

### Statement

**WebWaka is infrastructure for Partners who build, brand, and resell their own SaaS businesses. It is NOT a vertical SaaS for end users.**

### Target User

**Primary:** Partners (agencies, resellers, entrepreneurs)  
**Secondary:** Partner Clients (businesses using partner-branded solutions)  
**Tertiary:** End Users (employees/customers of partner clients)

### Value Proposition

Partners can:
- Build their own SaaS businesses without writing code
- White-label the entire platform (frontend + backend)
- Set their own pricing
- Configure multi-level affiliate systems
- Create industry-specific solutions from composable primitives

### Implications

- All UIs must be white-labelable
- All emails/notifications must be brandable
- All pricing must be partner-controlled
- All configuration must be hierarchical (Global → Partner → Client)

### Locked Decisions

- ✅ **Full White-Label** (Frontend + Backend)
- ✅ **Hierarchical Pricing** (Global → Partner → Client)
- ✅ **Centralized Billing** (Platform manages billing, partners set prices)
- ✅ **Platform-Managed Payouts** (for affiliate commissions)

---

## Assumption #4: Recursive System Usage Principle

### Statement

**ALL platform primitives must be recursively usable across all hierarchy levels. Anything WebWaka uses internally must be available to partners.**

### Hierarchy

**Super Admin** → **Partners** → **Partner Clients** → **End Users**

### Recursive Primitives

All of the following systems must be usable at every level:

1. **CRM** (Contact management, pipeline tracking)
2. **Automation** (Workflows, triggers, actions)
3. **Affiliate System** (Multi-level commissions, tracking)
4. **Site Builder** (Landing pages, funnels)
5. **Messaging** (Email, SMS, WhatsApp)
6. **Reporting** (Dashboards, analytics)
7. **Billing** (Invoicing, payments, subscriptions)

### Example

- **WebWaka** uses CRM to manage Partners
- **Partners** use the same CRM to manage their Clients
- **Partner Clients** use the same CRM to manage their End Users

### Implications

- No "admin-only" features
- No "partner-only" features
- All primitives must support multi-tenancy
- All primitives must support white-labeling

### Locked Decisions

- ✅ **Recursive for ALL platform primitives**
- ✅ **Super Admin → Partners → Clients → End Users** (all use same systems)

---

## Assumption #5: Partner Pricing Autonomy

### Statement

**Partners set their own retail prices. WebWaka sets wholesale prices. Partners control their own margins.**

### Pricing Authority Hierarchy

**Global (WebWaka)** → **Partner** → **Client**

- **WebWaka** sets wholesale prices (what partners pay)
- **Partners** set retail prices (what clients pay)
- **Clients** cannot set prices (they are end users)

### Example

| Entity | Role | Pricing Authority |
|--------|------|-------------------|
| **WebWaka** | Platform | Sets wholesale price: $50/month |
| **Partner A** | Reseller | Sets retail price: $99/month (margin: $49) |
| **Partner B** | Reseller | Sets retail price: $149/month (margin: $99) |
| **Client of Partner A** | End User | Pays $99/month (no pricing authority) |

### Implications

- Partners can compete on price
- Partners can bundle services
- Partners can offer discounts
- Partners can create custom pricing tiers

### Locked Decisions

- ✅ **Hierarchical Pricing** (Global → Partner → Client)
- ✅ **Partners set their own pricing**
- ✅ **Centralized Billing** (Platform manages billing, partners set prices)

---

## Assumption #6: Configurable Multi-Level Affiliate System

### Statement

**The affiliate system is a platform primitive that supports configurable, multi-level commission structures up to 10 levels deep.**

### Data Model

**Closure Table** (supports variable depth, efficient queries, long-term reporting)

### Configuration Authority

**Hierarchical Override:** Global → Partner → Contract → Org

- **Global:** WebWaka sets default affiliate configuration
- **Partner:** Partners can override global configuration
- **Contract:** Specific contracts can override partner configuration
- **Org:** Specific organizations can override contract configuration

**Conflict Resolution:** Most specific configuration wins

### Commission Calculation

**Fixed Percentages** (not cascading)

- Each level receives a fixed percentage of the transaction value
- Percentages are configurable per partner/contract
- Predictable, auditable, partner-friendly

### Example

| Level | Role | Commission |
|-------|------|------------|
| **Level 1** | Direct Referrer | 10% of transaction |
| **Level 2** | Referrer's Referrer | 5% of transaction |
| **Level 3** | Level 2's Referrer | 2% of transaction |

**Transaction:** $100  
**Level 1 earns:** $10  
**Level 2 earns:** $5  
**Level 3 earns:** $2  
**Platform keeps:** $83

### Payout Responsibility

**Platform-Managed Payouts**

- WebWaka calculates commissions
- WebWaka manages payouts
- Centralized control, compliance, trust

### Implications

- Affiliate system must be a core primitive (not an add-on)
- Affiliate configuration must be hierarchical
- Affiliate reporting must support up to 10 levels
- Affiliate payouts must be automated

### Locked Decisions

- ✅ **Closure Table** (up to 10 levels)
- ✅ **Hierarchical Override** (Global → Partner → Contract → Org)
- ✅ **Fixed Percentages** (not cascading)
- ✅ **Platform-Managed Payouts**

---

## Assumption #7: Composable Primitives Architecture

### Statement

**The platform consists of industry-agnostic primitives that Partners configure into vertical-specific solutions.**

### Core Primitives

1. **CRM Domain** (Contacts, Pipelines, Deals)
2. **Automation Domain** (Workflows, Triggers, Actions)
3. **Communication Domain** (Email, SMS, WhatsApp)
4. **Affiliate Domain** (Multi-level commissions, tracking)
5. **Site Builder Domain** (Landing pages, funnels)
6. **Forms Domain** (Lead capture, surveys)
7. **Calendar Domain** (Appointments, scheduling)
8. **Reporting Domain** (Dashboards, analytics)
9. **Billing Domain** (Invoicing, payments, subscriptions)
10. **API Gateway** (Webhooks, integrations)

### Industry Suites

Partners combine primitives into industry-specific suites:

- **Commerce Suite** (POS, Inventory, Orders)
- **Education Suite** (Courses, Students, Assignments)
- **Health Suite** (Patients, Appointments, Records)
- **Civic Suite** (Citizens, Services, Permits)
- **Hospitality Suite** (Reservations, Guests, Rooms)
- **Logistics Suite** (Shipments, Tracking, Routes)

### Implications

- Primitives must be industry-agnostic
- Primitives must be composable
- Suites are configurations of primitives (not separate codebases)

### Locked Decisions

- ✅ **Platform-only modules in Phase 1** (to ensure quality)
- ✅ **Partner-extensible in Phase 2** (with approval workflow)

---

## Assumption #8: Tenant-Scoped Identity & Data Ownership

### Statement

**User identity is tenant-scoped. Tenants own their data with full export rights.**

### Identity Model

**Tenant-Scoped Identity**

- Users belong to a specific tenant
- Users cannot cross tenant boundaries
- Same email can exist in multiple tenants (different users)

### Data Ownership

**Tenant Owns Data**

- Tenants own all data they create
- Tenants have full export rights
- Platform cannot use tenant data for other purposes

### Implications

- No global user directory
- No cross-tenant data sharing (without explicit API integration)
- All data exports must be provided in standard formats (JSON, CSV)

### Locked Decisions

- ✅ **Tenant-Scoped Identity**
- ✅ **Tenant owns data with full export rights**

---

## Assumption #9: Platform Kill-Switch Authority

### Statement

**WebWaka (Super Admin) retains kill-switch authority over all partners and tenants for compliance, abuse, and legal reasons.**

### Authority Hierarchy

**Super Admin** > **Partners** > **Clients** > **End Users**

- **Super Admin** can disable any partner or tenant
- **Partners** can disable their own clients
- **Clients** can disable their own end users

### Use Cases

- Fraud prevention
- Terms of Service violations
- Legal compliance (court orders, DMCA)
- Non-payment

### Implications

- All entities must have an "active" status flag
- All APIs must check "active" status before processing
- All UIs must show "account suspended" message when inactive

### Locked Decisions

- ✅ **Platform Kill-Switch** (Super Admin authority)

---

## Assumption #10: Differentiation Strategy (Not Parity)

### Statement

**WebWaka's strategy is to differentiate from GoHighLevel, not to achieve feature parity.**

### Positioning

**GoHighLevel + Platforms + Africa + Recursive Systems**

- **GoHighLevel:** Proven model (multi-industry, white-label, partner-centric)
- **+ Platforms:** Meta-platform (partners build platforms, not just use one)
- **+ Africa:** Africa-first (Nigeria, Kenya, South Africa, Ghana)
- **+ Recursive Systems:** Recursive primitives (anything WebWaka uses, partners can use)

### Differentiation

| Dimension | GoHighLevel | WebWaka |
|-----------|-------------|---------|
| **Target Market** | Global (US-centric) | Africa-first |
| **Partner Model** | Reseller | Platform Builder |
| **Extensibility** | Limited | Recursive (partners use same systems) |
| **Affiliate System** | Fixed | Configurable (up to 10 levels) |
| **Pricing Model** | Fixed tiers | Partner-controlled |
| **Module Creation** | Platform-only | Partner-extensible (Phase 2) |

### Implications

- Do NOT copy GoHighLevel features blindly
- Focus on Africa-specific needs (WhatsApp, mobile money, local languages)
- Focus on partner autonomy (pricing, branding, configuration)
- Focus on recursive systems (partners build platforms, not just use one)

### Locked Decisions

- ✅ **Differentiation** (not parity with GoHighLevel)

---

## Assumption #11: Multi-Currency Architecture (NGN-First)

### Statement

**Phase 1 is NGN-only. Architecture must be multi-currency ready from day one.**

### Phase 1

- **Currency:** NGN (Nigerian Naira) only
- **Payment Gateways:** Paystack, Flutterwave
- **Billing:** NGN-denominated invoices

### Phase 2+

- **Currencies:** USD, KES, ZAR, GHS, etc.
- **Payment Gateways:** Stripe, PayPal, local gateways
- **Billing:** Multi-currency invoices

### Implications

- All monetary values must be stored with currency code
- All APIs must accept currency parameter
- All UIs must display currency symbol
- All reports must support currency conversion

### Locked Decisions

- ✅ **NGN-only Phase 1** (multi-currency architecture)

---

## Assumption #12: Shared Database + Row-Level Security

### Statement

**All partners and tenants share a single database with row-level security (RLS) for data isolation.**

### Data Isolation Model

**Shared Database + Row-Level Security**

- All partners/tenants in same database
- Row-level security enforces data isolation
- Queries automatically filter by tenant_id

### Rationale

- **Scalability:** Easier to scale a single large database than thousands of small ones
- **Cost:** Lower infrastructure costs
- **Maintenance:** Easier to backup, migrate, and upgrade
- **Reporting:** Easier to run cross-tenant analytics (for platform, not partners)

### Implications

- All tables must have `tenant_id` or `partner_id` column
- All queries must filter by tenant/partner
- All APIs must enforce tenant/partner isolation
- Database must support row-level security (PostgreSQL RLS)

### Locked Decisions

- ✅ **Shared Database + Row-Level Security**

---

## Summary of Locked Decisions

| # | Decision | Status |
|---|----------|--------|
| **1** | Closure Table (up to 10 levels) | 🔒 Locked |
| **2** | Hierarchical Override (Global → Partner → Contract → Org) | 🔒 Locked |
| **3** | Fixed Percentages (not cascading) | 🔒 Locked |
| **4** | Platform-Managed Payouts | 🔒 Locked |
| **5** | Platform-only modules (Phase 1), Partner-extensible (Phase 2) | 🔒 Locked |
| **6** | Full White-Label (Frontend + Backend) | 🔒 Locked |
| **7** | Shared Database + Row-Level Security | 🔒 Locked |
| **8** | Hierarchical Pricing (Partners set their own prices) | 🔒 Locked |
| **9** | Centralized Billing | 🔒 Locked |
| **10** | NGN-only Phase 1 (multi-currency architecture) | 🔒 Locked |
| **11** | Tenant-Scoped Identity | 🔒 Locked |
| **12** | Tenant owns data with full export rights | 🔒 Locked |
| **13** | Platform Kill-Switch | 🔒 Locked |
| **14** | Recursive for ALL platform primitives | 🔒 Locked |
| **15** | Differentiation (not parity with GoHighLevel) | 🔒 Locked |

---

## Governance

**These assumptions are CANONICALLY LOCKED and may not be revisited without explicit Founder approval.**

All operators, developers, and contributors must:
- Read and understand these assumptions before starting work
- Align all decisions with these assumptions
- Escalate any conflicts to Founder immediately

---

*End of Section 0: Foundational Assumptions*
