# SECTION 0: FOUNDATIONAL ASSUMPTIONS

**Status:** 🔒 CANONICALLY LOCKED — These assumptions are non-negotiable and govern all architecture, tooling, and execution decisions.

---

## Overview

WebWaka is a **Platform for Building Platforms** (meta-platform). It is infrastructure that enables Partners to build, brand, and resell their own SaaS businesses across multiple industries.

This section establishes the foundational assumptions that underpin the entire WebWaka architecture. These assumptions are derived from the 15 Canonically Locked Founder Decisions and the Founder Directives.

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
- ✅ **AWS Amplify** for frontend hosting (with PWA support)
- ✅ **AWS SES** for email
- ✅ **AWS SNS** for SMS and push notifications
- ✅ **AWS S3 + CloudFront** for storage
- ✅ **AWS CloudWatch + Athena** for analytics
- ✅ **AWS Lambda** for background jobs
- ✅ **AWS SQS** for queues
- ✅ **AWS EventBridge** for events

---

## Assumption #2: Max-Scale-First Design

### Statement

**WebWaka is designed for maximum scale from day one. Architecture is not phased; only implementation is.**

### Scale Assumptions

- **1,000+ Partners** (e.g., Acme Corp, Beta Inc, Gamma LLC)
- **1,000,000+ Tenants** (clients of partners)
- **100,000,000+ End Users** (customers of tenants)

### Implications

- All data models must support multi-tenancy at scale
- All APIs must be designed for horizontal scaling
- All infrastructure must be auto-scaling by default
- Performance bottlenecks must be identified and eliminated early

---

## Assumption #3: Platform-for-Platforms Vision

### Statement

**WebWaka is not a vertical SaaS. It is a meta-platform that enables partners to build, brand, and resell their own SaaS businesses.**

### What This Means

- **Partners are platform builders**, not just users
- **Clients are platform builders**, not just end users
- **WebWaka provides primitives**, not just features
- **Recursive usage is mandatory**, not optional

### Implications

- All platform primitives (CRM, Automation, Billing, Affiliate, PWA, Offline, Push Notifications) must be recursively usable
- All surfaces must be white-labelable
- All data models must be scoped to hierarchy levels
- All access must be permission-based, not role-based

---

## Assumption #4: PWA-First by Default

### Statement

**Every dashboard, client app, and surface MUST be PWA-installable by default. No WebWaka surface is "web-only." Installability is a baseline requirement.**

### Rationale

1. **Nigeria's Mobile Reality:** Most users access the internet via mobile devices
2. **App-Like Experience:** PWAs provide an app-like experience without app store friction
3. **Offline Capability:** PWAs enable offline-first patterns (see Assumption #5)
4. **Push Notifications:** PWAs enable push notifications (see Assumption #6)
5. **Cost-Effective:** PWAs eliminate the need for separate native apps

### Implications

- All surfaces must have a valid `manifest.json` file
- All surfaces must have a registered service worker
- All surfaces must be served over HTTPS
- All surfaces must be responsive (mobile, tablet, desktop)
- Native mobile apps (iOS, Android) are optional accelerators, not architectural dependencies

### Locked Decisions

- ✅ PWA is the primary delivery model
- ✅ Native apps are optional accelerators
- ✅ Dynamic manifest generation for white-label surfaces

---

## Assumption #5: Offline-First for Core Actions

### Statement

**Offline capability is MANDATORY for core actions, not optional. Core actions must function offline and sync later. Graceful degradation is required where full offline is not possible.**

### Core Offline Actions

The following core actions MUST function offline:

1. **POS Transactions** (sales, refunds, inventory adjustments)
2. **Lead Capture & Onboarding** (form submissions, sign-ups)
3. **Inventory Updates** (stock adjustments, product additions, price changes)
4. **Affiliate Activity Logging** (referrals, sign-ups, conversions)
5. **Field Data Collection** (surveys, inspections, audits)

### Rationale

1. **Nigeria's Intermittent Connectivity:** Internet connectivity is unreliable and expensive
2. **Business Continuity:** Businesses cannot afford to stop when the internet goes down
3. **User Expectations:** Users expect apps to work offline and sync later

### Implications

- All core actions must be queueable in IndexedDB
- All core actions must have a documented sync strategy
- All core actions must have a conflict resolution strategy
- All surfaces must provide minimum offline UX (offline indicator, sync progress, error handling)

### Locked Decisions

- ✅ Offline-first is mandatory for core actions
- ✅ Background Sync API for automatic syncing
- ✅ IndexedDB for local queueing
- ✅ Conflict resolution strategies documented per action type

---

## Assumption #6: Push Notifications as Core Platform Primitive

### Statement

**Push notifications are a first-class system primitive, recursively usable across all hierarchy levels. They are not a "nice-to-have" or UI feature.**

### Recursive Usage

- **Super Admin** → Can send push notifications to partners
- **Partners** → Can send push notifications to clients
- **Clients** → Can send push notifications to end users
- **End Users** → Can receive push notifications from clients

### Rationale

1. **Real-Time Communication:** Push notifications enable real-time communication with users
2. **Engagement:** Push notifications increase user engagement and retention
3. **Event-Driven Architecture:** Push notifications are part of the event-driven architecture

### Implications

- Push notifications must be available platform-wide
- Push notifications must be integrated with AWS SNS
- Push notifications must respect user notification preferences
- Push notifications must support deep linking

### Locked Decisions

- ✅ Push notifications are a platform primitive
- ✅ AWS SNS for push notification delivery
- ✅ Recursive usage across all hierarchy levels
- ✅ User notification preferences respected

---

## Assumption #7: Recursive System Usage Principle

### Statement

**Any system WebWaka uses internally must be available for partners and clients to use for their own platforms.**

### What This Means

All platform primitives are recursively usable:

- **CRM** → Super Admin manages partners, Partners manage clients, Clients manage end users
- **Automation** → Super Admin automates partner onboarding, Partners automate client onboarding, Clients automate end-user onboarding
- **Billing** → Super Admin bills partners, Partners bill clients, Clients bill end users
- **Affiliate** → Super Admin manages partner referrals, Partners manage client referrals, Clients manage end-user referrals
- **PWA** → Super Admin Dashboard is a PWA, Partner Dashboard is a PWA, Client Dashboard is a PWA, End User Apps are PWAs
- **Offline** → Super Admin can work offline, Partners can work offline, Clients can work offline, End Users can work offline
- **Push Notifications** → Super Admin sends push notifications, Partners send push notifications, Clients send push notifications

### Implications

- All platform primitives must be permission-based, not role-based
- All data models must be scoped to hierarchy levels
- All surfaces must be white-labelable
- No hard-coded assumptions about who uses what

### Locked Decisions

- ✅ All platform primitives are recursively usable
- ✅ Permission-based access (not role-based)
- ✅ Scoped data models (Super Admin, Partner, Client, End User)
- ✅ White-label everything

---

## Assumption #8: Partner Pricing Autonomy

### Statement

**Partners have full control over their pricing. They set their own retail prices for clients, independent of WebWaka's wholesale prices.**

### Hierarchical Pricing Model

- **Global Pricing** (WebWaka → Partners) → Wholesale prices set by WebWaka
- **Partner Pricing** (Partners → Clients) → Retail prices set by Partners
- **Client Pricing** (Clients → End Users) → Retail prices set by Clients (if applicable)

### Implications

- Pricing data models must support hierarchical overrides
- Partners must be able to configure their own pricing in the Partner Dashboard
- Clients must see the partner's retail prices, not WebWaka's wholesale prices

### Locked Decisions

- ✅ Hierarchical Pricing Model (Global → Partner → Client)
- ✅ Partners set their own retail prices
- ✅ Clients see partner's retail prices, not WebWaka's wholesale prices

---

## Assumption #9: Configurable Multi-Level Affiliate System

### Statement

**The affiliate system is configurable per partner, per contract, per use case. Level depth is variable (up to 10 levels), not hardcoded.**

### Hierarchical Override Model

- **Global Configuration** (WebWaka default) → Default affiliate logic
- **Partner Configuration** (per partner) → Partner-specific affiliate logic
- **Contract Configuration** (per contract) → Contract-specific affiliate logic
- **Org Configuration** (per org) → Org-specific affiliate logic

### Implications

- Affiliate data models must support variable depth (up to 10 levels)
- Affiliate logic must be configurable via hierarchical overrides
- Commission percentages must be configurable (not hardcoded)

### Locked Decisions

- ✅ Closure Table Pattern for variable depth (up to 10 levels)
- ✅ Hierarchical Override Model (Global → Partner → Contract → Org)
- ✅ Fixed Percentages (not cascading)
- ✅ Platform-Managed Payouts

---

## Assumption #10: Composable Primitives Architecture

### Statement

**WebWaka is built from composable primitives, not monolithic features. Primitives can be combined to create industry-specific suites.**

### Platform Primitives

- **CRM Domain** (contacts, leads, deals)
- **Automation Domain** (workflows, triggers, actions)
- **Communication Domain** (email, SMS, WhatsApp, push notifications)
- **Billing Domain** (invoicing, payments, subscriptions)
- **Affiliate Domain** (referrals, commissions, payouts)
- **Site Builder Domain** (landing pages, websites)
- **Reporting Domain** (analytics, dashboards, charts)
- **PWA Domain** (installability, offline, service workers)
- **Notification Domain** (push notifications, event-driven architecture)

### Industry Suites

Industry suites are **combinations of primitives**:

- **POS Suite** = CRM + Billing + Inventory + Offline + PWA
- **CRM Suite** = CRM + Automation + Communication + PWA
- **Affiliate Suite** = Affiliate + Billing + Reporting + PWA

### Implications

- All primitives must be independently buildable and testable
- All primitives must be composable (can be combined with other primitives)
- All suites are just configurations of primitives, not separate codebases

### Locked Decisions

- ✅ Composable Primitives Architecture
- ✅ Industry suites are combinations of primitives
- ✅ Primitives are independently buildable and testable

---

## Assumption #11: Tenant-Scoped Identity & Data Ownership

### Statement

**User identity is tenant-scoped, not global. Tenants own their data and have full export rights.**

### Tenant-Scoped Identity

- A user in Partner A's platform is a separate identity from the same user in Partner B's platform
- Users cannot log in to multiple partners' platforms with the same credentials (unless explicitly linked)

### Data Ownership

- **Tenants own their data** (partners own partner data, clients own client data)
- **Tenants have full export rights** (can export all their data at any time)
- **Platform has kill-switch authority** (Super Admin can disable a tenant for policy violations)

### Implications

- Identity data models must be scoped to tenants
- Data export APIs must be available to all tenants
- Platform kill-switch must be implemented for governance

### Locked Decisions

- ✅ Tenant-Scoped Identity
- ✅ Tenant owns data with full export rights
- ✅ Platform Kill-Switch (Super Admin authority)

---

## Assumption #12: Shared Database + Row-Level Security

### Statement

**All partners and clients share a single database with row-level security (RLS) for data isolation.**

### Rationale

1. **Operational Simplicity:** One database to manage, not thousands
2. **Cost Efficiency:** Shared infrastructure reduces costs
3. **Cross-Tenant Analytics:** Super Admin can analyze platform-wide data
4. **Scalability:** Proven at scale (e.g., Salesforce, Shopify)

### Implications

- All data models must include `tenant_id` or equivalent scoping field
- All queries must filter by `tenant_id` to enforce data isolation
- Row-level security policies must be implemented in the database

### Locked Decisions

- ✅ Shared Database + Row-Level Security
- ✅ All data models include `tenant_id`
- ✅ All queries filter by `tenant_id`

---

## Summary

These 12 Foundational Assumptions are **non-negotiable** and govern all architecture, tooling, and execution decisions. They reflect WebWaka's vision as a **Platform for Building Platforms**, designed from the ground up for **Nigeria's mobile, intermittent-connectivity reality**, with **PWA-first**, **offline-first**, and **push notifications** as core architectural laws.

All future work by all operators must align strictly with these assumptions.
