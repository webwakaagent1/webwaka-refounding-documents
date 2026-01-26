# SECTION: RECURSIVE APPLICATION MODEL

**Status:** 🔒 CANONICALLY LOCKED — Recursive usage is a foundational architectural law, not an optional feature.

---

## Overview

WebWaka is a **Platform for Building Platforms** (meta-platform). This means that any system WebWaka uses internally MUST be available for partners and clients to use for their own platforms.

This is not a feature. This is a **foundational architectural law** that shapes every design decision in WebWaka.

---

## Foundational Principle

**"Any system WebWaka uses internally must be available for partners and clients to use for their own platforms."**

This principle applies to **all platform primitives**, including:

- **PWA** (Progressive Web Apps)
- **Offline-first patterns** (offline sync, background sync, conflict resolution)
- **Push notifications** (send notifications to users)
- **CRM** (manage customers, leads, contacts)
- **Automation** (workflows, triggers, actions)
- **Billing** (invoicing, payments, subscriptions)
- **Affiliate system** (multi-level referrals, commissions, payouts)
- **Site builder** (create landing pages, websites)
- **Messaging** (email, SMS, WhatsApp)
- **Reporting** (analytics, dashboards, charts)

---

## Recursive Usage Across Hierarchy Levels

### Hierarchy Levels

WebWaka has 4 hierarchy levels:

1. **Super Admin** (WebWaka)
2. **Partners** (e.g., Acme Corp)
3. **Clients** (e.g., John's Store)
4. **End Users** (e.g., customers of John's Store)

### Recursive Usage Model

Each level can use the same platform primitives that the level above them uses.

**Example: CRM**

- **Super Admin** uses CRM to manage partners (e.g., track partner sign-ups, onboarding status, revenue)
- **Partners** use CRM to manage clients (e.g., track client sign-ups, onboarding status, revenue)
- **Clients** use CRM to manage end users (e.g., track customer sign-ups, purchase history, support tickets)
- **End Users** do not use CRM (they are managed by clients)

**Example: Push Notifications**

- **Super Admin** sends push notifications to partners (e.g., "New feature released")
- **Partners** send push notifications to clients (e.g., "Your monthly invoice is ready")
- **Clients** send push notifications to end users (e.g., "Your order has been shipped")
- **End Users** receive push notifications from clients

**Example: PWA**

- **Super Admin Dashboard** is a PWA (installable, offline-capable, push-enabled)
- **Partner Dashboard** is a PWA (installable, offline-capable, push-enabled, white-labeled)
- **Client Dashboard** is a PWA (installable, offline-capable, push-enabled, white-labeled)
- **End User Apps** (POS, CRM, etc.) are PWAs (installable, offline-capable, push-enabled, white-labeled)

---

## What This Means for Architecture

### 1. **No Hard-Coded Assumptions**

The architecture MUST NOT hard-code assumptions about who uses what.

**Bad Example:**

```javascript
// Hard-coded assumption: only Super Admin can send push notifications
if (user.role === "SUPER_ADMIN") {
  sendPushNotification(notification);
}
```

**Good Example:**

```javascript
// Recursive usage: anyone with permission can send push notifications
if (user.hasPermission("SEND_PUSH_NOTIFICATIONS")) {
  sendPushNotification(notification);
}
```

---

### 2. **Scoped Data Models**

All data models MUST be scoped to the hierarchy level.

**Example: CRM Contacts**

- **Super Admin's contacts** → Partners (scoped to Super Admin)
- **Partner's contacts** → Clients (scoped to Partner)
- **Client's contacts** → End Users (scoped to Client)

**Data Model:**

```sql
CREATE TABLE contacts (
  id UUID PRIMARY KEY,
  owner_id UUID NOT NULL, -- Super Admin, Partner, or Client
  owner_type VARCHAR(50) NOT NULL, -- "SUPER_ADMIN", "PARTNER", "CLIENT"
  name VARCHAR(255) NOT NULL,
  email VARCHAR(255),
  phone VARCHAR(50),
  created_at TIMESTAMP DEFAULT NOW()
);
```

This allows each hierarchy level to have their own contacts, scoped to their level.

---

### 3. **Permission-Based Access**

All platform primitives MUST be permission-based, not role-based.

**Example: Push Notifications**

Instead of checking if the user is a Super Admin, check if the user has the `SEND_PUSH_NOTIFICATIONS` permission.

**Permission Model:**

```sql
CREATE TABLE permissions (
  id UUID PRIMARY KEY,
  user_id UUID NOT NULL,
  permission VARCHAR(100) NOT NULL, -- "SEND_PUSH_NOTIFICATIONS", "MANAGE_CRM", etc.
  scope VARCHAR(50) NOT NULL, -- "GLOBAL", "PARTNER", "CLIENT"
  created_at TIMESTAMP DEFAULT NOW()
);
```

This allows fine-grained control over who can use what, at what level.

---

### 4. **White-Label Everything**

All surfaces MUST be white-labelable so that partners and clients can brand them as their own.

**Example: Partner Dashboard**

When a partner logs in, they see their own branding:

- **Logo** → Partner's logo (uploaded by partner)
- **Colors** → Partner's brand colors (configured by partner)
- **Name** → Partner's brand name (e.g., "Acme Partner Dashboard")

When a partner's client logs in, they see the partner's branding (not WebWaka's):

- **Logo** → Partner's logo
- **Colors** → Partner's brand colors
- **Name** → Partner's brand name (e.g., "Acme Client Dashboard")

This ensures that WebWaka is invisible to end users. They only see the partner's brand.

---

## Examples of Recursive Usage

### Example 1: Multi-Level Affiliate System

**Super Admin** uses the affiliate system to manage partner referrals:

- Partner A refers Partner B → Partner A earns a commission on Partner B's revenue

**Partner** uses the affiliate system to manage client referrals:

- Client A refers Client B → Client A earns a commission on Client B's revenue

**Client** uses the affiliate system to manage end-user referrals:

- End User A refers End User B → End User A earns a commission on End User B's purchases

**All three levels use the same affiliate system**, just scoped to their level.

---

### Example 2: Site Builder

**Super Admin** uses the site builder to create landing pages for WebWaka:

- WebWaka marketing site
- WebWaka partner onboarding page

**Partner** uses the site builder to create landing pages for their clients:

- Partner's marketing site
- Partner's client onboarding page

**Client** uses the site builder to create landing pages for their end users:

- Client's product landing page
- Client's checkout page

**All three levels use the same site builder**, just scoped to their level.

---

### Example 3: Automation Workflows

**Super Admin** uses automation workflows to onboard partners:

- Trigger: Partner signs up
- Action: Send welcome email, create partner dashboard, assign onboarding tasks

**Partner** uses automation workflows to onboard clients:

- Trigger: Client signs up
- Action: Send welcome email, create client dashboard, assign onboarding tasks

**Client** uses automation workflows to onboard end users:

- Trigger: End user signs up
- Action: Send welcome email, create user account, send confirmation SMS

**All three levels use the same automation workflows**, just scoped to their level.

---

## What This Means for Partners

### Partners Are Not Just Users

Partners are not just users of WebWaka. They are **platform builders** who use WebWaka to build their own platforms.

**What Partners Can Do:**

- **Create their own branded dashboards** (white-labeled)
- **Create their own affiliate systems** (multi-level, configurable)
- **Create their own automation workflows** (onboarding, notifications, etc.)
- **Create their own site builders** (landing pages, websites)
- **Create their own CRM systems** (manage clients, leads, contacts)
- **Create their own billing systems** (invoicing, payments, subscriptions)
- **Create their own push notification systems** (send notifications to clients)

**What This Means:**

Partners can build **their own SaaS businesses** on top of WebWaka, without writing code. They use WebWaka's platform primitives to build their own platforms.

---

## What This Means for Clients

### Clients Are Not Just End Users

Clients are not just end users of a partner's platform. They are **platform builders** who use the partner's platform to build their own platforms.

**What Clients Can Do:**

- **Create their own branded apps** (white-labeled)
- **Create their own affiliate systems** (multi-level, configurable)
- **Create their own automation workflows** (onboarding, notifications, etc.)
- **Create their own site builders** (landing pages, websites)
- **Create their own CRM systems** (manage end users, leads, contacts)
- **Create their own billing systems** (invoicing, payments, subscriptions)
- **Create their own push notification systems** (send notifications to end users)

**What This Means:**

Clients can build **their own SaaS businesses** on top of the partner's platform, without writing code. They use the partner's platform primitives to build their own platforms.

---

## Governance & Enforcement

### Governance Rule #1: All Platform Primitives Must Be Recursively Usable

**Rule:** All platform primitives (CRM, Automation, Billing, Affiliate, Site Builder, Messaging, Reporting, PWA, Offline, Push Notifications) MUST be recursively usable across all hierarchy levels (Super Admin → Partners → Clients → End Users).

**Enforcement:** All operators (Manus, Emergent, Replit) MUST validate recursive usage during development. Any PR that restricts a platform primitive to a specific hierarchy level MUST be rejected.

### Governance Rule #2: No Hard-Coded Assumptions

**Rule:** The architecture MUST NOT hard-code assumptions about who uses what. All access MUST be permission-based, not role-based.

**Enforcement:** All operators MUST validate that no hard-coded assumptions exist during development. Any PR that introduces hard-coded assumptions MUST be rejected.

### Governance Rule #3: All Data Models Must Be Scoped

**Rule:** All data models MUST be scoped to the hierarchy level (Super Admin, Partner, Client, End User).

**Enforcement:** All operators MUST validate that data models are scoped during development. Any PR that introduces unscoped data models MUST be rejected.

### Governance Rule #4: All Surfaces Must Be White-Labelable

**Rule:** All surfaces (dashboards, apps, landing pages) MUST be white-labelable so that partners and clients can brand them as their own.

**Enforcement:** All operators MUST validate that surfaces are white-labelable during development. Any PR that introduces non-white-labelable surfaces MUST be rejected.

---

## Summary

The Recursive Application Model is a **foundational architectural law** of WebWaka. Any system WebWaka uses internally MUST be available for partners and clients to use for their own platforms. This applies to all platform primitives (PWA, offline, push notifications, CRM, automation, billing, affiliate, site builder, messaging, reporting). The architecture MUST NOT hard-code assumptions, all data models MUST be scoped, all access MUST be permission-based, and all surfaces MUST be white-labelable.

This is not negotiable.
