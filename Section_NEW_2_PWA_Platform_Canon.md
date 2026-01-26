# SECTION: PWA PLATFORM CANON

**Status:** 🔒 CANONICALLY LOCKED — PWA-first is a foundational architectural law, not an optional feature.

---

## Overview

WebWaka is a **PWA-first platform**. Every dashboard, client app, and surface MUST be PWA-installable by default. No WebWaka surface is "web-only." Installability is a baseline requirement, not a progressive enhancement.

PWA is the **primary delivery model** for WebWaka. Native mobile apps (iOS, Android) are **optional accelerators**, not architectural dependencies. The platform architecture must not assume native apps exist.

---

## Foundational Principle

**"Every surface must be PWA-installable by default. Native apps are optional accelerators, not dependencies."**

This principle applies recursively across all hierarchy levels:

- **Super Admin Dashboard** → PWA-installable
- **Partner Dashboard** → PWA-installable
- **Client Dashboard** → PWA-installable
- **End User Apps** (POS, CRM, etc.) → PWA-installable

---

## Installability Requirements

All WebWaka surfaces MUST meet the following installability requirements to be considered PWA-compliant:

### 1. **Web App Manifest**

Every surface MUST have a valid `manifest.json` file that defines:

- **name** (e.g., "WebWaka Partner Dashboard")
- **short_name** (e.g., "Partner Dashboard")
- **description** (e.g., "Manage your WebWaka partner account")
- **start_url** (e.g., "/partner/dashboard")
- **display** (e.g., "standalone" for app-like experience)
- **theme_color** (e.g., "#1E40AF" for brand color)
- **background_color** (e.g., "#FFFFFF" for splash screen)
- **icons** (multiple sizes: 192x192, 512x512, etc.)

**Dynamic Manifest Generation:**

For white-labeled surfaces (e.g., Partner Dashboards, Client Dashboards), the manifest MUST be dynamically generated per partner/client to reflect their branding:

- **name** → Partner's brand name (e.g., "Acme Partner Dashboard")
- **short_name** → Partner's short name (e.g., "Acme Dashboard")
- **theme_color** → Partner's brand color (e.g., "#FF5733")
- **icons** → Partner's logo (uploaded by partner)

This ensures that when a partner's client installs the PWA, it appears as the partner's branded app, not WebWaka's.

---

### 2. **Service Worker**

Every surface MUST have a registered service worker that provides:

- **Offline support** (cache critical assets for offline access)
- **Background sync** (sync offline actions when connectivity is restored)
- **Push notifications** (receive push notifications even when the PWA is closed)

**Service Worker Expectations:**

- **Caching Strategy:** Cache-first for static assets (HTML, CSS, JS, images); network-first for API requests
- **Update Strategy:** Prompt user to update when a new service worker is available (e.g., "A new version is available. Tap to update.")
- **Lifecycle Management:** Handle service worker lifecycle events (install, activate, fetch) correctly

---

### 3. **HTTPS**

All WebWaka surfaces MUST be served over HTTPS. PWAs require HTTPS to function (except on localhost for development).

**Enforcement:**

- AWS Amplify automatically provides HTTPS via AWS CloudFront
- All custom domains MUST be configured with SSL certificates (AWS Certificate Manager)

---

### 4. **Responsive Design**

All WebWaka surfaces MUST be responsive and work seamlessly on mobile, tablet, and desktop devices.

**Enforcement:**

- All operators MUST test surfaces on multiple device sizes during development
- Any PR that introduces a non-responsive surface MUST be rejected

---

## Service Worker Expectations

The service worker is the **core of the PWA**. It enables offline support, background sync, and push notifications. All WebWaka surfaces MUST implement a service worker that meets the following expectations:

### 1. **Offline Support**

The service worker MUST cache critical assets (HTML, CSS, JS, images) so that the PWA can load offline.

**Caching Strategy:**

| Asset Type | Caching Strategy | Rationale |
|------------|------------------|-----------|
| **Static Assets** (HTML, CSS, JS, images) | Cache-first | Static assets rarely change; serve from cache for fast load times |
| **API Requests** | Network-first | API data changes frequently; fetch from network first, fall back to cache if offline |
| **User-Generated Content** (e.g., uploaded images) | Cache-first | User-generated content is static once uploaded |

**Cache Invalidation:**

When a new version of the PWA is deployed, the service worker MUST invalidate the old cache and prompt the user to update.

---

### 2. **Background Sync**

The service worker MUST implement the **Background Sync API** to sync offline actions when connectivity is restored.

**How It Works:**

1. User performs an offline action (e.g., POS transaction)
2. Action is queued in IndexedDB
3. Service worker registers a background sync event
4. When connectivity is restored, the service worker syncs the queued action in the background (even if the PWA is closed)
5. User is notified when the sync is complete (via push notification)

**Fallback:**

If the Background Sync API is not supported (e.g., on iOS Safari), the PWA MUST fall back to manual sync (e.g., "Tap to sync" button).

---

### 3. **Push Notifications**

The service worker MUST implement the **Push API** to receive push notifications even when the PWA is closed.

**How It Works:**

1. User grants push notification permission
2. Service worker subscribes to push notifications (via AWS SNS)
3. Server sends push notification to AWS SNS
4. AWS SNS delivers push notification to the service worker
5. Service worker displays the notification to the user

**Notification Payload:**

Push notifications MUST include:

- **title** (e.g., "New Order Received")
- **body** (e.g., "You have a new order from John Doe")
- **icon** (e.g., partner's logo)
- **badge** (e.g., unread count)
- **data** (e.g., order ID, deep link to order details)

**Deep Linking:**

When the user taps a push notification, the PWA MUST open to the relevant page (e.g., order details page).

---

## Cache Strategies (High-Level)

### Cache-First

**When to Use:** Static assets (HTML, CSS, JS, images) that rarely change.

**How It Works:**

1. Service worker checks if the asset is in the cache
2. If yes, serve from cache
3. If no, fetch from network and cache for future use

**Benefit:** Fast load times (no network request required).

---

### Network-First

**When to Use:** API requests that fetch dynamic data (e.g., user data, inventory data).

**How It Works:**

1. Service worker fetches from network first
2. If network request succeeds, return the response and update the cache
3. If network request fails (offline), fall back to cache

**Benefit:** Always fetch the latest data when online; fall back to cached data when offline.

---

### Stale-While-Revalidate

**When to Use:** Assets that change occasionally but can tolerate stale data (e.g., product images, user avatars).

**How It Works:**

1. Service worker serves from cache immediately (stale data)
2. In the background, service worker fetches from network and updates the cache
3. Next time the asset is requested, the updated version is served

**Benefit:** Fast load times (serve from cache) + always up-to-date (revalidate in background).

---

## Update & Versioning Behavior

### How Updates Work

When a new version of the PWA is deployed:

1. **New service worker is installed** (but not activated yet)
2. **Old service worker remains active** (until all tabs are closed)
3. **User is prompted to update** (e.g., "A new version is available. Tap to update.")
4. **User taps "Update"** → All tabs are closed and reopened with the new service worker

### Versioning Strategy

Each PWA version MUST have a unique version number (e.g., `v1.0.0`, `v1.1.0`, `v2.0.0`).

**Where to Store Version:**

- In the `manifest.json` file (e.g., `"version": "1.0.0"`)
- In the service worker file (e.g., `const VERSION = "1.0.0";`)

**Cache Invalidation:**

When a new version is deployed, the service worker MUST invalidate the old cache by deleting all cached assets with the old version number.

---

## Recursive PWA Patterns

PWA patterns are **recursively usable** across all hierarchy levels:

- **Super Admin Dashboard** → PWA-installable, offline-capable, push-enabled
- **Partner Dashboard** → PWA-installable, offline-capable, push-enabled, white-labeled
- **Client Dashboard** → PWA-installable, offline-capable, push-enabled, white-labeled
- **End User Apps** → PWA-installable, offline-capable, push-enabled, white-labeled

WebWaka does not just use PWA patterns—it **provides PWA as a platform primitive** that partners and clients can use to build their own installable, offline-capable, push-enabled apps.

**Example:**

A partner (e.g., Acme Corp) can create a white-labeled POS app for their clients. When a client installs the POS app, it appears as "Acme POS" (not "WebWaka POS") with Acme's branding (logo, colors, name). The POS app is offline-capable and push-enabled, just like WebWaka's own dashboards.

---

## Governance & Enforcement

### Governance Rule #1: All Surfaces Must Be PWA-Installable

**Rule:** All WebWaka surfaces MUST be PWA-installable by default. Any surface that is not PWA-installable is considered incomplete and non-compliant.

**Enforcement:** All operators (Manus, Emergent, Replit) MUST validate PWA installability during development. Any PR that introduces a non-installable surface MUST be rejected.

### Governance Rule #2: Service Workers Must Be Implemented

**Rule:** All WebWaka surfaces MUST have a registered service worker that provides offline support, background sync, and push notifications.

**Enforcement:** All operators MUST validate service worker implementation during development. Any PR that introduces a surface without a service worker MUST be rejected.

### Governance Rule #3: Dynamic Manifest Generation for White-Label

**Rule:** All white-labeled surfaces (Partner Dashboards, Client Dashboards) MUST dynamically generate the manifest to reflect the partner/client's branding.

**Enforcement:** All operators MUST validate dynamic manifest generation during development. Any PR that introduces a white-labeled surface without dynamic manifest generation MUST be rejected.

---

## Summary

PWA-first is a **foundational architectural law** of WebWaka, not an optional feature. Every surface must be PWA-installable by default, with a registered service worker that provides offline support, background sync, and push notifications. PWA patterns are recursively usable across all hierarchy levels, and WebWaka provides PWA as a platform primitive that partners and clients can use to build their own installable, offline-capable, push-enabled apps.

This is not negotiable.
