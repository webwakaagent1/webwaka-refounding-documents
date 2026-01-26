# SECTION 1: FOUNDATIONAL ASSUMPTIONS

**Status:** 🔒 CANONICALLY LOCKED — These assumptions are non-negotiable and govern all architecture, tooling, and execution decisions.

---

## Assumption #1: AWS-First, Single-Bill Architecture

**Statement:** WebWaka will be built AWS-first, with a strong preference for AWS-native services over third-party platforms wherever viable.

**Rationale:** AWS provides a comprehensive ecosystem of services that can scale to meet WebWaka's needs (1,000+ partners, 1M+ tenants, 100M+ users). Using AWS-native services ensures a single bill, simplified operations, and better cost control.

**Implications:**
- **Auth:** AWS Cognito (not Clerk)
- **Database:** AWS Aurora PostgreSQL (not Neon)
- **Backend Hosting:** AWS Fargate (not Fly.io)
- **Frontend Hosting:** AWS Amplify (not Vercel)
- **Email:** AWS SES (not Resend)
- **SMS:** AWS SNS (not Twilio)
- **Storage:** AWS S3 + CloudFront
- **Analytics:** AWS CloudWatch + Athena + QuickSight (not PostHog)
- **Background Jobs:** AWS Lambda
- **Error Tracking:** AWS CloudWatch + X-Ray (not Sentry)
- **Queues:** AWS SQS
- **Events:** AWS EventBridge
- **AI:** AWS Bedrock (primary), OpenAI (fallback)

**Exceptions:**
- **Prisma (ORM):** No AWS-native alternative (application-level tool)
- **Africa's Talking (WhatsApp):** AWS does not provide WhatsApp messaging (required for Nigerian market)

---

## Assumption #2: Max-Scale-First Design

**Statement:** WebWaka is designed for maximum scale from day one. Architecture is not phased; only implementation is.

**Rationale:** WebWaka is a Platform for Building Platforms. It must support 1,000+ partners, 1M+ tenants, and 100M+ users. Designing for scale from day one avoids costly refactoring later.

**Scale Assumptions:**
- **Partners:** 1,000+ (each with their own branding, pricing, and clients)
- **Tenants:** 1,000,000+ (each with their own data, users, and configuration)
- **End Users:** 100,000,000+ (across all tenants)
- **Transactions:** 1B+ per month (POS, orders, payments)
- **Events:** 10B+ per month (platform events, AI requests, notifications)

**Implications:**
- **Database:** Sharding strategy defined from day one
- **Caching:** Multi-layer caching (CDN, application, database)
- **Queues:** Asynchronous processing for all non-critical operations
- **Events:** Event-driven architecture for all meaningful actions
- **AI:** Multi-model support with cost attribution per tenant

---

## Assumption #3: Platform-for-Platforms Vision

**Statement:** WebWaka is not a vertical SaaS. It is a meta-platform that enables partners to build, brand, and resell their own SaaS businesses.

**Rationale:** WebWaka's business model is partner-led scale. Partners are the primary customers, not end users. Partners use WebWaka to build their own platforms for their clients.

**Hierarchy:**
```
Super Admin (WebWaka)
    │
    └─ Partners (1,000+)
            │
            └─ Clients (1M+)
                    │
                    └─ End Users (100M+)
```

**Implications:**
- **Partner Portal:** Partners have full control over branding, pricing, and configuration
- **White-Label:** Partners can fully white-label the platform (frontend + backend)
- **Recursive Systems:** Any system WebWaka uses internally must be available for partners and clients
- **Partner Autonomy:** Partners set their own prices, not WebWaka

---

## Assumption #4: PWA-First by Default

**Statement:** Every dashboard, client app, and surface MUST be PWA-installable by default. No WebWaka surface is "web-only." Installability is a baseline requirement.

**Rationale:** Nigeria's mobile-first reality requires PWA-first design. PWAs provide app-like experiences without app store friction, data costs, or device storage constraints.

**Implications:**
- **Super Admin Dashboard:** PWA-installable
- **Partner Portal:** PWA-installable
- **Client Dashboard:** PWA-installable
- **End-User Apps:** PWA-installable
- **Service Workers:** Mandatory for all surfaces
- **Manifest Files:** Mandatory for all surfaces
- **Installability:** Tested and enforced

---

## Assumption #5: Offline-First for Core Actions

**Statement:** Offline capability is MANDATORY for core actions, not optional. Core actions must function offline and sync later. Graceful degradation is required where full offline is not possible.

**Rationale:** Nigeria's intermittent connectivity reality requires offline-first design. Core actions (POS, lead capture, inventory, affiliate, field data) must work offline.

**Core Actions (Must Work Offline):**
1. **POS Transactions:** Record sales offline, sync when online
2. **Lead Capture:** Capture leads offline, sync when online
3. **Inventory Updates:** Update inventory offline, sync when online
4. **Affiliate Tracking:** Track referrals offline, sync when online
5. **Field Data Collection:** Collect data offline, sync when online

**Implications:**
- **Service Workers:** Cache core app logic and data
- **IndexedDB:** Store offline data locally
- **Background Sync:** Sync queued actions when online
- **Conflict Resolution:** Handle conflicts when syncing offline changes

---

## Assumption #6: Push Notifications as Core Platform Primitive

**Statement:** Push notifications are a first-class system primitive, recursively usable across all hierarchy levels. They are not a "nice-to-have" or UI feature.

**Rationale:** Push notifications are critical for engagement, retention, and real-time communication. They must be available at all hierarchy levels (Super Admin, Partner, Client, End User).

**Implications:**
- **Notification Domain:** First-class platform primitive
- **AWS SNS:** Push notification delivery
- **Recursive Usage:** Super Admin → Partner → Client → End User
- **Event-Driven:** Notifications triggered by platform events
- **Offline-Aware:** Notifications queued offline, sent when online

---

## Assumption #7: AI as Core Platform Primitive

**Statement:** AI is a first-class platform primitive, equal to Auth, Billing, and Affiliates. AI is not a feature; it is a core system that integrates with Events, Workflows, Permissions, and Cost Attribution.

**Rationale:** AI is critical for automation, intelligence, and partner differentiation. AI must be available at all hierarchy levels (Super Admin, Partner, Client, End User).

**Implications:**
- **AI Orchestration Layer:** One unified AI system (not separate bots)
- **Multi-Model Support:** AWS Bedrock (primary), OpenAI (fallback)
- **Role-Based Behavior:** AI respects permissions and roles
- **Event-Driven:** AI responds to platform events
- **Cost Attribution:** AI usage tracked per tenant
- **Offline-Aware:** AI queues requests offline, syncs when online
- **Recursive Usage:** Super Admin → Partner → Client → End User

---

## Assumption #8: Recursive System Usage Principle

**Statement:** Any system WebWaka uses internally must be available for partners and clients to use for their own platforms.

**Rationale:** WebWaka is a Platform for Building Platforms. Partners must be able to use the same systems WebWaka uses to build their own platforms for their clients.

**Examples:**
- **Workflow Builder:** WebWaka uses it → Partners use it → Clients use it
- **AI Agent:** WebWaka uses it → Partners use it → Clients use it
- **Affiliate System:** WebWaka uses it → Partners use it → Clients use it
- **Offline Sync:** WebWaka uses it → Partners use it → Clients use it

**Implications:**
- **No Internal-Only Shortcuts:** All systems must be recursive
- **Same Tools for All Levels:** Super Admin, Partner, Client, End User use the same tools
- **Role-Scoped, Not Hardcoded:** Systems adapt to role, not separate systems for each role

---

## Assumption #9: Partner Pricing Autonomy

**Statement:** Partners have full control over their pricing. They set their own retail prices for clients, independent of WebWaka's wholesale prices.

**Rationale:** Partners are the primary customers. They must be able to set their own prices to compete in their markets.

**Pricing Hierarchy:**
```
WebWaka → Wholesale Price → Partners
Partners → Retail Price → Clients
Clients → End-User Price → End Users
```

**Implications:**
- **Hierarchical Pricing Model:** Global → Partner → Contract → Org
- **Partner Autonomy:** Partners set their own prices
- **Profit Margin:** Partners keep the difference between wholesale and retail
- **Configurable:** Pricing can be configured per partner, per contract, per org

---

## Assumption #10: Configurable Multi-Level Affiliate System

**Statement:** The affiliate system is configurable per partner, per contract, per use case. Level depth is variable (up to 10 levels), not hardcoded.

**Rationale:** Different partners have different affiliate needs. Some need 3 levels, some need 10. The system must support variable depth.

**Implications:**
- **Closure Table Pattern:** Supports variable depth (up to 10 levels)
- **Hierarchical Override:** Global → Partner → Contract → Org
- **Configurable Commissions:** Each partner can have different commission percentages
- **Platform-Managed Payouts:** WebWaka manages payouts, not partners

---

## Assumption #11: Composable Primitives Architecture

**Statement:** WebWaka is built from composable primitives, not monolithic features. Primitives can be combined to create industry-specific suites.

**Rationale:** Composable primitives enable flexibility, extensibility, and future-proofing. New suites can be created by composing existing primitives.

**Platform Primitives:**
- Auth, AI, Billing, Affiliates, Notifications, Storage, Events, Workflows, Identity, Permissions, PWA, Offline Sync, CRM, Automation, Communication, Forms, Calendar, Integrations

**Suites (Compositions of Primitives):**
- **Commerce Suite:** POS + Site Builder + CRM + Affiliate + Analytics
- **Education Suite:** LMS + CRM + Billing + Communication
- **Health Suite:** Appointments + EHR + Billing + Communication
- **Civic Suite:** Forms + Workflows + Communication + Analytics

**Implications:**
- **No Monolithic Features:** All features are compositions of primitives
- **Reusable Primitives:** Primitives can be used in multiple suites
- **Extensible:** New suites can be created by composing primitives

---

## Assumption #12: Tenant-Scoped Identity & Data Ownership

**Statement:** User identity is tenant-scoped, not global. Each tenant owns its own data and has full export rights.

**Rationale:** Tenants must own their data and be able to export it at any time. User identity is scoped to tenants to ensure data isolation.

**Implications:**
- **Tenant-Scoped Identity:** Users are scoped to tenants (not global)
- **Data Ownership:** Tenants own their data
- **Export Rights:** Tenants can export their data at any time
- **Data Isolation:** Tenants cannot access other tenants' data

---

## Assumption #13: Shared Database + Row-Level Security

**Statement:** WebWaka uses a shared database with row-level security (RLS) for tenant isolation, not separate databases per tenant.

**Rationale:** Separate databases per tenant do not scale to 1M+ tenants. Shared database with RLS provides better performance, cost, and manageability.

**Implications:**
- **Shared Database:** All tenants share the same database
- **Row-Level Security:** Tenants can only access their own data
- **Tenant ID Column:** Every table has a `tenant_id` column
- **RLS Policies:** Enforced at the database level

---

## Assumption #14: Platform Kill-Switch Authority

**Statement:** WebWaka retains the authority to disable a partner or tenant account for fraud, abuse, or legal reasons.

**Rationale:** WebWaka must be able to protect the platform and other users from fraud, abuse, or illegal activity.

**Implications:**
- **Kill-Switch:** WebWaka can disable accounts
- **Audit Logs:** All kill-switch actions are logged
- **Appeal Process:** Partners and tenants can appeal

---

## Assumption #15: Platform Extensibility & Future-Proofing

**Statement:** Every system, module, service, UI, workflow, AI capability, and integration built today MUST be designed such that unknown future capabilities can be added later as plug-ins, without breaking, refactoring, or rewriting existing systems.

**Rationale:** WebWaka is designed to evolve for 10–20 years. The platform must be extensible, composable, and future-proof.

**Implications:**
- **No Closed Systems:** All systems must be extensible
- **Event-Driven Architecture:** All meaningful actions emit events
- **Contract-First Interfaces:** APIs, events, schemas are versioned and stable
- **Loose Coupling, Strong Contracts:** Systems know what to expect, not how others work
- **Capability-Based Design:** Features expose capabilities, not assumptions
- **Recursive Extensibility:** Extensibility available at all hierarchy levels

---

## Summary of Foundational Assumptions

| # | Assumption | Status |
|---|------------|--------|
| 1 | AWS-First, Single-Bill Architecture | 🔒 Locked |
| 2 | Max-Scale-First Design | 🔒 Locked |
| 3 | Platform-for-Platforms Vision | 🔒 Locked |
| 4 | PWA-First by Default | 🔒 Locked |
| 5 | Offline-First for Core Actions | 🔒 Locked |
| 6 | Push Notifications as Core Platform Primitive | 🔒 Locked |
| 7 | AI as Core Platform Primitive | 🔒 Locked |
| 8 | Recursive System Usage Principle | 🔒 Locked |
| 9 | Partner Pricing Autonomy | 🔒 Locked |
| 10 | Configurable Multi-Level Affiliate System | 🔒 Locked |
| 11 | Composable Primitives Architecture | 🔒 Locked |
| 12 | Tenant-Scoped Identity & Data Ownership | 🔒 Locked |
| 13 | Shared Database + Row-Level Security | 🔒 Locked |
| 14 | Platform Kill-Switch Authority | 🔒 Locked |
| 15 | Platform Extensibility & Future-Proofing | 🔒 Locked |

---

**End of Foundational Assumptions**
