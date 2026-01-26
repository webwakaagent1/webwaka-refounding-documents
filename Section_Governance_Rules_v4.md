# SECTION: GOVERNANCE & OPERATOR RULES

**Status:** 🔒 CANONICALLY LOCKED — These governance rules are mandatory and apply to all operators (Manus, Emergent, Replit).

---

## Overview

This section defines the governance rules that all operators MUST follow when working on WebWaka. These rules enforce the Foundational Assumptions and ensure consistency, quality, and compliance across all work.

---

## Governance Rule #1: All Surfaces Must Be PWA-Installable

### Rule

**All WebWaka surfaces (dashboards, apps, landing pages) MUST be PWA-installable by default. Any surface that is not PWA-installable is considered incomplete and non-compliant.**

### Enforcement

- All operators MUST validate PWA installability during development
- All operators MUST test installability on mobile, tablet, and desktop devices
- Any PR that introduces a non-installable surface MUST be rejected

### Validation Checklist

- [ ] Valid `manifest.json` file exists
- [ ] Service worker registered and functional
- [ ] HTTPS enabled
- [ ] Responsive design (mobile, tablet, desktop)
- [ ] Install prompt appears on supported browsers
- [ ] App icon displays correctly after installation

---

## Governance Rule #2: Service Workers Must Be Implemented

### Rule

**All WebWaka surfaces MUST have a registered service worker that provides offline support, background sync, and push notifications.**

### Enforcement

- All operators MUST validate service worker implementation during development
- All operators MUST test offline support, background sync, and push notifications
- Any PR that introduces a surface without a service worker MUST be rejected

### Validation Checklist

- [ ] Service worker registered on page load
- [ ] Offline support enabled (cache-first for static assets, network-first for API requests)
- [ ] Background Sync API integrated (automatic syncing when connectivity is restored)
- [ ] Push API integrated (receive push notifications even when PWA is closed)
- [ ] Service worker lifecycle handled correctly (install, activate, fetch)
- [ ] Cache invalidation implemented (prompt user to update when new version is available)

---

## Governance Rule #3: Offline-First for Core Actions

### Rule

**All core actions MUST function offline and sync later. Any surface that does not support offline-first for its core actions is considered incomplete and non-compliant.**

### Core Actions

The following core actions MUST function offline:

1. POS Transactions (sales, refunds, inventory adjustments)
2. Lead Capture & Onboarding (form submissions, sign-ups)
3. Inventory Updates (stock adjustments, product additions, price changes)
4. Affiliate Activity Logging (referrals, sign-ups, conversions)
5. Field Data Collection (surveys, inspections, audits)

### Enforcement

- All operators MUST validate offline support for core actions during development
- All operators MUST test offline scenarios (airplane mode, slow network, intermittent connectivity)
- Any PR that introduces a core action without offline support MUST be rejected

### Validation Checklist

- [ ] Core actions are queueable in IndexedDB
- [ ] Sync strategy documented (queueing, conflict resolution, retry mechanism)
- [ ] Conflict resolution strategy implemented (per action type)
- [ ] Retry mechanism implemented (exponential backoff)
- [ ] Minimum offline UX provided (offline indicator, sync progress, error handling)

---

## Governance Rule #4: Minimum Offline UX Must Be Provided

### Rule

**All surfaces that support offline-first MUST provide the minimum offline UX (offline mode indicator, action confirmation, sync progress indicator, error handling, sync history).**

### Minimum Offline UX

- **Offline Mode Indicator:** Clear, persistent indicator that the user is offline
- **Action Confirmation:** Immediate confirmation when a user performs an offline action (e.g., "Transaction saved. Pending sync.")
- **Sync Progress Indicator:** Display sync progress when connectivity is restored (e.g., "Syncing 3 of 5 actions...")
- **Error Handling:** Notify user of failed syncs with options to retry or review (e.g., "Sync failed for 1 transaction. Tap to retry.")
- **Sync History:** Allow users to view a history of synced and pending actions

### Enforcement

- All operators MUST validate offline UX during development
- All operators MUST test offline UX on mobile devices (where offline scenarios are most common)
- Any PR that introduces offline support without the minimum offline UX MUST be rejected

### Validation Checklist

- [ ] Offline mode indicator displayed when offline
- [ ] Action confirmation displayed when offline action is performed
- [ ] Sync progress indicator displayed when syncing
- [ ] Error handling provided for failed syncs
- [ ] Sync history page available

---

## Governance Rule #5: Push Notifications Must Be Available Platform-Wide

### Rule

**Push notifications MUST be available as a platform primitive to all users at all hierarchy levels (Super Admin, Partners, Clients, End Users).**

### Enforcement

- All operators MUST validate that push notifications are available platform-wide during development
- All operators MUST test push notifications on all hierarchy levels
- Any PR that restricts push notifications to specific users or levels MUST be rejected

### Validation Checklist

- [ ] Push notification subscription API available (`/api/push/subscribe`)
- [ ] Push notification send API available (`/api/push/send`)
- [ ] Push notification preferences API available (`/api/push/preferences`)
- [ ] Push notifications integrated with AWS SNS
- [ ] Push notifications respect user preferences (enable/disable types, senders)
- [ ] Deep linking implemented (notification opens relevant page)

---

## Governance Rule #6: Dynamic Manifest Generation for White-Label

### Rule

**All white-labeled surfaces (Partner Dashboards, Client Dashboards) MUST dynamically generate the manifest to reflect the partner/client's branding.**

### Enforcement

- All operators MUST validate dynamic manifest generation during development
- All operators MUST test that the manifest reflects the correct branding (name, icons, theme color)
- Any PR that introduces a white-labeled surface without dynamic manifest generation MUST be rejected

### Validation Checklist

- [ ] Manifest API endpoint created (`/api/manifest.json`)
- [ ] Manifest dynamically generated per tenant (name, icons, theme color)
- [ ] Manifest includes correct branding for partner/client
- [ ] Install prompt displays correct branding

---

## Governance Rule #7: All Platform Primitives Must Be Recursively Usable

### Rule

**All platform primitives (CRM, Automation, Billing, Affiliate, Site Builder, Messaging, Reporting, PWA, Offline, Push Notifications) MUST be recursively usable across all hierarchy levels (Super Admin → Partners → Clients → End Users).**

### Enforcement

- All operators MUST validate recursive usage during development
- All operators MUST test that primitives are usable at all hierarchy levels
- Any PR that restricts a platform primitive to a specific hierarchy level MUST be rejected

### Validation Checklist

- [ ] Platform primitive is permission-based (not role-based)
- [ ] Platform primitive is scoped to hierarchy level (Super Admin, Partner, Client, End User)
- [ ] Platform primitive is white-labelable (if applicable)
- [ ] Platform primitive is available via API (for recursive usage)

---

## Governance Rule #8: No Hard-Coded Assumptions

### Rule

**The architecture MUST NOT hard-code assumptions about who uses what. All access MUST be permission-based, not role-based.**

### Enforcement

- All operators MUST validate that no hard-coded assumptions exist during development
- All operators MUST use permission-based access control (not role-based)
- Any PR that introduces hard-coded assumptions MUST be rejected

### Validation Checklist

- [ ] No hard-coded role checks (e.g., `if (user.role === "SUPER_ADMIN")`)
- [ ] Permission-based access control used (e.g., `if (user.hasPermission("SEND_PUSH_NOTIFICATIONS"))`)
- [ ] Permissions are scoped to hierarchy level (e.g., `SEND_PUSH_NOTIFICATIONS:PARTNER`)

---

## Governance Rule #9: All Data Models Must Be Scoped

### Rule

**All data models MUST be scoped to the hierarchy level (Super Admin, Partner, Client, End User).**

### Enforcement

- All operators MUST validate that data models are scoped during development
- All operators MUST include `tenant_id` or equivalent scoping field in all data models
- Any PR that introduces unscoped data models MUST be rejected

### Validation Checklist

- [ ] All tables include `tenant_id` or equivalent scoping field
- [ ] All queries filter by `tenant_id` to enforce data isolation
- [ ] Row-level security (RLS) policies implemented in the database

---

## Governance Rule #10: All Surfaces Must Be White-Labelable

### Rule

**All surfaces (dashboards, apps, landing pages) MUST be white-labelable so that partners and clients can brand them as their own.**

### Enforcement

- All operators MUST validate that surfaces are white-labelable during development
- All operators MUST test that branding (logo, colors, name) is correctly applied
- Any PR that introduces non-white-labelable surfaces MUST be rejected

### Validation Checklist

- [ ] Branding (logo, colors, name) is configurable per tenant
- [ ] Branding is dynamically applied on page load
- [ ] Branding is reflected in the PWA manifest (name, icons, theme color)

---

## Governance Rule #11: Sync Strategies Must Be Documented

### Rule

**All offline actions MUST have a documented sync strategy (queueing, conflict resolution, retry mechanism).**

### Enforcement

- All operators MUST document sync strategies in the codebase (e.g., in comments, README files, or architecture docs)
- All operators MUST test sync strategies in offline scenarios
- Any PR that introduces an offline action without a documented sync strategy MUST be rejected

### Validation Checklist

- [ ] Sync strategy documented (queueing, conflict resolution, retry mechanism)
- [ ] Sync strategy tested in offline scenarios
- [ ] Sync strategy handles edge cases (e.g., duplicate submissions, stale data)

---

## Governance Rule #12: Notifications Must Respect User Preferences

### Rule

**All push notifications MUST respect user notification preferences (enable/disable all, enable/disable types, enable/disable senders).**

### Enforcement

- All operators MUST validate that notification preferences are respected during development
- All operators MUST test that notifications are not sent when user has disabled them
- Any PR that sends notifications without respecting user preferences MUST be rejected

### Validation Checklist

- [ ] User notification preferences stored in database
- [ ] Push notification service checks user preferences before sending
- [ ] User can enable/disable all notifications
- [ ] User can enable/disable specific notification types (transactional, marketing, system, reminder)
- [ ] User can enable/disable notifications from specific senders (partners, clients)

---

## Governance Rule #13: Notifications Must Include Required Fields

### Rule

**All push notifications MUST include the required fields (title, body, icon).**

### Enforcement

- All operators MUST validate that notifications include required fields during development
- All operators MUST test that notifications display correctly on all devices
- Any PR that sends notifications without required fields MUST be rejected

### Validation Checklist

- [ ] Notification includes `title` field
- [ ] Notification includes `body` field
- [ ] Notification includes `icon` field
- [ ] Notification includes `data` field (for deep linking)
- [ ] Notification includes `actions` field (for action buttons, if applicable)

---

## Governance Rule #14: Build Order Must Be Followed

### Rule

**The build order MUST be followed strictly. No phase can be started until the previous phase is complete.**

### Enforcement

- All operators MUST validate that the previous phase is complete before starting the next phase
- All operators MUST document phase completion (e.g., in GitHub issues, project boards)
- Any PR that violates the build order MUST be rejected

### Validation Checklist

- [ ] Previous phase is complete (all deliverables met)
- [ ] Previous phase is documented (e.g., in GitHub issues, project boards)
- [ ] Current phase dependencies are met (e.g., infrastructure is operational before building APIs)

---

## Governance Rule #15: AWS-Native Services Must Be Preferred

### Rule

**AWS-native services MUST be preferred over third-party platforms wherever viable. Any third-party service MUST be explicitly justified.**

### Enforcement

- All operators MUST justify any third-party service in the PR description
- All operators MUST explain why AWS-native alternatives are insufficient
- Any PR that introduces a third-party service without justification MUST be rejected

### Justified Exceptions

- **Prisma (ORM):** No AWS-native alternative (application-level tool)
- **Africa's Talking (WhatsApp):** AWS does not provide WhatsApp messaging (required for Nigerian market)

### Validation Checklist

- [ ] Third-party service is justified in PR description
- [ ] AWS-native alternatives are evaluated and deemed insufficient
- [ ] Third-party service is cost-effective and scalable

---

## Operator-Specific Rules

### Manus (Primary Operator)

- **Role:** Primary operator for Phase 1 (Core Infrastructure + Commerce Suites)
- **Responsibilities:**
  - Set up AWS infrastructure
  - Build core API domains
  - Build Super Admin Dashboard, Partner Dashboard, POS Suite
  - Validate PWA, offline, and push notification support
- **Constraints:**
  - MUST follow the build order strictly
  - MUST validate all governance rules before submitting PRs
  - MUST document all work in GitHub (issues, PRs, project boards)

### Emergent (Secondary Operator, if needed)

- **Role:** Secondary operator for Phase 2+ (Composable Primitives + Multi-Industry Expansion)
- **Responsibilities:**
  - Build composable primitives (CRM, Automation, Affiliate)
  - Build industry suites (Education, Health, Civic)
  - Validate PWA, offline, and push notification support
- **Constraints:**
  - MUST follow the build order strictly
  - MUST validate all governance rules before submitting PRs
  - MUST coordinate with Manus to avoid conflicts

### Replit (Tertiary Operator, if needed)

- **Role:** Tertiary operator for specialized tasks (e.g., mobile-specific optimizations)
- **Responsibilities:**
  - Optimize PWA performance on mobile devices
  - Optimize offline sync performance
  - Optimize push notification delivery
- **Constraints:**
  - MUST follow the build order strictly
  - MUST validate all governance rules before submitting PRs
  - MUST coordinate with Manus and Emergent to avoid conflicts

---

## Summary

These governance rules are **mandatory** and apply to all operators (Manus, Emergent, Replit). They enforce the Foundational Assumptions and ensure that all work aligns with WebWaka's vision as a **Platform for Building Platforms**, designed from the ground up for **Nigeria's mobile, intermittent-connectivity reality**, with **PWA-first**, **offline-first**, and **push notifications** as core architectural laws.

All future work by all operators must align strictly with these governance rules.
