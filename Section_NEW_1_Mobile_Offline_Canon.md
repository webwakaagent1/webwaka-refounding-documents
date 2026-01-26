# SECTION: MOBILE-FIRST & OFFLINE-FIRST CANON

**Status:** 🔒 CANONICALLY LOCKED — Offline-first is a foundational architectural law, not an optional feature.

---

## Overview

WebWaka is designed from the ground up for **Nigeria's mobile, intermittent-connectivity reality**. The platform assumes that internet connectivity is **unreliable, expensive, and intermittent**, not ubiquitous. This is not a technical limitation to work around—it is the primary design constraint that shapes the entire architecture.

Offline-first is not a feature. It is a **mandatory architectural pattern** that applies to all core actions across the platform. Any surface that does not support offline-first for its core actions is considered incomplete and non-compliant with the WebWaka architecture.

---

## Foundational Principle

**"Core actions must function offline and sync later. Graceful degradation is required where full offline is not possible."**

This principle applies recursively across all hierarchy levels:

- **Super Admin** → Can perform core platform management actions offline
- **Partners** → Can perform core partner management actions offline
- **Clients** → Can perform core client management actions offline
- **End Users** → Can perform core end-user actions offline

---

## Offline Guarantees for Core Actions

The following core actions **MUST** function offline and sync later when connectivity is restored:

### 1. **POS Transactions** (Point of Sale)

**Offline Guarantee:** All POS transactions (sales, refunds, inventory adjustments) must be recordable offline.

**Sync Strategy:**
- Transactions are queued locally in IndexedDB
- When connectivity is restored, transactions are synced to the server in chronological order
- Conflict resolution: Server timestamp wins for inventory conflicts; local transaction is preserved but flagged for review

**Minimum Offline UX:**
- Clear "Offline Mode" indicator
- Transaction confirmation with "Pending Sync" status
- Sync progress indicator when connectivity is restored
- Error handling for failed syncs with retry mechanism

---

### 2. **Lead Capture & Onboarding**

**Offline Guarantee:** All lead capture forms and onboarding workflows must be completable offline.

**Sync Strategy:**
- Form submissions are queued locally in IndexedDB
- When connectivity is restored, submissions are synced to the server
- Conflict resolution: Duplicate detection based on email/phone; server merges duplicates

**Minimum Offline UX:**
- Form submission confirmation with "Pending Sync" status
- Sync progress indicator when connectivity is restored
- Error handling for failed syncs with retry mechanism

---

### 3. **Inventory Updates**

**Offline Guarantee:** All inventory updates (stock adjustments, product additions, price changes) must be recordable offline.

**Sync Strategy:**
- Updates are queued locally in IndexedDB
- When connectivity is restored, updates are synced to the server in chronological order
- Conflict resolution: Last-write-wins for price changes; additive for stock adjustments (e.g., +10 units offline, +5 units online = +15 units total)

**Minimum Offline UX:**
- Update confirmation with "Pending Sync" status
- Sync progress indicator when connectivity is restored
- Error handling for failed syncs with retry mechanism

---

### 4. **Affiliate Activity Logging**

**Offline Guarantee:** All affiliate activity (referrals, sign-ups, conversions) must be loggable offline.

**Sync Strategy:**
- Activity logs are queued locally in IndexedDB
- When connectivity is restored, logs are synced to the server in chronological order
- Conflict resolution: Duplicate detection based on activity ID; server deduplicates

**Minimum Offline UX:**
- Activity confirmation with "Pending Sync" status
- Sync progress indicator when connectivity is restored
- Error handling for failed syncs with retry mechanism

---

### 5. **Field Data Collection**

**Offline Guarantee:** All field data collection (surveys, inspections, audits) must be completable offline.

**Sync Strategy:**
- Data submissions are queued locally in IndexedDB
- When connectivity is restored, submissions are synced to the server
- Conflict resolution: No conflicts expected (field data is append-only)

**Minimum Offline UX:**
- Submission confirmation with "Pending Sync" status
- Sync progress indicator when connectivity is restored
- Error handling for failed syncs with retry mechanism

---

## Sync Strategies (High-Level)

### Queueing

All offline actions are queued locally in **IndexedDB** (browser-native, persistent storage). Each queued action includes:

- **Action Type** (e.g., "POS_TRANSACTION", "LEAD_CAPTURE")
- **Payload** (the data to be synced)
- **Timestamp** (when the action was performed offline)
- **Retry Count** (how many times sync has been attempted)
- **Status** ("PENDING", "SYNCING", "SYNCED", "FAILED")

### Background Sync

When connectivity is restored, the **Background Sync API** (Web API) is used to automatically sync queued actions in the background, even if the user has closed the PWA.

### Conflict Resolution

Conflict resolution strategies vary by action type:

| Action Type | Conflict Resolution Strategy |
|-------------|------------------------------|
| **POS Transactions** | Server timestamp wins for inventory conflicts; local transaction preserved but flagged |
| **Lead Capture** | Duplicate detection based on email/phone; server merges duplicates |
| **Inventory Updates** | Last-write-wins for price changes; additive for stock adjustments |
| **Affiliate Activity** | Duplicate detection based on activity ID; server deduplicates |
| **Field Data Collection** | No conflicts expected (append-only) |

### Retry Mechanism

If a sync fails (e.g., server error, network timeout), the action remains in the queue and is retried automatically:

- **Retry 1:** Immediate retry
- **Retry 2:** 5 seconds later
- **Retry 3:** 30 seconds later
- **Retry 4:** 5 minutes later
- **Retry 5+:** Exponential backoff (10 minutes, 30 minutes, 1 hour, etc.)

After 10 failed retries, the action is marked as "FAILED" and requires manual intervention (e.g., Super Admin review).

---

## Minimum Offline UX Expectations

All surfaces that support offline-first must provide the following minimum UX:

### 1. **Offline Mode Indicator**

A clear, persistent indicator that the user is offline (e.g., banner at the top of the screen: "You are offline. Actions will sync when connectivity is restored.").

### 2. **Action Confirmation with Sync Status**

When a user performs an offline action, they must receive immediate confirmation with a clear sync status (e.g., "Transaction saved. Pending sync.").

### 3. **Sync Progress Indicator**

When connectivity is restored, a sync progress indicator must be displayed (e.g., "Syncing 3 of 5 actions...").

### 4. **Error Handling for Failed Syncs**

If a sync fails, the user must be notified with a clear error message and options to retry or review the failed action (e.g., "Sync failed for 1 transaction. Tap to retry.").

### 5. **Sync History**

Users must be able to view a history of synced and pending actions (e.g., "Sync History" page showing all queued, syncing, synced, and failed actions).

---

## Graceful Degradation

Where full offline support is not possible (e.g., real-time collaboration, live chat), **graceful degradation** is required:

- **Inform the user:** Display a clear message that the feature requires connectivity (e.g., "Live chat requires an internet connection.").
- **Provide alternatives:** Offer an offline-capable alternative (e.g., "You can leave a message, and we'll respond when you're back online.").
- **Preserve context:** When connectivity is restored, restore the user's context (e.g., resume the live chat session where they left off).

---

## Nigeria's Mobile, Intermittent-Connectivity Reality

WebWaka is designed for Nigeria, where:

- **Mobile-first is the norm:** Most users access the internet via mobile devices, not desktops.
- **Connectivity is intermittent:** Users frequently experience dropped connections, slow networks, and limited data plans.
- **Data is expensive:** Users are cost-conscious and prefer apps that minimize data usage.
- **Offline is expected:** Users expect apps to work offline and sync later, not fail with "No internet connection" errors.

This reality shapes every architectural decision in WebWaka. Offline-first is not a "nice-to-have"—it is a **survival requirement** for the platform to succeed in Nigeria.

---

## Recursive Offline Patterns

Offline-first patterns are **recursively usable** across all hierarchy levels:

- **Super Admin** can perform platform management actions offline (e.g., create new partners, adjust global settings).
- **Partners** can perform partner management actions offline (e.g., create new clients, adjust pricing).
- **Clients** can perform client management actions offline (e.g., create new users, adjust inventory).
- **End Users** can perform end-user actions offline (e.g., make purchases, submit forms).

WebWaka does not just use offline-first patterns—it **provides offline-first as a platform primitive** that partners and clients can use to build their own offline-capable platforms.

---

## Governance & Enforcement

### Governance Rule #1: Offline-First for Core Actions

**Rule:** All core actions MUST function offline and sync later. Any surface that does not support offline-first for its core actions is considered incomplete and non-compliant.

**Enforcement:** All operators (Manus, Emergent, Replit) MUST validate offline support for core actions during development. Any PR that introduces a core action without offline support MUST be rejected.

### Governance Rule #2: Sync Strategies Must Be Documented

**Rule:** All offline actions MUST have a documented sync strategy (queueing, conflict resolution, retry mechanism).

**Enforcement:** All operators MUST document sync strategies in the codebase (e.g., in comments, README files, or architecture docs). Any PR that introduces an offline action without a documented sync strategy MUST be rejected.

### Governance Rule #3: Minimum Offline UX Must Be Provided

**Rule:** All surfaces that support offline-first MUST provide the minimum offline UX (offline mode indicator, action confirmation, sync progress indicator, error handling, sync history).

**Enforcement:** All operators MUST validate offline UX during development. Any PR that introduces offline support without the minimum offline UX MUST be rejected.

---

## Summary

Offline-first is a **foundational architectural law** of WebWaka, not an optional feature. It is designed for Nigeria's mobile, intermittent-connectivity reality and applies recursively across all hierarchy levels. All core actions MUST function offline and sync later, with clear sync strategies, conflict resolution, and minimum offline UX.

This is not negotiable.
