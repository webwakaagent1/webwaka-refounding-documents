_**Document Status:** 🔒 CANONICALLY LOCKED — This document is the final execution authority for the WebWaka Platform._

# WebWaka Platform Re-Founding Blueprint (vNext)

**Document Version:** 3.0 (Canonically Locked)
**Finalization Date:** 2026-01-26
**Author:** Manus AI Operator

---

## Executive Summary

_This is the **final, authoritative, and Canonically Locked WebWaka Platform Re-Founding Blueprint**. It supersedes all previous blueprints, constitutions, and decision documents. All 15 Founder Decisions have been integrated, and this document now serves as the **single source of truth and execution authority** for all future development._

_This blueprint defines WebWaka as a **Platform for Building Platforms (meta-platform)**, architected from day one for maximum scale and built with a non-negotiable preference for AWS-native services. All architecture, tooling, and implementation decisions are constrained by the **12 Foundational Assumptions** outlined in Section 0, which are derived directly from the locked Founder Decisions._

_Key tenets of this final blueprint include:_

*   **_Canonically Locked Decisions:_** _All 15 critical decisions are final and may not be revisited without explicit Founder approval._
*   **_Recursive System Usage:_** _All platform primitives (CRM, Automation, Billing, etc.) are recursively usable across all hierarchy levels (Super Admin → Partners → Clients → End Users)._
*   **_Partner Autonomy:_** _Partners have full control over their branding (full white-label), pricing (hierarchical pricing), and affiliate systems (configurable up to 10 levels)._
*   **_Max-Scale-First Architecture:_** _The platform is designed for 1,000+ partners, 1,000,000+ tenants, and 100,000,000+ end users. Architecture is not phased; only implementation is._
*   **_AWS-Native Infrastructure:_** _All infrastructure is built on AWS-native services (Cognito, Aurora, Fargate, SES, etc.) to ensure a single consolidated bill and deep infrastructure coherence._
*   **_Strict, Sequential Build Order:_** _Implementation is phased (Infrastructure → Primitives → Suites) to manage complexity, but the architecture supports the full scope from day one._

_This document is the **new canonical execution authority**. All future work by all operators must align strictly with this blueprint._

---

## Table of Contents

0.  **Foundational Assumptions (🔒 Canonically Locked)**
1.  **Affiliate System Architecture**
2.  **Configuration & Pricing Authority**
3.  **Clean Platform Architecture (Target State)**
4.  **Strict, Sequential Build Order**
5.  **Governance & Operator Rules**
6.  **Transition Plan: From Current State to Target State**
7.  **Founder Decision Table (🔒 Canonically Locked)**
8.  **Historical Analysis (For Context Only)**
    *   8.1. Contradictions & Unresolved Tensions Analysis
    *   8.2. Idea Triage: KEEP / DISCARD / RECONSIDER
    *   8.3. Conflict Report: Prior Recommendations vs. Founder Directives

---

---

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


---

# SECTION 1 & 2 & 3: ARCHITECTURE (Affiliate, Config, Pricing, Platform)

# SECTION 5: CLEAN PLATFORM ARCHITECTURE (TARGET STATE)

**Status:** This section defines the target architecture that reflects all Canonically Locked Founder Decisions.

---

## Overview

WebWaka's architecture is designed as a **Platform for Building Platforms** (meta-platform). It consists of:

1. **Core Infrastructure** (AWS-native services)
2. **Platform Primitives** (industry-agnostic modules)
3. **Industry Suites** (vertical-specific configurations)
4. **Partner Portal** (white-label management)

All architecture decisions reflect the 12 Foundational Assumptions established in Section 0.

---

## 5.1 High-Level Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                        WEBWAKA PLATFORM                          │
│                    (Platform for Platforms)                      │
└─────────────────────────────────────────────────────────────────┘
                                 │
                    ┌────────────┴────────────┐
                    │                         │
              ┌─────▼─────┐           ┌──────▼──────┐
              │  PARTNERS │           │ SUPER ADMIN │
              │ (Resellers)│           │  (WebWaka)  │
              └─────┬─────┘           └──────┬──────┘
                    │                        │
         ┌──────────┴──────────┐            │
         │                     │            │
    ┌────▼────┐          ┌────▼────┐       │
    │ CLIENTS │          │ CLIENTS │       │
    │(Tenants)│          │(Tenants)│       │
    └────┬────┘          └────┬────┘       │
         │                     │            │
    ┌────▼────┐          ┌────▼────┐       │
    │END USERS│          │END USERS│       │
    └─────────┘          └─────────┘       │
                                            │
         ALL USE THE SAME PLATFORM PRIMITIVES
                (Recursive System Usage)
```

---

## 5.2 Core Infrastructure (AWS-Native)

### 5.2.1 Compute

| Service | Purpose | Justification |
|---------|---------|---------------|
| **AWS Fargate** | Backend API hosting | Serverless containers, auto-scaling, no server management |
| **AWS Lambda** | Background jobs, webhooks | Event-driven, pay-per-use, auto-scaling |
| **AWS Amplify** | Frontend hosting | Static site hosting, CDN, CI/CD |

### 5.2.2 Database

| Service | Purpose | Justification |
|---------|---------|---------------|
| **AWS Aurora (PostgreSQL)** | Primary database | Multi-AZ, auto-scaling, row-level security, max 128 TB |
| **AWS ElastiCache (Redis)** | Session cache, rate limiting | Sub-millisecond latency, auto-scaling |

**Data Isolation:** Shared Database + Row-Level Security (🔒 Locked Decision #7)

### 5.2.3 Authentication

| Service | Purpose | Justification |
|---------|---------|---------------|
| **AWS Cognito** | User authentication | Multi-tenancy, OAuth, SAML, MFA, user pools |

**Identity Model:** Tenant-Scoped Identity (🔒 Locked Decision #11)

### 5.2.4 Communication

| Service | Purpose | Justification |
|---------|---------|---------------|
| **AWS SES** | Transactional email | High deliverability, low cost, $0.10/1000 emails |
| **AWS SNS** | SMS notifications | Global coverage, pay-per-use |
| **Africa's Talking** | WhatsApp (Nigeria) | AWS does not provide WhatsApp messaging |

### 5.2.5 Storage

| Service | Purpose | Justification |
|---------|---------|---------------|
| **AWS S3** | Object storage | Unlimited storage, 99.999999999% durability |
| **AWS CloudFront** | CDN | Global edge network, low latency |

### 5.2.6 Queues & Events

| Service | Purpose | Justification |
|---------|---------|---------------|
| **AWS SQS** | Background job queues | Reliable, scalable, pay-per-use |
| **AWS EventBridge** | Event bus | Decoupled architecture, event-driven workflows |

### 5.2.7 Analytics & Monitoring

| Service | Purpose | Justification |
|---------|---------|---------------|
| **AWS CloudWatch** | Logs, metrics, alarms | Centralized monitoring, auto-scaling triggers |
| **AWS X-Ray** | Distributed tracing | Performance debugging, bottleneck identification |
| **AWS Athena** | Log analytics | SQL queries on S3 logs, pay-per-query |
| **AWS QuickSight** | Business intelligence | Dashboards, reports, embedded analytics |

---

## 5.3 Platform Primitives (Industry-Agnostic)

### 5.3.1 CRM Domain

**Purpose:** Contact management, pipeline tracking, deal management

**Recursive Usage:** ✅ (🔒 Locked Decision #14)
- Super Admin uses CRM to manage Partners
- Partners use CRM to manage Clients
- Clients use CRM to manage End Users

**Key Entities:**
- Contacts
- Companies
- Pipelines
- Deals
- Activities (calls, emails, meetings)

### 5.3.2 Automation Domain

**Purpose:** Workflows, triggers, actions

**Recursive Usage:** ✅ (🔒 Locked Decision #14)
- Super Admin automates partner onboarding
- Partners automate client onboarding
- Clients automate end user onboarding

**Key Entities:**
- Workflows
- Triggers (time-based, event-based)
- Actions (send email, create task, update field)
- Conditions (if/then logic)

### 5.3.3 Communication Domain

**Purpose:** Email, SMS, WhatsApp messaging

**Recursive Usage:** ✅ (🔒 Locked Decision #14)
- Super Admin sends emails to Partners
- Partners send emails to Clients
- Clients send emails to End Users

**Key Entities:**
- Email templates
- SMS templates
- WhatsApp templates
- Message logs
- Delivery status

### 5.3.4 Affiliate Domain

**Purpose:** Multi-level commission tracking and payouts

**Recursive Usage:** ✅ (🔒 Locked Decision #14)
- Super Admin tracks partner referrals
- Partners track client referrals
- Clients track end user referrals

**Data Model:** Closure Table (🔒 Locked Decision #1)

**Configuration Authority:** Hierarchical Override (🔒 Locked Decision #2)
- Global → Partner → Contract → Org

**Commission Calculation:** Fixed Percentages (🔒 Locked Decision #3)

**Payout Responsibility:** Platform-Managed (🔒 Locked Decision #4)

**Key Entities:**
- Affiliate relationships (closure table)
- Commission rules (hierarchical configuration)
- Commission transactions
- Payout schedules
- Payout history

**Closure Table Schema:**

```sql
CREATE TABLE affiliate_relationships (
  id UUID PRIMARY KEY,
  ancestor_id UUID NOT NULL,  -- The upline (referrer)
  descendant_id UUID NOT NULL, -- The downline (referred)
  depth INTEGER NOT NULL,      -- 0 = self, 1 = direct, 2 = 2nd level, etc.
  tenant_id UUID NOT NULL,
  created_at TIMESTAMP NOT NULL,
  UNIQUE(ancestor_id, descendant_id, tenant_id)
);

-- Indexes for efficient queries
CREATE INDEX idx_affiliate_ancestor ON affiliate_relationships(ancestor_id, depth);
CREATE INDEX idx_affiliate_descendant ON affiliate_relationships(descendant_id);
CREATE INDEX idx_affiliate_tenant ON affiliate_relationships(tenant_id);
```

**Commission Configuration Schema:**

```sql
CREATE TABLE affiliate_commission_configs (
  id UUID PRIMARY KEY,
  config_level TEXT NOT NULL,  -- 'global', 'partner', 'contract', 'org'
  entity_id UUID,              -- NULL for global, partner_id/contract_id/org_id otherwise
  level_depth INTEGER NOT NULL, -- 1, 2, 3, ..., 10
  commission_percentage DECIMAL(5,2) NOT NULL, -- e.g., 10.00 for 10%
  tenant_id UUID NOT NULL,
  created_at TIMESTAMP NOT NULL,
  updated_at TIMESTAMP NOT NULL,
  UNIQUE(config_level, entity_id, level_depth, tenant_id)
);
```

**Configuration Override Logic:**

```
1. Query all applicable configs (global, partner, contract, org)
2. Sort by specificity (org > contract > partner > global)
3. Use most specific config for each level depth
4. If no config exists for a level, use 0% commission
```

### 5.3.5 Site Builder Domain

**Purpose:** Landing pages, funnels, forms

**Recursive Usage:** ✅ (🔒 Locked Decision #14)
- Super Admin builds partner signup pages
- Partners build client signup pages
- Clients build end user signup pages

**Key Entities:**
- Pages
- Sections (hero, features, pricing, CTA)
- Forms (lead capture, surveys)
- Funnels (multi-step workflows)

### 5.3.6 Forms Domain

**Purpose:** Lead capture, surveys, data collection

**Recursive Usage:** ✅ (🔒 Locked Decision #14)

**Key Entities:**
- Form definitions
- Form submissions
- Field types (text, email, phone, dropdown, etc.)
- Validation rules

### 5.3.7 Calendar Domain

**Purpose:** Appointments, scheduling, availability

**Recursive Usage:** ✅ (🔒 Locked Decision #14)

**Key Entities:**
- Calendars
- Events
- Availability rules
- Booking links

### 5.3.8 Reporting Domain

**Purpose:** Dashboards, analytics, insights

**Recursive Usage:** ✅ (🔒 Locked Decision #14)
- Super Admin views partner metrics
- Partners view client metrics
- Clients view end user metrics

**Key Entities:**
- Dashboards
- Reports
- Metrics (KPIs)
- Data exports

### 5.3.9 Billing Domain

**Purpose:** Invoicing, payments, subscriptions

**Recursive Usage:** ✅ (🔒 Locked Decision #14)
- Super Admin bills Partners
- Partners bill Clients
- Clients bill End Users

**Billing Model:** Centralized Billing (🔒 Locked Decision #9)

**Pricing Authority:** Hierarchical Pricing (🔒 Locked Decision #8)
- Global (WebWaka) → Partner → Client

**Multi-Currency:** NGN-only Phase 1, multi-currency architecture (🔒 Locked Decision #10)

**Key Entities:**
- Pricing plans (hierarchical)
- Subscriptions
- Invoices
- Payments
- Payment methods

**Pricing Hierarchy Schema:**

```sql
CREATE TABLE pricing_plans (
  id UUID PRIMARY KEY,
  plan_level TEXT NOT NULL,  -- 'global', 'partner', 'client'
  entity_id UUID,            -- NULL for global, partner_id/client_id otherwise
  plan_name TEXT NOT NULL,
  base_price DECIMAL(10,2) NOT NULL,
  currency TEXT NOT NULL,    -- 'NGN', 'USD', etc.
  billing_cycle TEXT NOT NULL, -- 'monthly', 'annual'
  tenant_id UUID NOT NULL,
  created_at TIMESTAMP NOT NULL,
  updated_at TIMESTAMP NOT NULL
);
```

**Pricing Override Logic:**

```
1. Query all applicable pricing plans (global, partner, client)
2. Sort by specificity (client > partner > global)
3. Use most specific pricing plan
4. If no client-specific plan, use partner plan
5. If no partner-specific plan, use global plan
```

### 5.3.10 API Gateway

**Purpose:** Webhooks, integrations, external APIs

**Recursive Usage:** ✅ (🔒 Locked Decision #14)

**Key Entities:**
- API keys
- Webhooks
- Integrations (Zapier, Make, custom)
- API logs

---

## 5.4 Industry Suites (Vertical-Specific)

### 5.4.1 Suite Architecture

Industry suites are **configurations of platform primitives**, not separate codebases.

**Module Creation Authority:** (🔒 Locked Decision #5)
- **Phase 1:** Platform-only (to ensure quality and consistency)
- **Phase 2:** Partner-extensible (with approval workflow)

### 5.4.2 Commerce Suite

**Primitives Used:**
- CRM (customers, orders)
- Billing (invoicing, payments)
- Reporting (sales analytics)
- Automation (order fulfillment workflows)

**Additional Entities:**
- Products
- Inventory
- Orders
- Payments

### 5.4.3 Education Suite

**Primitives Used:**
- CRM (students, instructors)
- Site Builder (course landing pages)
- Forms (enrollment forms)
- Reporting (student progress)

**Additional Entities:**
- Courses
- Lessons
- Assignments
- Grades

### 5.4.4 Health Suite

**Primitives Used:**
- CRM (patients, providers)
- Calendar (appointments)
- Forms (intake forms)
- Reporting (patient outcomes)

**Additional Entities:**
- Patients
- Appointments
- Medical records
- Prescriptions

### 5.4.5 Civic Suite

**Primitives Used:**
- CRM (citizens, officials)
- Forms (service requests)
- Automation (approval workflows)
- Reporting (service metrics)

**Additional Entities:**
- Citizens
- Services
- Permits
- Requests

### 5.4.6 Hospitality Suite

**Primitives Used:**
- CRM (guests, staff)
- Calendar (reservations)
- Billing (room charges)
- Reporting (occupancy rates)

**Additional Entities:**
- Rooms
- Reservations
- Guests
- Amenities

### 5.4.7 Logistics Suite

**Primitives Used:**
- CRM (shippers, carriers)
- Automation (routing workflows)
- Reporting (delivery metrics)
- Forms (shipment requests)

**Additional Entities:**
- Shipments
- Routes
- Tracking events
- Carriers

---

## 5.5 Partner Portal (White-Label Management)

### 5.5.1 White-Label Depth

**Full White-Label** (🔒 Locked Decision #6)
- Frontend branding (logo, colors, fonts)
- Backend branding (emails, notifications, system messages)
- Custom domains (partner.example.com)
- Custom email domains (noreply@partner.example.com)

### 5.5.2 Partner Portal Features

**Tenant Management:**
- Create/edit/delete clients
- View client usage metrics
- Suspend/activate clients

**Pricing Management:**
- Set retail prices (🔒 Locked Decision #8)
- Create custom pricing plans
- View pricing hierarchy

**Affiliate Management:**
- Configure commission rules (🔒 Locked Decision #2)
- View affiliate hierarchy
- Manage payouts

**Branding Management:**
- Upload logo
- Set colors/fonts
- Configure custom domain
- Configure email templates

**Reporting:**
- Partner dashboard
- Client metrics
- Revenue reports
- Affiliate reports

### 5.5.3 Super Admin Portal

**Partner Management:**
- Create/edit/delete partners
- View partner usage metrics
- Suspend/activate partners (🔒 Locked Decision #13: Kill-Switch)

**Global Configuration:**
- Set wholesale prices
- Set global affiliate rules
- Set global branding defaults

**Platform Monitoring:**
- System health
- Usage metrics
- Error logs
- Performance metrics

---

## 5.6 Data Isolation & Security

### 5.6.1 Data Isolation Model

**Shared Database + Row-Level Security** (🔒 Locked Decision #7)

**PostgreSQL Row-Level Security (RLS) Example:**

```sql
-- Enable RLS on all tenant tables
ALTER TABLE contacts ENABLE ROW LEVEL SECURITY;

-- Create policy: Users can only see contacts in their tenant
CREATE POLICY tenant_isolation ON contacts
  FOR ALL
  USING (tenant_id = current_setting('app.current_tenant_id')::uuid);

-- Set tenant context in application
SET app.current_tenant_id = '<tenant_uuid>';
```

### 5.6.2 Data Ownership

**Tenant Owns Data** (🔒 Locked Decision #12)

**Export Rights:**
- Tenants can export all their data
- Export formats: JSON, CSV, SQL
- Export includes all entities (contacts, deals, invoices, etc.)

**Export API:**

```
POST /api/v1/exports
{
  "format": "json",
  "entities": ["contacts", "deals", "invoices"]
}

Response:
{
  "export_id": "uuid",
  "status": "processing",
  "download_url": null
}

GET /api/v1/exports/{export_id}
{
  "export_id": "uuid",
  "status": "completed",
  "download_url": "https://s3.amazonaws.com/exports/..."
}
```

---

## 5.7 Configuration Authority Hierarchy

### 5.7.1 Hierarchical Override Model

**Global → Partner → Contract → Org** (🔒 Locked Decision #2)

**Configuration Scope:**
- **Global:** WebWaka sets platform-wide defaults
- **Partner:** Partners override global settings for their entire organization
- **Contract:** Specific contracts override partner settings
- **Org:** Specific organizations override contract settings

**Conflict Resolution:**
- Most specific configuration wins
- If no specific configuration exists, inherit from parent level

**Example: Affiliate Commission Configuration**

| Level | Entity | Level 1 Commission | Level 2 Commission |
|-------|--------|--------------------|--------------------|
| **Global** | WebWaka | 10% | 5% |
| **Partner** | Partner A | 15% | 7% |
| **Contract** | Contract X | 20% | 10% |
| **Org** | Org Y | (inherit) | (inherit) |

**Result for Org Y:**
- Level 1: 20% (from Contract X)
- Level 2: 10% (from Contract X)

---

## 5.8 Deployment Architecture

### 5.8.1 AWS Services Deployment

```
┌─────────────────────────────────────────────────────────────────┐
│                        AWS CLOUD                                 │
└─────────────────────────────────────────────────────────────────┘
                                 │
                    ┌────────────┴────────────┐
                    │                         │
              ┌─────▼─────┐           ┌──────▼──────┐
              │ AWS Amplify│           │AWS CloudFront│
              │  (Frontend)│           │     (CDN)    │
              └─────┬─────┘           └──────┬──────┘
                    │                        │
                    └────────────┬───────────┘
                                 │
                          ┌──────▼──────┐
                          │ AWS Fargate │
                          │  (Backend)  │
                          └──────┬──────┘
                                 │
                    ┌────────────┴────────────┐
                    │                         │
              ┌─────▼─────┐           ┌──────▼──────┐
              │AWS Aurora │           │AWS Cognito  │
              │(Database) │           │    (Auth)   │
              └───────────┘           └─────────────┘
                    │
         ┌──────────┴──────────┐
         │                     │
    ┌────▼────┐          ┌────▼────┐
    │ AWS SQS │          │AWS Lambda│
    │(Queues) │          │  (Jobs)  │
    └─────────┘          └──────────┘
```

### 5.8.2 Scalability Assumptions

| Entity | Target Scale | Architectural Support |
|--------|--------------|----------------------|
| **Partners** | 1,000+ | Shared database + RLS, horizontal scaling |
| **Tenants** | 1,000,000+ | Shared database + RLS, database sharding |
| **End Users** | 100,000,000+ | Stateless APIs, horizontal scaling |
| **Transactions/day** | 10,000,000+ | AWS SQS + Lambda, auto-scaling |
| **API Requests/second** | 100,000+ | AWS Fargate auto-scaling, CloudFront CDN |

---

## 5.9 Architecture Principles

### Principle #1: Recursive System Usage

**ALL platform primitives must be recursively usable across all hierarchy levels.**

Super Admin → Partners → Clients → End Users (all use the same systems)

### Principle #2: Stateless APIs

**All APIs must be stateless to enable horizontal scaling.**

- No session state in API servers
- Session state stored in Redis (AWS ElastiCache)
- All requests include authentication token

### Principle #3: Event-Driven Architecture

**Use AWS EventBridge for decoupled, event-driven workflows.**

- Events: `user.created`, `invoice.paid`, `affiliate.referred`
- Subscribers: Lambda functions, SQS queues
- Benefits: Loose coupling, scalability, resilience

### Principle #4: Multi-Tenancy

**All data models must support multi-tenancy.**

- All tables include `tenant_id` or `partner_id`
- All queries filter by tenant/partner
- Row-level security enforces isolation

### Principle #5: White-Label by Default

**All UIs, emails, and notifications must be white-labelable.**

- No hardcoded branding
- All branding configurable per partner
- Custom domains supported

---

*End of Section 5: Clean Platform Architecture (Target State)*


---

# SECTION 4 & 5: BUILD ORDER & GOVERNANCE

# SECTION 6: STRICT, SEQUENTIAL BUILD ORDER

**Status:** This section defines the implementation phasing that respects all Canonically Locked Founder Decisions.

---

## 6.1 Build Order Philosophy

### Architecture vs. Implementation

**CRITICAL DISTINCTION:**

- **Architecture is designed for full scope from day one** (Max-Scale-First, 🔒 Locked)
- **Implementation is phased** (to manage complexity and risk)

**What This Means:**

- All data models support max scale (1,000+ partners, 1,000,000+ tenants)
- All APIs support full feature set (even if not all features are implemented yet)
- All infrastructure supports max load (even if current load is low)

**Implementation phases are about WHEN we build features, not HOW we architect them.**

---

## 6.2 Phase 1: Core Infrastructure + Commerce Suites (3-6 months)

### 6.2.1 Goals

- Establish AWS-native infrastructure
- Build core platform primitives
- Implement Commerce Suite (POS, Inventory, Orders)
- Launch MVP for Nigerian market

### 6.2.2 Prerequisites

**MUST be completed before Phase 1 starts:**

1. ✅ Founder Decisions finalized (DONE)
2. ✅ Blueprint approved (DONE)
3. AWS accounts provisioned
4. Domain names purchased
5. GitHub repository initialized

### 6.2.3 Phase 1 Deliverables

#### Infrastructure (AWS-Native)

| Component | Service | Status |
|-----------|---------|--------|
| **Backend Hosting** | AWS Fargate | Required |
| **Frontend Hosting** | AWS Amplify | Required |
| **Database** | AWS Aurora (PostgreSQL) | Required |
| **Authentication** | AWS Cognito | Required |
| **Email** | AWS SES | Required |
| **SMS** | AWS SNS | Required |
| **Storage** | AWS S3 + CloudFront | Required |
| **Queues** | AWS SQS | Required |
| **Events** | AWS EventBridge | Required |
| **Monitoring** | AWS CloudWatch + X-Ray | Required |

#### Platform Primitives (Phase 1 Subset)

| Primitive | Phase 1 Scope | Locked Decisions |
|-----------|---------------|------------------|
| **CRM Domain** | Contacts, Companies, basic pipeline | Recursive usage (🔒 #14) |
| **Billing Domain** | Invoicing, payments (NGN-only) | Centralized billing (🔒 #9), Hierarchical pricing (🔒 #8), NGN-only (🔒 #10) |
| **Affiliate Domain** | Closure table, fixed percentages, up to 10 levels | Closure table (🔒 #1), Hierarchical override (🔒 #2), Fixed percentages (🔒 #3), Platform-managed payouts (🔒 #4) |
| **Communication Domain** | Email templates, SMS (basic) | Recursive usage (🔒 #14) |
| **Reporting Domain** | Basic dashboards | Recursive usage (🔒 #14) |

#### Industry Suites (Phase 1)

| Suite | Phase 1 Scope | Locked Decisions |
|-------|---------------|------------------|
| **Commerce Suite** | POS, Inventory, Orders, Payments | Platform-only modules (🔒 #5) |

#### Partner Portal (Phase 1)

| Feature | Phase 1 Scope | Locked Decisions |
|---------|---------------|------------------|
| **Tenant Management** | Create/edit/delete clients | Platform kill-switch (🔒 #13) |
| **Pricing Management** | Set retail prices | Hierarchical pricing (🔒 #8) |
| **Branding Management** | Logo, colors, custom domain | Full white-label (🔒 #6) |
| **Affiliate Management** | Configure commission rules | Hierarchical override (🔒 #2) |

### 6.2.4 Phase 1 Constraints

**Module Creation Authority:** Platform-only (🔒 Locked Decision #5)

- Partners CANNOT create custom modules in Phase 1
- All modules are built by WebWaka
- This ensures quality, consistency, and security

**Currency:** NGN-only (🔒 Locked Decision #10)

- All pricing, billing, and payments in Nigerian Naira
- Architecture must be multi-currency ready (for Phase 2+)

**Data Isolation:** Shared Database + Row-Level Security (🔒 Locked Decision #7)

- All partners/tenants in same database
- PostgreSQL RLS enforces isolation

**Identity Model:** Tenant-Scoped Identity (🔒 Locked Decision #11)

- Users belong to specific tenants
- No global user directory

### 6.2.5 Phase 1 Success Criteria

1. ✅ 10+ partners onboarded
2. ✅ 100+ clients (tenants) active
3. ✅ 1,000+ end users
4. ✅ 10,000+ transactions processed
5. ✅ 99.9% uptime
6. ✅ All Canonically Locked decisions implemented

---

## 6.3 Phase 2: Composable Primitives + Affiliate System (6-9 months)

### 6.3.1 Goals

- Complete all platform primitives
- Enable partner extensibility (with approval)
- Expand to multi-currency
- Launch 3+ additional industry suites

### 6.3.2 Phase 2 Deliverables

#### Platform Primitives (Phase 2 Completion)

| Primitive | Phase 2 Scope | Locked Decisions |
|-----------|---------------|------------------|
| **Automation Domain** | Workflows, triggers, actions | Recursive usage (🔒 #14) |
| **Site Builder Domain** | Landing pages, funnels | Recursive usage (🔒 #14) |
| **Forms Domain** | Lead capture, surveys | Recursive usage (🔒 #14) |
| **Calendar Domain** | Appointments, scheduling | Recursive usage (🔒 #14) |
| **API Gateway** | Webhooks, integrations | Recursive usage (🔒 #14) |

#### Industry Suites (Phase 2)

| Suite | Phase 2 Scope | Locked Decisions |
|-------|---------------|------------------|
| **Education Suite** | Courses, Students, Assignments | Partner-extensible (🔒 #5) |
| **Health Suite** | Patients, Appointments, Records | Partner-extensible (🔒 #5) |
| **Civic Suite** | Citizens, Services, Permits | Partner-extensible (🔒 #5) |

#### Partner Extensibility (Phase 2)

**Module Creation Authority:** Partner-extensible (🔒 Locked Decision #5)

- Partners CAN create custom modules in Phase 2
- Approval workflow required (WebWaka reviews and approves)
- Module marketplace vision

**Approval Workflow:**

1. Partner submits module for review
2. WebWaka reviews code, security, quality
3. WebWaka approves or rejects
4. Approved modules published to marketplace
5. Other partners can install approved modules

#### Multi-Currency Support (Phase 2)

**Currency Expansion:** (🔒 Locked Decision #10)

- Add USD, KES, ZAR, GHS
- Multi-currency invoices
- Currency conversion
- Multi-currency reporting

### 6.3.3 Phase 2 Success Criteria

1. ✅ 50+ partners onboarded
2. ✅ 1,000+ clients (tenants) active
3. ✅ 100,000+ end users
4. ✅ 1,000,000+ transactions processed
5. ✅ 99.95% uptime
6. ✅ 10+ partner-created modules approved

---

## 6.4 Phase 3: Multi-Industry Expansion (9-12 months)

### 6.4.1 Goals

- Launch remaining industry suites
- Expand to additional African countries
- Enable advanced partner features
- Achieve GoHighLevel differentiation

### 6.4.2 Phase 3 Deliverables

#### Industry Suites (Phase 3)

| Suite | Phase 3 Scope |
|-------|---------------|
| **Hospitality Suite** | Reservations, Guests, Rooms |
| **Logistics Suite** | Shipments, Tracking, Routes |

#### Geographic Expansion

- Kenya (KES)
- South Africa (ZAR)
- Ghana (GHS)
- Additional African countries

#### Advanced Partner Features

- Partner-to-partner marketplace
- White-label mobile apps
- Advanced automation (AI-powered)
- Advanced reporting (predictive analytics)

### 6.4.3 Phase 3 Success Criteria

1. ✅ 200+ partners onboarded
2. ✅ 10,000+ clients (tenants) active
3. ✅ 1,000,000+ end users
4. ✅ 10,000,000+ transactions processed
5. ✅ 99.99% uptime
6. ✅ 100+ partner-created modules approved

---

## 6.5 Dependency Graph

### Structural Dependencies (MUST be built in order)

```
Phase 1.1: Core Infrastructure (AWS)
    ↓
Phase 1.2: Authentication & Multi-Tenancy (AWS Cognito + RLS)
    ↓
Phase 1.3: Core Primitives (CRM, Billing, Affiliate, Communication, Reporting)
    ↓
Phase 1.4: Commerce Suite (POS, Inventory, Orders)
    ↓
Phase 1.5: Partner Portal (Tenant Management, Pricing, Branding, Affiliate)
    ↓
Phase 2.1: Complete Primitives (Automation, Site Builder, Forms, Calendar, API Gateway)
    ↓
Phase 2.2: Additional Suites (Education, Health, Civic)
    ↓
Phase 2.3: Partner Extensibility (Module Marketplace)
    ↓
Phase 2.4: Multi-Currency Support
    ↓
Phase 3.1: Remaining Suites (Hospitality, Logistics)
    ↓
Phase 3.2: Geographic Expansion
    ↓
Phase 3.3: Advanced Partner Features
```

### Optional Dependencies (Can be built in parallel)

- **Reporting Domain** can be built in parallel with other primitives
- **Communication Domain** can be built in parallel with CRM
- **Industry Suites** can be built in parallel with each other (after primitives are complete)

---

## 6.6 Build Order Enforcement

### Forbidden Actions

**DO NOT:**

1. ❌ Build Phase 2 features before Phase 1 is complete
2. ❌ Enable partner extensibility before Phase 2
3. ❌ Add multi-currency support before Phase 2
4. ❌ Build industry suites before primitives are complete
5. ❌ Skip any structural dependencies

### Allowed Flexibility

**CAN:**

1. ✅ Build primitives in parallel (if no dependencies)
2. ✅ Build industry suites in parallel (after primitives are complete)
3. ✅ Adjust phase timelines based on progress
4. ✅ Add new features to existing phases (if no dependencies)

---

*End of Section 6: Strict, Sequential Build Order*

---

# SECTION 7: GOVERNANCE & OPERATOR RULES

**Status:** This section defines governance rules that enforce all Canonically Locked Founder Decisions.

---

## 7.1 Governance Principles

### Principle #1: Canonically Locked Decisions Are Final

**All 15 Founder Decisions are CANONICALLY LOCKED and may not be revisited without explicit Founder approval.**

**Operators MUST:**
- Read and understand all 15 decisions before starting work
- Align all implementation with locked decisions
- Escalate any conflicts to Founder immediately

**Operators MUST NOT:**
- Revisit locked decisions without Founder approval
- Implement alternatives to locked decisions
- Propose changes to locked decisions (without Founder approval)

### Principle #2: AWS-First, Always

**All tooling decisions must be AWS-first unless AWS-native options are insufficient.**

**Operators MUST:**
- Justify why AWS-native options are insufficient (if proposing third-party tools)
- Prioritize AWS-native services for all new features
- Consolidate all infrastructure costs on single AWS bill

**Operators MUST NOT:**
- Default to third-party SaaS for convenience
- Add new third-party tools without justification
- Ignore AWS-native alternatives

### Principle #3: Max-Scale-First, Always

**All architecture decisions must assume max scale from day one.**

**Operators MUST:**
- Design for 1,000+ partners, 1,000,000+ tenants, 100,000,000+ end users
- Ensure all data models support max scale
- Ensure all APIs are stateless and horizontally scalable

**Operators MUST NOT:**
- Make "we'll scale later" decisions
- Assume small-scale architecture is sufficient
- Ignore scalability implications

### Principle #4: Recursive System Usage, Always

**ALL platform primitives must be recursively usable across all hierarchy levels.**

**Operators MUST:**
- Ensure all primitives support multi-tenancy
- Ensure all primitives support white-labeling
- Ensure Super Admin → Partners → Clients → End Users all use same systems

**Operators MUST NOT:**
- Build "admin-only" features
- Build "partner-only" features
- Build non-recursive systems

---

## 7.2 Operator Roles & Responsibilities

### 7.2.1 Manus AI Operator

**Role:** Primary AI operator for WebWaka platform development

**Responsibilities:**
- Implement all features according to Blueprint
- Enforce all Canonically Locked decisions
- Commit all code/documents to GitHub immediately
- Escalate conflicts to Founder

**Authority:**
- Can implement any feature in current phase
- Can make technical decisions within locked constraints
- Cannot revisit locked decisions without Founder approval

### 7.2.2 Emergent AI Operator (if needed)

**Role:** Specialized AI operator for complex tasks

**Responsibilities:**
- Same as Manus AI Operator
- Focus on specialized tasks (e.g., complex algorithms, performance optimization)

**Authority:**
- Same as Manus AI Operator

### 7.2.3 Replit AI Operator (if needed)

**Role:** Specialized AI operator for rapid prototyping

**Responsibilities:**
- Same as Manus AI Operator
- Focus on rapid prototyping and experimentation

**Authority:**
- Same as Manus AI Operator

### 7.2.4 Founder

**Role:** Final decision authority

**Responsibilities:**
- Approve/reject all major decisions
- Resolve conflicts between operators
- Approve changes to Canonically Locked decisions

**Authority:**
- Can override any decision
- Can change any locked decision
- Can add/remove operators

---

## 7.3 Multi-Operator Coordination (if needed)

### 7.3.1 When to Use Multiple Operators

**Use multiple operators when:**
- Task requires specialized expertise (e.g., complex algorithms)
- Task requires parallel work (e.g., frontend + backend)
- Single operator is blocked or unavailable

**DO NOT use multiple operators when:**
- Task is simple and can be done by single operator
- Task requires deep context (single operator is better)
- Coordination overhead exceeds benefits

### 7.3.2 Coordination Rules

**If multiple operators are used:**
- Each operator must read entire Blueprint before starting
- Each operator must commit code/documents to GitHub immediately
- Each operator must communicate via GitHub issues/PRs
- Founder resolves conflicts between operators

---

## 7.4 Repository Structure & Workflow

### 7.4.1 Monorepo (Phase 1)

**Repository Structure:**

```
webwaka-platform/
├── docs/                    # All documentation
│   ├── blueprint.md         # This Blueprint
│   ├── architecture.md      # Architecture diagrams
│   └── decisions/           # Decision records
├── backend/                 # Backend API (Node.js + TypeScript)
│   ├── src/
│   ├── tests/
│   └── package.json
├── frontend/                # Frontend (React + TypeScript + TailwindCSS)
│   ├── src/
│   ├── tests/
│   └── package.json
├── infrastructure/          # AWS infrastructure (Terraform or CDK)
│   ├── terraform/
│   └── cdk/
├── database/                # Database migrations (Prisma or Drizzle)
│   ├── migrations/
│   └── schema.prisma
└── README.md
```

### 7.4.2 Git Workflow

**Branch Strategy:**

- **main:** Production-ready code
- **develop:** Development branch
- **feature/*:** Feature branches

**Commit Rules:**

- All commits must be pushed immediately before stopping work
- All commits must have descriptive messages
- All commits must reference related issues (if applicable)

**Pull Request Rules:**

- All PRs must be reviewed by Founder (or designated reviewer)
- All PRs must pass CI/CD checks
- All PRs must update documentation (if needed)

### 7.4.3 Documentation Discipline

**MANDATORY:**

- All documents must be committed to GitHub
- All code changes must be pushed immediately before stopping work
- All future operators must be able to reconstruct context from repo alone

**Operators MUST:**
- Update Blueprint when making architectural changes
- Update README when adding new features
- Update decision records when making decisions

**Operators MUST NOT:**
- Leave uncommitted work
- Leave undocumented decisions
- Leave incomplete features in main branch

---

## 7.5 Configuration Authority Enforcement

### 7.5.1 Hierarchical Override Rules

**Configuration Authority:** Global → Partner → Contract → Org (🔒 Locked Decision #2)

**Operators MUST:**
- Implement hierarchical override for ALL configurable settings
- Ensure most specific configuration wins
- Ensure configuration inheritance works correctly

**Operators MUST NOT:**
- Hardcode configuration values
- Ignore hierarchical override rules
- Allow configuration conflicts

### 7.5.2 Pricing Authority Enforcement

**Pricing Authority:** Global → Partner → Client (🔒 Locked Decision #8)

**Operators MUST:**
- Implement hierarchical pricing for ALL pricing plans
- Ensure partners can set their own retail prices
- Ensure pricing inheritance works correctly

**Operators MUST NOT:**
- Hardcode pricing values
- Ignore pricing authority rules
- Allow pricing conflicts

### 7.5.3 Affiliate Configuration Enforcement

**Affiliate Configuration:** Hierarchical Override (🔒 Locked Decision #2)

**Operators MUST:**
- Implement hierarchical override for affiliate commission rules
- Ensure most specific configuration wins
- Ensure affiliate configuration inheritance works correctly

**Operators MUST NOT:**
- Hardcode affiliate commission percentages
- Ignore affiliate configuration rules
- Allow affiliate configuration conflicts

---

## 7.6 Kill-Switch Authority Enforcement

**Platform Kill-Switch:** Super Admin authority (🔒 Locked Decision #13)

**Operators MUST:**
- Implement "active" status flag on all entities (partners, clients, users)
- Check "active" status before processing any request
- Display "account suspended" message when inactive

**Operators MUST NOT:**
- Allow inactive entities to access platform
- Allow inactive entities to process transactions
- Bypass "active" status checks

---

## 7.7 Data Ownership & Export Enforcement

**Data Ownership:** Tenant owns data with full export rights (🔒 Locked Decision #12)

**Operators MUST:**
- Implement data export API for all tenants
- Support JSON, CSV, SQL export formats
- Include all entities in exports (contacts, deals, invoices, etc.)

**Operators MUST NOT:**
- Restrict tenant data exports
- Use tenant data for other purposes (without consent)
- Deny export requests

---

## 7.8 Recursive System Usage Enforcement

**Recursive Usage:** ALL platform primitives (🔒 Locked Decision #14)

**Operators MUST:**
- Ensure all primitives support multi-tenancy
- Ensure all primitives support white-labeling
- Ensure Super Admin → Partners → Clients → End Users all use same systems

**Operators MUST NOT:**
- Build "admin-only" features
- Build "partner-only" features
- Build non-recursive systems

---

## 7.9 Conflict Resolution

### 7.9.1 Technical Conflicts

**If operators disagree on technical approach:**

1. Operators discuss and try to reach consensus
2. If no consensus, escalate to Founder
3. Founder makes final decision
4. All operators must align with Founder decision

### 7.9.2 Locked Decision Conflicts

**If implementation conflicts with locked decision:**

1. Operator must stop work immediately
2. Operator must escalate to Founder
3. Founder decides whether to:
   - Change implementation to align with locked decision
   - Change locked decision (rare)
4. Operator resumes work after Founder decision

### 7.9.3 AWS-First Conflicts

**If AWS-native option is insufficient:**

1. Operator must document why AWS-native option is insufficient
2. Operator must propose third-party alternative
3. Operator must escalate to Founder
4. Founder approves or rejects third-party alternative

---

## 7.10 Quality Standards

### 7.10.1 Code Quality

**Operators MUST:**
- Write clean, readable, maintainable code
- Follow TypeScript/JavaScript best practices
- Write unit tests for all business logic
- Write integration tests for all APIs

### 7.10.2 Documentation Quality

**Operators MUST:**
- Update documentation when making changes
- Write clear, concise, accurate documentation
- Include examples and diagrams where helpful

### 7.10.3 Performance Standards

**Operators MUST:**
- Ensure all APIs respond within 200ms (p95)
- Ensure all pages load within 2 seconds (p95)
- Ensure all background jobs complete within 5 minutes

### 7.10.4 Security Standards

**Operators MUST:**
- Follow OWASP Top 10 security guidelines
- Implement authentication/authorization correctly
- Sanitize all user inputs
- Use HTTPS for all communications

---

*End of Section 7: Governance & Operator Rules*


---

# SECTION 6: TRANSITION PLAN

_This section is carried over from the previous blueprint and remains the recommended transition strategy._

---

## 6.1 Recommended Strategy: Clean Slate

**The recommended transition strategy is a CLEAN SLATE.**

**Rationale:**

1.  **Avoid Architectural Debt:** The existing codebase is not aligned with the new architecture. Attempting to refactor it will be more costly and time-consuming than starting fresh.
2.  **Ensure Clean Foundation:** A clean slate ensures that all new code adheres to the new architecture and governance rules.
3.  **Reduce Risk:** A clean slate reduces the risk of introducing new bugs or security vulnerabilities from the old codebase.

**What This Means:**

-   Archive the existing codebase (for reference only)
-   Create a new monorepo for the new platform
-   Build the new platform from scratch, following the new blueprint
-   Migrate data from the old platform to the new platform
-   Deprecate the old platform

---

## 6.2 Transition Plan (8 Steps)

| Step | Action | Owner | Timeline |
|------|--------|-------|----------|
| **1** | **Freeze Old Platform** | Founder | Day 0 |
| **2** | **Archive Old Codebase** | Manus | Day 1 |
| **3** | **Create New Monorepo** | Manus | Day 1 |
| **4** | **Build Phase 1** | Manus | 3-6 months |
| **5** | **Migrate Data** | Manus | During Phase 1 |
| **6** | **Launch New Platform** | Founder | After Phase 1 |
| **7** | **Deprecate Old Platform** | Founder | After launch |
| **8** | **Archive Old Database** | Manus | After deprecation |

---

## 6.3 Migration Checklist

### Pre-Migration

-   [ ] Analyze old database schema
-   [ ] Map old schema to new schema
-   [ ] Write migration scripts
-   [ ] Test migration scripts on staging environment

### Phase 1 Migration

-   [ ] Migrate users
-   [ ] Migrate partners
-   [ ] Migrate tenants
-   [ ] Migrate products
-   [ ] Migrate orders
-   [ ] Migrate payments

### Post-Migration

-   [ ] Verify data integrity
-   [ ] Run smoke tests
-   [ ] Monitor for errors

---

## 6.4 Rollback Plan

**If migration fails:**

1.  **Stop migration**
2.  **Roll back database changes**
3.  **Investigate root cause**
4.  **Fix migration scripts**
5.  **Retry migration**

**If new platform fails after launch:**

1.  **Redirect traffic to old platform**
2.  **Investigate root cause**
3.  **Fix new platform**
4.  **Retry launch**

---

*End of Section 6: Transition Plan*

---

# SECTION 7: FOUNDER DECISION TABLE (🔒 CANONICALLY LOCKED)

_This section documents the 15 Canonically Locked Founder Decisions. These decisions are final and may not be revisited without explicit Founder approval._

---

| # | Decision | Status | Approved Option |
|---|----------|--------|-----------------|
| **1** | Affiliate Hierarchy Data Model | 🔒 Locked | Closure Table (up to 10 levels) |
| **2** | Affiliate Configuration Authority | 🔒 Locked | Hierarchical Override (Global → Partner → Contract → Org) |
| **3** | Commission Calculation Model | 🔒 Locked | Fixed Percentages (not cascading) |
| **4** | Payout Responsibility | 🔒 Locked | Platform-Managed Payouts |
| **5** | Module Creation Authority | 🔒 Locked | Platform-only (Phase 1), Partner-extensible (Phase 2) |
| **6** | White-Label Depth | 🔒 Locked | Full White-Label (Frontend + Backend) |
| **7** | Partner Data Isolation | 🔒 Locked | Shared Database + Row-Level Security |
| **8** | Pricing Authority | 🔒 Locked | Hierarchical Pricing (Global → Partner → Client) |
| **9** | Billing Model | 🔒 Locked | Centralized Billing |
| **10** | Multi-Currency | 🔒 Locked | NGN-only Phase 1 (multi-currency architecture) |
| **11** | User Identity Model | 🔒 Locked | Tenant-Scoped Identity |
| **12** | Data Ownership | 🔒 Locked | Tenant owns data with full export rights |
| **13** | Kill-Switch Authority | 🔒 Locked | Platform Kill-Switch (Super Admin) |
| **14** | Recursive System Usage | 🔒 Locked | Recursive for ALL platform primitives |
| **15** | GoHighLevel Strategy | 🔒 Locked | Differentiation, not parity |

---

*End of Section 7: Founder Decision Table*

---

# SECTION 8: HISTORICAL ANALYSIS (FOR CONTEXT ONLY)

_This section contains the historical analysis that led to the final blueprint. It is included for context only and is not part of the canonical execution authority._

---

## 8.1 Contradictions & Unresolved Tensions Analysis

_This analysis identified the 3 incompatible mental models that existed across the original Constitution, previous blueprint, and marketing site:_

-   **_Model A:_** _Vertical SaaS for Nigerian commerce_
-   **_Model B:_** _Modular multi-industry platform_
-   **_Model C:_** _Meta-platform for building SaaS platforms (GoHighLevel-class)_

_The final blueprint resolves these contradictions by adopting **Model C** as the canonical vision._

---

## 8.2 Idea Triage: KEEP / DISCARD / RECONSIDER

_This analysis categorized 36 ideas from previous documents into:_

-   **_18 KEEP_** _(validated patterns worth preserving)_
-   **_8 DISCARD_** _(mistakes, anti-patterns, dead ends)_
-   **_10 RECONSIDER_** _(good ideas requiring fresh decisions)_

_The final blueprint integrates all **KEEP** ideas and resolves all **RECONSIDER** ideas through the 15 Founder Decisions._

---

## 8.3 Conflict Report: Prior Recommendations vs. Founder Directives

_This analysis identified 15 conflicts between prior recommendations and the two Founder Directives (AWS-First, Max-Scale-First)._

_All conflicts have been corrected in the final blueprint, primarily by replacing third-party SaaS tools with AWS-native alternatives._

---

*End of Section 8: Historical Analysis*

---

_**End of Document**_
