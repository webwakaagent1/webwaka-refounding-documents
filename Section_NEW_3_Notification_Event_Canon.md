# SECTION: NOTIFICATION & EVENT DELIVERY CANON

**Status:** 🔒 CANONICALLY LOCKED — Push notifications are a first-class platform primitive, not a UI feature.

---

## Overview

Push notifications are a **first-class system primitive** in WebWaka, not a "nice-to-have" or UI feature. They are a core part of the event-driven architecture and are recursively usable across all hierarchy levels.

WebWaka does not just use push notifications—it **provides push notifications as a platform primitive** that partners and clients can use to send notifications to their own users.

---

## Foundational Principle

**"Push notifications are a first-class system primitive, recursively usable across all hierarchy levels."**

This principle applies recursively:

- **Super Admin** → Can send push notifications to partners
- **Partners** → Can send push notifications to clients
- **Clients** → Can send push notifications to end users
- **End Users** → Can receive push notifications from clients

---

## Push Notifications as a Platform Primitive

### What This Means

Push notifications are not just a feature of WebWaka's dashboards. They are a **platform-wide capability** that is available to all users at all hierarchy levels.

**Examples:**

1. **Super Admin sends push notification to all partners:**
   - "New feature released: Multi-currency support is now available."

2. **Partner sends push notification to all clients:**
   - "Your monthly invoice is ready. Tap to view."

3. **Client sends push notification to all end users:**
   - "Your order has been shipped. Track your package here."

4. **End User receives push notification from client:**
   - "Your appointment is tomorrow at 10 AM. Tap to reschedule."

---

## Event-Driven Architecture Implications

Push notifications are triggered by **events** in the system. WebWaka uses an **event-driven architecture** where events are published to **AWS EventBridge** and consumed by various services, including the push notification service.

### How It Works

1. **Event is published** (e.g., "ORDER_SHIPPED")
2. **EventBridge routes the event** to the push notification service
3. **Push notification service determines recipients** (e.g., all users who ordered this product)
4. **Push notification service sends notifications** via AWS SNS
5. **AWS SNS delivers notifications** to service workers
6. **Service workers display notifications** to users

---

## Notification Types

WebWaka supports the following notification types:

### 1. **Transactional Notifications**

**Definition:** Notifications triggered by user actions or system events.

**Examples:**

- "Your order has been confirmed."
- "Your payment was successful."
- "Your affiliate commission has been credited."

**Delivery:** Immediate (sent as soon as the event occurs).

---

### 2. **Marketing Notifications**

**Definition:** Notifications sent by partners or clients to promote products, services, or campaigns.

**Examples:**

- "New product launch: 20% off for the next 24 hours."
- "Flash sale: Limited time offer on all items."

**Delivery:** Scheduled (sent at a specific time or triggered by a campaign).

**Opt-In Required:** Users must explicitly opt in to receive marketing notifications.

---

### 3. **System Notifications**

**Definition:** Notifications sent by WebWaka to inform users of platform updates, maintenance, or issues.

**Examples:**

- "Scheduled maintenance: WebWaka will be offline from 2 AM to 4 AM."
- "New feature released: Multi-currency support is now available."

**Delivery:** Immediate or scheduled.

---

### 4. **Reminder Notifications**

**Definition:** Notifications sent to remind users of upcoming events, tasks, or deadlines.

**Examples:**

- "Your appointment is tomorrow at 10 AM."
- "Your subscription expires in 3 days. Tap to renew."

**Delivery:** Scheduled (sent at a specific time before the event).

---

## Recursive Usage by Partners and Clients

### Partner Use Case

A partner (e.g., Acme Corp) can use WebWaka's push notification primitive to send notifications to their clients.

**Example:**

Acme Corp wants to notify all their clients about a new feature they've added to their platform.

**How It Works:**

1. Acme Corp creates a notification in their Partner Dashboard
2. Acme Corp selects recipients (e.g., all clients)
3. Acme Corp writes the notification message (e.g., "New feature: Inventory management is now available.")
4. Acme Corp schedules the notification (e.g., send immediately)
5. WebWaka's push notification service sends the notification to all Acme Corp's clients via AWS SNS
6. Clients receive the notification on their devices

**Branding:**

The notification appears as coming from Acme Corp (not WebWaka), with Acme Corp's logo and branding.

---

### Client Use Case

A client (e.g., John's Store) can use WebWaka's push notification primitive to send notifications to their end users.

**Example:**

John's Store wants to notify all customers who ordered a product that their order has been shipped.

**How It Works:**

1. John's Store's system publishes an "ORDER_SHIPPED" event
2. WebWaka's push notification service listens for this event
3. Push notification service determines recipients (e.g., all customers who ordered this product)
4. Push notification service sends the notification to all recipients via AWS SNS
5. Customers receive the notification on their devices

**Branding:**

The notification appears as coming from John's Store (not WebWaka or Acme Corp), with John's Store's logo and branding.

---

## Integration with AWS SNS

WebWaka uses **AWS SNS (Simple Notification Service)** for push notification delivery.

### Why AWS SNS?

1. **Scalable:** Can send millions of notifications per day
2. **Reliable:** Industry-leading SLAs (99.9% uptime)
3. **Multi-Platform:** Supports web push (via service workers), iOS push (APNS), and Android push (FCM)
4. **Cost-Effective:** Pay only for what you use (no fixed costs)

### How It Works

1. **User subscribes to push notifications** (via service worker)
2. **Service worker registers with AWS SNS** (receives a subscription ARN)
3. **Subscription ARN is stored in the database** (associated with the user)
4. **When a notification is sent:**
   - Push notification service retrieves the subscription ARN
   - Push notification service publishes the notification to AWS SNS
   - AWS SNS delivers the notification to the service worker
   - Service worker displays the notification to the user

---

## Notification Payload

All push notifications MUST include the following fields:

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| **title** | String | Yes | Notification title (e.g., "New Order Received") |
| **body** | String | Yes | Notification body (e.g., "You have a new order from John Doe") |
| **icon** | String (URL) | Yes | Icon to display (e.g., partner's logo) |
| **badge** | String (URL) | No | Badge to display (e.g., unread count) |
| **image** | String (URL) | No | Image to display (e.g., product image) |
| **data** | Object | No | Custom data (e.g., order ID, deep link) |
| **actions** | Array | No | Action buttons (e.g., "View Order", "Dismiss") |

**Example Payload:**

```json
{
  "title": "New Order Received",
  "body": "You have a new order from John Doe",
  "icon": "https://cdn.webwaka.com/partners/acme/logo.png",
  "badge": "https://cdn.webwaka.com/partners/acme/badge.png",
  "image": "https://cdn.webwaka.com/products/12345/image.jpg",
  "data": {
    "orderId": "12345",
    "deepLink": "/orders/12345"
  },
  "actions": [
    { "action": "view", "title": "View Order" },
    { "action": "dismiss", "title": "Dismiss" }
  ]
}
```

---

## Deep Linking

When a user taps a push notification, the PWA MUST open to the relevant page (deep linking).

**How It Works:**

1. Notification payload includes a `deepLink` field (e.g., `/orders/12345`)
2. Service worker listens for notification click events
3. When the user taps the notification, the service worker opens the PWA to the deep link

**Example:**

User taps "New Order Received" notification → PWA opens to `/orders/12345` (order details page).

---

## Notification Preferences

Users MUST be able to control their notification preferences.

**Preferences:**

- **Enable/Disable All Notifications:** User can turn off all notifications
- **Enable/Disable Notification Types:** User can turn off specific types (e.g., marketing notifications)
- **Enable/Disable Notifications from Specific Senders:** User can turn off notifications from specific partners or clients

**Storage:**

Notification preferences are stored in the database and respected by the push notification service.

---

## Governance & Enforcement

### Governance Rule #1: Push Notifications Must Be Available Platform-Wide

**Rule:** Push notifications MUST be available as a platform primitive to all users at all hierarchy levels (Super Admin, Partners, Clients, End Users).

**Enforcement:** All operators (Manus, Emergent, Replit) MUST validate that push notifications are available platform-wide during development. Any PR that restricts push notifications to specific users or levels MUST be rejected.

### Governance Rule #2: Notifications Must Respect User Preferences

**Rule:** All push notifications MUST respect user notification preferences (enable/disable all, enable/disable types, enable/disable senders).

**Enforcement:** All operators MUST validate that notification preferences are respected during development. Any PR that sends notifications without respecting user preferences MUST be rejected.

### Governance Rule #3: Notifications Must Include Required Fields

**Rule:** All push notifications MUST include the required fields (title, body, icon).

**Enforcement:** All operators MUST validate that notifications include required fields during development. Any PR that sends notifications without required fields MUST be rejected.

---

## Summary

Push notifications are a **first-class system primitive** in WebWaka, not a UI feature. They are part of the event-driven architecture and are recursively usable across all hierarchy levels. WebWaka provides push notifications as a platform primitive that partners and clients can use to send notifications to their own users. All notifications are delivered via AWS SNS and respect user notification preferences.

This is not negotiable.
