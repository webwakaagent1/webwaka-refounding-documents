# WebWaka Platform Re-Founding Blueprint

**Version:** 4.0 (Canonically Locked)
**Status:** 🔒 CANONICALLY LOCKED — This is now the single execution authority for all future WebWaka development.
**Finalization Date:** 2026-01-26

---

## Executive Summary

This document is the **final, authoritative WebWaka Platform Re-Founding Blueprint**. It integrates all 15 Canonically Locked Founder Decisions and the Founder Directives, including the PWA-first and offline-first invariants.

**What This Blueprint Delivers:**

1. **A clear, coherent, and authoritative vision** for WebWaka as a **Platform for Building Platforms** (meta-platform).
2. **A scalable, AWS-first architecture** designed for **maximum scale from day one** (1,000+ partners, 1M+ tenants, 100M+ users).
3. **A PWA-first and offline-first delivery model** designed for **Nigeria's mobile, intermittent-connectivity reality**.
4. **A strict, sequential build order** that manages complexity and ensures a solid foundation.
5. **A set of governance rules** that enforce the Foundational Assumptions and ensure consistency, quality, and compliance across all work.

**Key Tenets:**

1. **Platform-for-Platforms Vision** (not vertical SaaS)
2. **AWS-First, Single-Bill Architecture**
3. **Max-Scale-First Design**
4. **PWA-First by Default**
5. **Offline-First for Core Actions**
6. **Push Notifications as Core Platform Primitive**
7. **Recursive System Usage Principle**
8. **Partner Pricing Autonomy**
9. **Configurable Multi-Level Affiliate System**
10. **Composable Primitives Architecture**
11. **Tenant-Scoped Identity & Data Ownership**
12. **Shared Database + Row-Level Security**

This blueprint is the **single source of truth** for all future WebWaka development. All operators (Manus, Emergent, Replit) must align strictly with this document.

---

## Table of Contents

1. **Foundational Assumptions**
2. **Mobile-First & Offline-First Canon**
3. **PWA Platform Canon**
4. **Notification & Event Delivery Canon**
5. **Recursive Application Model**
6. **Clean Platform Architecture**
7. **Strict, Sequential Build Order**
8. **Governance & Operator Rules**
9. **Transition Plan**
10. **Founder Decision Table**
11. **Historical Analysis**

---

## SECTION 1: FOUNDATIONAL ASSUMPTIONS

**Status:** 🔒 CANONICALLY LOCKED — These assumptions are non-negotiable and govern all architecture, tooling, and execution decisions.

### Assumption #1: AWS-First, Single-Bill Architecture

**Statement:** WebWaka will be built AWS-first, with a strong preference for AWS-native services over third-party platforms wherever viable.

### Assumption #2: Max-Scale-First Design

**Statement:** WebWaka is designed for maximum scale from day one. Architecture is not phased; only implementation is.

### Assumption #3: Platform-for-Platforms Vision

**Statement:** WebWaka is not a vertical SaaS. It is a meta-platform that enables partners to build, brand, and resell their own SaaS businesses.

### Assumption #4: PWA-First by Default

**Statement:** Every dashboard, client app, and surface MUST be PWA-installable by default. No WebWaka surface is "web-only." Installability is a baseline requirement.

### Assumption #5: Offline-First for Core Actions

**Statement:** Offline capability is MANDATORY for core actions, not optional. Core actions must function offline and sync later. Graceful degradation is required where full offline is not possible.

### Assumption #6: Push Notifications as Core Platform Primitive

**Statement:** Push notifications are a first-class system primitive, recursively usable across all hierarchy levels. They are not a "nice-to-have" or UI feature.

### Assumption #7: Recursive System Usage Principle

**Statement:** Any system WebWaka uses internally must be available for partners and clients to use for their own platforms.

### Assumption #8: Partner Pricing Autonomy

**Statement:** Partners have full control over their pricing. They set their own retail prices for clients, independent of WebWaka's wholesale prices.

### Assumption #9: Configurable Multi-Level Affiliate System

**Statement:** The affiliate system is configurable per partner, per contract, per use case. Level depth is variable (up to 10 levels), not hardcoded.

### Assumption #10: Composable Primitives Architecture

**Statement:** WebWaka is built from composable primitives, not monolithic features. Primitives can be combined to create industry-specific suites.

### Assumption #11: Tenant-Scoped Identity & Data Ownership

**Statement:** User identity is tenant-scoped, not global. Tenants own their data and have full export rights.

### Assumption #12: Shared Database + Row-Level Security

**Statement:** All partners and clients share a single database with row-level security (RLS) for data isolation.

---

## SECTION 2: MOBILE-FIRST & OFFLINE-FIRST CANON

**Status:** 🔒 CANONICALLY LOCKED — Offline-first is a foundational architectural law, not an optional feature.

### Overview

WebWaka is designed from the ground up for **Nigeria's mobile, intermittent-connectivity reality**. The platform assumes that internet connectivity is **unreliable, expensive, and intermittent**, not ubiquitous. This is not a technical limitation to work around—it is the primary design constraint that shapes the entire architecture.

### Foundational Principle

**"Core actions must function offline and sync later. Graceful degradation is required where full offline is not possible."**

### Offline Guarantees for Core Actions

The following core actions **MUST** function offline and sync later when connectivity is restored:

1. **POS Transactions** (sales, refunds, inventory adjustments)
2. **Lead Capture & Onboarding** (form submissions, sign-ups)
3. **Inventory Updates** (stock adjustments, product additions, price changes)
4. **Affiliate Activity Logging** (referrals, sign-ups, conversions)
5. **Field Data Collection** (surveys, inspections, audits)

### Sync Strategies (High-Level)

- **Queueing:** All offline actions are queued locally in **IndexedDB**.
- **Background Sync:** The **Background Sync API** is used to automatically sync queued actions.
- **Conflict Resolution:** Strategies vary by action type (e.g., last-write-wins, additive).
- **Retry Mechanism:** Exponential backoff for failed syncs.

### Minimum Offline UX Expectations

- **Offline Mode Indicator:** Clear, persistent indicator that the user is offline.
- **Action Confirmation:** Immediate confirmation with a clear sync status.
- **Sync Progress Indicator:** Display sync progress when connectivity is restored.
- **Error Handling:** Notify user of failed syncs with options to retry or review.
- **Sync History:** Allow users to view a history of synced and pending actions.

---

## SECTION 3: PWA PLATFORM CANON

**Status:** 🔒 CANONICALLY LOCKED — PWA-first is a foundational architectural law, not an optional feature.

### Overview

WebWaka is a **PWA-first platform**. Every dashboard, client app, and surface MUST be PWA-installable by default. No WebWaka surface is "web-only." Installability is a baseline requirement, not a progressive enhancement.

### Foundational Principle

**"Every surface must be PWA-installable by default. Native apps are optional accelerators, not dependencies."**

### Installability Requirements

- **Web App Manifest:** Every surface MUST have a valid `manifest.json` file (dynamically generated for white-label surfaces).
- **Service Worker:** Every surface MUST have a registered service worker (offline support, background sync, push notifications).
- **HTTPS:** All surfaces MUST be served over HTTPS.
- **Responsive Design:** All surfaces MUST be responsive (mobile, tablet, desktop).

### Service Worker Expectations

- **Offline Support:** Cache critical assets for offline access.
- **Background Sync:** Sync offline actions when connectivity is restored.
- **Push Notifications:** Receive push notifications even when the PWA is closed.

### Cache Strategies (High-Level)

- **Cache-First:** For static assets (HTML, CSS, JS, images).
- **Network-First:** For API requests (dynamic data).
- **Stale-While-Revalidate:** For assets that change occasionally (product images, user avatars).

---

## SECTION 4: NOTIFICATION & EVENT DELIVERY CANON

**Status:** 🔒 CANONICALLY LOCKED — Push notifications are a first-class platform primitive, not a UI feature.

### Overview

Push notifications are a **first-class system primitive** in WebWaka, not a "nice-to-have" or UI feature. They are a core part of the event-driven architecture and are recursively usable across all hierarchy levels.

### Foundational Principle

**"Push notifications are a first-class system primitive, recursively usable across all hierarchy levels."**

### Event-Driven Architecture Implications

Push notifications are triggered by **events** in the system. WebWaka uses an **event-driven architecture** where events are published to **AWS EventBridge** and consumed by various services, including the push notification service.

### Notification Types

- **Transactional Notifications:** Triggered by user actions or system events.
- **Marketing Notifications:** Sent by partners or clients to promote products, services, or campaigns.
- **System Notifications:** Sent by WebWaka to inform users of platform updates, maintenance, or issues.
- **Reminder Notifications:** Sent to remind users of upcoming events, tasks, or deadlines.

### Integration with AWS SNS

WebWaka uses **AWS SNS (Simple Notification Service)** for push notification delivery.

---

## SECTION 5: RECURSIVE APPLICATION MODEL

**Status:** 🔒 CANONICALLY LOCKED — Recursive usage is a foundational architectural law, not an optional feature.

### Overview

WebWaka is a **Platform for Building Platforms** (meta-platform). This means that any system WebWaka uses internally MUST be available for partners and clients to use for their own platforms.

### Foundational Principle

**"Any system WebWaka uses internally must be available for partners and clients to use for their own platforms."**

### Recursive Usage Across Hierarchy Levels

Each hierarchy level (Super Admin, Partners, Clients, End Users) can use the same platform primitives that the level above them uses.

**Examples:**

- **CRM:** Super Admin manages partners, Partners manage clients, Clients manage end users.
- **Push Notifications:** Super Admin sends to partners, Partners send to clients, Clients send to end users.
- **PWA:** Super Admin Dashboard is a PWA, Partner Dashboard is a PWA, Client Dashboard is a PWA, End User Apps are PWAs.

### What This Means for Architecture

- **No Hard-Coded Assumptions:** All access is permission-based, not role-based.
- **Scoped Data Models:** All data models are scoped to the hierarchy level.
- **Permission-Based Access:** All platform primitives are permission-based.
- **White-Label Everything:** All surfaces are white-labelable.

---

## SECTION 6: CLEAN PLATFORM ARCHITECTURE

**Status:** 🔒 CANONICALLY LOCKED — This architecture is designed for maximum scale from day one.

### Overview

WebWaka's architecture is built on **composable primitives**, not monolithic features. Primitives can be combined to create industry-specific suites.

### Platform Primitives

- CRM Domain
- Automation Domain
- Communication Domain
- Billing Domain
- Affiliate Domain
- Site Builder Domain
- Reporting Domain
- PWA Domain
- Notification Domain

### Industry Suites

Industry suites are **combinations of primitives**:

- **POS Suite** = CRM + Billing + Inventory + Offline + PWA
- **CRM Suite** = CRM + Automation + Communication + PWA
- **Affiliate Suite** = Affiliate + Billing + Reporting + PWA

### AWS Services

- **Authentication:** AWS Cognito
- **Database:** AWS Aurora PostgreSQL
- **Backend Hosting:** AWS Fargate
- **Frontend Hosting:** AWS Amplify
- **Email:** AWS SES
- **SMS & Push Notifications:** AWS SNS
- **Storage:** AWS S3 + CloudFront
- **Background Jobs:** AWS Lambda
- **Events:** AWS EventBridge
- **Monitoring & Logging:** AWS CloudWatch

---

## SECTION 7: STRICT, SEQUENTIAL BUILD ORDER

**Status:** 🔒 CANONICALLY LOCKED — This build order is mandatory and may not be reordered without explicit Founder approval.

### Overview

WebWaka's architecture is designed for **maximum scale from day one**, but implementation is **phased** to manage complexity.

### Implementation Phases

- **Phase 1 (3-6 months):** Core Infrastructure + Commerce Suites
- **Phase 2 (6-9 months):** Composable Primitives + Affiliate System
- **Phase 3 (9-12 months):** Multi-Industry Expansion

### Phase 1.1: Core Infrastructure (Weeks 1-4)

- AWS Account Setup
- Authentication & Authorization (AWS Cognito)
- Database (AWS Aurora PostgreSQL)
- Backend Hosting (AWS Fargate)
- Frontend Hosting (AWS Amplify)
- PWA Infrastructure (Service Worker, Manifest, Push Notifications)
- Offline Sync Infrastructure (IndexedDB, Background Sync)
- Storage (AWS S3 + CloudFront)
- Email (AWS SES)
- SMS & Push Notifications (AWS SNS)
- Background Jobs (AWS Lambda)
- Events (AWS EventBridge)
- Monitoring & Logging (AWS CloudWatch)

### Phase 1.2: Core API Domains (Weeks 5-8)

- Tenant Management API
- User Management API
- Permission Management API
- Affiliate Management API
- Pricing Management API
- Notification Management API
- Offline Sync API
- File Upload API

### Phase 1.3: Super Admin Dashboard (Weeks 9-10)

- Super Admin Dashboard (PWA, Offline-Capable, Push-Enabled)

### Phase 1.4: Partner Dashboard (Weeks 11-12)

- Partner Dashboard (PWA, White-Labeled, Offline-Capable, Push-Enabled)

### Phase 1.5: POS Suite (Weeks 13-16)

- POS Suite (PWA, White-Labeled, Offline-First)

---

## SECTION 8: GOVERNANCE & OPERATOR RULES

**Status:** 🔒 CANONICALLY LOCKED — These governance rules are mandatory and apply to all operators (Manus, Emergent, Replit).

### Overview

This section defines the governance rules that all operators MUST follow when working on WebWaka.

### Key Governance Rules

1. **All Surfaces Must Be PWA-Installable**
2. **Service Workers Must Be Implemented**
3. **Offline-First for Core Actions**
4. **Minimum Offline UX Must Be Provided**
5. **Push Notifications Must Be Available Platform-Wide**
6. **Dynamic Manifest Generation for White-Label**
7. **All Platform Primitives Must Be Recursively Usable**
8. **No Hard-Coded Assumptions**
9. **All Data Models Must Be Scoped**
10. **All Surfaces Must Be White-Labelable**
11. **Sync Strategies Must Be Documented**
12. **Notifications Must Respect User Preferences**
13. **Notifications Must Include Required Fields**
14. **Build Order Must Be Followed**
15. **AWS-Native Services Must Be Preferred**

---

## SECTION 9: TRANSITION PLAN

**Status:** 🔒 CANONICALLY LOCKED — This transition plan is mandatory.

### Recommended Strategy: Clean Slate

- **Archive old code:** All existing code will be archived in a separate repository.
- **Build new:** A new monorepo will be created for the new platform.
- **Migrate data:** Data from the old platform will be migrated to the new platform.
- **Deprecate old code:** The old platform will be deprecated after a transition period.

### 8-Step Transition Plan

1. **Freeze old platform:** No new features will be added to the old platform.
2. **Archive old code:** Create a new repository for the old code.
3. **Create new monorepo:** Create a new monorepo for the new platform.
4. **Build Phase 1:** Build the core infrastructure and commerce suites.
5. **Migrate data:** Migrate data from the old platform to the new platform.
6. **Deprecate old code:** The old platform will be deprecated after a transition period.
7. **Rollback plan:** A rollback plan will be in place in case of migration failure.
8. **Communicate with users:** Users will be notified of the transition.

---

## SECTION 10: FOUNDER DECISION TABLE

**Status:** 🔒 CANONICALLY LOCKED — All 15 decisions are final execution authority.

### Overview

This section documents the 15 Canonically Locked Founder Decisions that underpin the entire WebWaka architecture.

### Locked Decisions

1. **Affiliate Hierarchy Data Model:** Closure Table (up to 10 levels)
2. **Affiliate Configuration Authority Hierarchy:** Hierarchical Override (Global → Partner → Contract → Org)
3. **Affiliate Commission Calculation Model:** Fixed Percentages
4. **Affiliate Payout Responsibility:** Platform-Managed Payouts
5. **Module Creation Authority:** Platform-Only in Phase 1, Partner-Extensible in Phase 2
6. **White-Label Branding Depth:** Full White-Label (Frontend + Backend)
7. **Partner Data Isolation Model:** Shared Database + Row-Level Security
8. **Pricing Authority Hierarchy:** Hierarchical Pricing (Partners set their own prices)
9. **Billing Model:** Centralized Billing
10. **Multi-Currency Support:** NGN-Only in Phase 1, Multi-Currency in Phase 2
11. **Cross-Platform User Identity:** Tenant-Scoped Identity
12. **Tenant Data Ownership & Export Rights:** Tenant Owns Data, Full Export Rights
13. **Platform Kill-Switch Authority:** Platform Kill-Switch
14. **Recursive System Usage Enforcement:** Recursive Usage for All Platform Primitives
15. **WebWaka vs. GoHighLevel Feature Parity Strategy:** Differentiation (Exceed GoHighLevel in Key Areas)

---

## SECTION 11: HISTORICAL ANALYSIS

**Status:** For context only.

### Overview

This section provides historical context on the evolution of the WebWaka platform, including:

- **Contradictions & Unresolved Tensions Analysis:** Analysis of the original WebWaka Constitution.
- **Idea Triage (KEEP/DISCARD/RECONSIDER):** Categorization of ideas from the original Constitution.
- **Tooling & Platform Re-Evaluation:** Re-evaluation of tooling and platform decisions.

---

## Conclusion

This document is the **final, authoritative WebWaka Platform Re-Founding Blueprint**. It is the single source of truth for all future WebWaka development. All operators must align strictly with this document.

This is not negotiable.
