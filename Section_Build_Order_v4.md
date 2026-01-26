# SECTION: STRICT, SEQUENTIAL BUILD ORDER

**Status:** 🔒 CANONICALLY LOCKED — This build order is mandatory and may not be reordered without explicit Founder approval.

---

## Overview

WebWaka's architecture is designed for **maximum scale from day one**, but implementation is **phased** to manage complexity. This section defines the strict, sequential build order that all operators must follow.

**Key Principle:** Architecture supports the full scope from day one; only implementation is phased.

---

## Implementation vs. Architecture Phasing

### Architecture

The architecture is **NOT phased**. All architectural decisions (data models, APIs, infrastructure) are designed to support the full scope from day one:

- Multi-level affiliate system (up to 10 levels)
- Hierarchical pricing (Global → Partner → Client)
- Recursive system usage (Super Admin → Partners → Clients → End Users)
- PWA-first (all surfaces installable)
- Offline-first (all core actions offline-capable)
- Push notifications (platform-wide, recursively usable)

### Implementation

The implementation **IS phased**. Features are built incrementally across 3 phases:

- **Phase 1 (3-6 months):** Core Infrastructure + Commerce Suites
- **Phase 2 (6-9 months):** Composable Primitives + Affiliate System
- **Phase 3 (9-12 months):** Multi-Industry Expansion

---

## Phase 1: Core Infrastructure + Commerce Suites (3-6 months)

**Goal:** Build the foundational infrastructure and prove the platform with commerce-focused suites.

### Phase 1.1: Core Infrastructure (Weeks 1-4)

**Deliverables:**

1. **AWS Account Setup**
   - AWS Organization created
   - Dev, Staging, Prod environments configured
   - IAM roles and policies configured
   - Billing alerts configured

2. **Authentication & Authorization (AWS Cognito)**
   - User pools created (Super Admin, Partners, Clients, End Users)
   - OAuth 2.0 flows configured
   - Multi-factor authentication (MFA) enabled
   - Row-level security (RLS) policies defined

3. **Database (AWS Aurora PostgreSQL)**
   - Database cluster created (multi-AZ for high availability)
   - Core tables created:
     - `users` (Super Admin, Partners, Clients, End Users)
     - `tenants` (Partners, Clients)
     - `permissions` (permission-based access control)
     - `affiliates` (Closure Table for multi-level affiliates)
     - `pricing` (hierarchical pricing overrides)
     - `notifications` (push notification subscriptions)
   - Row-level security (RLS) policies implemented
   - Database migrations setup (e.g., Prisma Migrate)

4. **Backend Hosting (AWS Fargate)**
   - ECS cluster created
   - Fargate tasks configured (auto-scaling enabled)
   - Load balancer configured (ALB)
   - Health checks configured

5. **Frontend Hosting (AWS Amplify)**
   - Amplify app created
   - CI/CD pipeline configured (auto-deploy on push to `main`)
   - Custom domain configured (SSL certificate via AWS Certificate Manager)
   - **PWA support enabled** (manifest.json, service worker, HTTPS)

6. **PWA Infrastructure (NEW)**
   - **Service Worker Setup:**
     - Service worker template created (offline support, background sync, push notifications)
     - Cache strategies defined (cache-first for static assets, network-first for API requests)
     - Background Sync API integrated (automatic syncing when connectivity is restored)
   - **PWA Manifest Generation:**
     - Dynamic manifest generation per tenant (name, icons, theme color)
     - Manifest API endpoint created (`/api/manifest.json`)
   - **Push Notification Setup:**
     - AWS SNS integration (push notification delivery)
     - Push subscription API endpoint created (`/api/push/subscribe`)
     - Push notification service created (event-driven, listens for events on EventBridge)

7. **Offline Sync Infrastructure (NEW)**
   - **IndexedDB Setup:**
     - IndexedDB schema defined (queued actions, sync status, retry count)
     - IndexedDB wrapper library created (for queuing offline actions)
   - **Background Sync Service:**
     - Background sync service created (syncs queued actions when connectivity is restored)
     - Conflict resolution strategies implemented (per action type)
     - Retry mechanism implemented (exponential backoff)

8. **Storage (AWS S3 + CloudFront)**
   - S3 bucket created (for user-uploaded files)
   - CloudFront distribution created (CDN for fast file delivery)
   - Pre-signed URLs configured (secure file uploads)

9. **Email (AWS SES)**
   - SES configured (send transactional emails)
   - Email templates created (welcome email, password reset, etc.)

10. **SMS & Push Notifications (AWS SNS)**
    - SNS configured (send SMS and push notifications)
    - Push notification topics created (per tenant)

11. **Background Jobs (AWS Lambda)**
    - Lambda functions created (for background tasks)
    - SQS queues created (for job queueing)

12. **Events (AWS EventBridge)**
    - EventBridge configured (event-driven architecture)
    - Event rules created (e.g., "ORDER_SHIPPED" → send push notification)

13. **Monitoring & Logging (AWS CloudWatch)**
    - CloudWatch dashboards created (monitor infrastructure health)
    - Log groups created (centralized logging)
    - Alarms configured (alert on errors, high latency, etc.)

**Success Criteria:**

- ✅ All AWS services configured and operational
- ✅ Authentication & authorization working (Cognito)
- ✅ Database operational with RLS policies (Aurora)
- ✅ Backend deployed and auto-scaling (Fargate)
- ✅ Frontend deployed with PWA support (Amplify)
- ✅ Service worker registered and functional (offline, background sync, push notifications)
- ✅ Push notifications working (AWS SNS)
- ✅ Offline sync infrastructure operational (IndexedDB, background sync)

---

### Phase 1.2: Core API Domains (Weeks 5-8)

**Deliverables:**

1. **Tenant Management API**
   - Create tenant (Partner, Client)
   - Update tenant (name, branding, settings)
   - Delete tenant (soft delete)
   - List tenants (scoped to hierarchy level)

2. **User Management API**
   - Create user (Super Admin, Partner, Client, End User)
   - Update user (name, email, password)
   - Delete user (soft delete)
   - List users (scoped to tenant)

3. **Permission Management API**
   - Grant permission (e.g., "SEND_PUSH_NOTIFICATIONS")
   - Revoke permission
   - List permissions (scoped to user)

4. **Affiliate Management API**
   - Create affiliate relationship (Closure Table)
   - Calculate commissions (fixed percentages)
   - List affiliates (scoped to tenant)

5. **Pricing Management API**
   - Set pricing (hierarchical overrides: Global → Partner → Client)
   - Get pricing (resolve hierarchical overrides)
   - List pricing (scoped to tenant)

6. **Notification Management API (NEW)**
   - Subscribe to push notifications (`/api/push/subscribe`)
   - Unsubscribe from push notifications (`/api/push/unsubscribe`)
   - Send push notification (`/api/push/send`)
   - List notification preferences (scoped to user)
   - Update notification preferences (enable/disable types, senders)

7. **Offline Sync API (NEW)**
   - Queue offline action (`/api/sync/queue`)
   - Sync queued actions (`/api/sync/sync`)
   - Get sync status (`/api/sync/status`)
   - List queued actions (scoped to user)

8. **File Upload API**
   - Generate pre-signed URL (`/api/files/upload`)
   - Download file (`/api/files/download`)
   - Delete file (`/api/files/delete`)

**Success Criteria:**

- ✅ All core API domains operational
- ✅ APIs respect RLS policies (data isolation)
- ✅ APIs support hierarchical overrides (pricing, affiliate)
- ✅ APIs support offline sync (queue, sync, status)
- ✅ APIs support push notifications (subscribe, send, preferences)

---

### Phase 1.3: Super Admin Dashboard (Weeks 9-10)

**Deliverables:**

1. **Super Admin Dashboard (PWA)**
   - Partner management (create, update, delete, list)
   - User management (create, update, delete, list)
   - Global pricing configuration
   - Global affiliate configuration
   - Platform analytics (partner count, client count, revenue)
   - **PWA-installable** (manifest, service worker, HTTPS)
   - **Offline-capable** (core actions work offline and sync later)
   - **Push-enabled** (receive push notifications)

**Success Criteria:**

- ✅ Super Admin Dashboard operational
- ✅ Super Admin can manage partners and users
- ✅ Super Admin can configure global pricing and affiliate logic
- ✅ Super Admin Dashboard is PWA-installable
- ✅ Super Admin Dashboard supports offline core actions
- ✅ Super Admin Dashboard supports push notifications

---

### Phase 1.4: Partner Dashboard (Weeks 11-12)

**Deliverables:**

1. **Partner Dashboard (PWA, White-Labeled)**
   - Client management (create, update, delete, list)
   - User management (create, update, delete, list)
   - Partner pricing configuration (override global pricing)
   - Partner affiliate configuration (override global affiliate logic)
   - Partner analytics (client count, revenue)
   - **PWA-installable** (dynamic manifest with partner branding)
   - **Offline-capable** (core actions work offline and sync later)
   - **Push-enabled** (receive push notifications from Super Admin, send push notifications to clients)

**Success Criteria:**

- ✅ Partner Dashboard operational
- ✅ Partner can manage clients and users
- ✅ Partner can configure their own pricing and affiliate logic
- ✅ Partner Dashboard is PWA-installable with partner branding
- ✅ Partner Dashboard supports offline core actions
- ✅ Partner Dashboard supports push notifications (receive and send)

---

### Phase 1.5: POS Suite (Weeks 13-16)

**Deliverables:**

1. **POS Suite (PWA, White-Labeled, Offline-First)**
   - Product management (create, update, delete, list)
   - Inventory management (stock adjustments, low stock alerts)
   - Sales transactions (create sale, refund, void)
   - Payment processing (cash, card, mobile money)
   - Receipt generation (print, email, SMS)
   - **PWA-installable** (dynamic manifest with client branding)
   - **Offline-first** (all POS transactions work offline and sync later)
   - **Push-enabled** (receive push notifications from partner, send push notifications to customers)

**Success Criteria:**

- ✅ POS Suite operational
- ✅ POS transactions work offline and sync later
- ✅ POS Suite is PWA-installable with client branding
- ✅ POS Suite supports push notifications (receive and send)

---

## Phase 2: Composable Primitives + Affiliate System (6-9 months)

**Goal:** Build the composable primitives that enable partners to build their own platforms.

### Phase 2.1: CRM Primitive (Weeks 17-20)

**Deliverables:**

1. **CRM Primitive (PWA, Recursively Usable, Offline-First)**
   - Contact management (create, update, delete, list)
   - Lead management (create, update, delete, list, convert to contact)
   - Deal management (create, update, delete, list, move through pipeline)
   - **PWA-installable** (dynamic manifest with tenant branding)
   - **Offline-first** (all CRM actions work offline and sync later)
   - **Push-enabled** (receive push notifications for new leads, deals)
   - **Recursively usable** (Super Admin → Partners → Clients)

**Success Criteria:**

- ✅ CRM Primitive operational
- ✅ CRM actions work offline and sync later
- ✅ CRM Primitive is PWA-installable
- ✅ CRM Primitive is recursively usable across all hierarchy levels

---

### Phase 2.2: Automation Primitive (Weeks 21-24)

**Deliverables:**

1. **Automation Primitive (PWA, Recursively Usable)**
   - Workflow builder (create, update, delete, list)
   - Trigger configuration (e.g., "New Lead Created")
   - Action configuration (e.g., "Send Email", "Send Push Notification")
   - **PWA-installable** (dynamic manifest with tenant branding)
   - **Push-enabled** (send push notifications as workflow actions)
   - **Recursively usable** (Super Admin → Partners → Clients)

**Success Criteria:**

- ✅ Automation Primitive operational
- ✅ Workflows can trigger push notifications
- ✅ Automation Primitive is PWA-installable
- ✅ Automation Primitive is recursively usable across all hierarchy levels

---

### Phase 2.3: Affiliate System (Weeks 25-28)

**Deliverables:**

1. **Affiliate System (PWA, Recursively Usable, Offline-First)**
   - Affiliate relationship management (Closure Table, up to 10 levels)
   - Commission calculation (fixed percentages, hierarchical overrides)
   - Payout management (platform-managed payouts)
   - Affiliate activity logging (referrals, sign-ups, conversions)
   - **PWA-installable** (dynamic manifest with tenant branding)
   - **Offline-first** (affiliate activity logging works offline and syncs later)
   - **Push-enabled** (receive push notifications for new referrals, commissions)
   - **Recursively usable** (Super Admin → Partners → Clients)

**Success Criteria:**

- ✅ Affiliate System operational
- ✅ Affiliate activity logging works offline and syncs later
- ✅ Affiliate System is PWA-installable
- ✅ Affiliate System is recursively usable across all hierarchy levels

---

## Phase 3: Multi-Industry Expansion (9-12 months)

**Goal:** Expand to additional industries beyond commerce.

### Phase 3.1: Education Suite (Weeks 29-32)

**Deliverables:**

1. **Education Suite (PWA, White-Labeled, Offline-First)**
   - Course management (create, update, delete, list)
   - Student management (enroll, unenroll, track progress)
   - Assignment management (create, submit, grade)
   - **PWA-installable** (dynamic manifest with tenant branding)
   - **Offline-first** (course content accessible offline)
   - **Push-enabled** (receive push notifications for new assignments, grades)

---

### Phase 3.2: Health Suite (Weeks 33-36)

**Deliverables:**

1. **Health Suite (PWA, White-Labeled, Offline-First)**
   - Patient management (create, update, delete, list)
   - Appointment scheduling (create, update, cancel)
   - Medical records management (create, update, view)
   - **PWA-installable** (dynamic manifest with tenant branding)
   - **Offline-first** (medical records accessible offline)
   - **Push-enabled** (receive push notifications for appointment reminders)

---

### Phase 3.3: Civic Suite (Weeks 37-40)

**Deliverables:**

1. **Civic Suite (PWA, White-Labeled, Offline-First)**
   - Citizen engagement (surveys, polls, feedback)
   - Service request management (create, track, resolve)
   - Event management (create, RSVP, attend)
   - **PWA-installable** (dynamic manifest with tenant branding)
   - **Offline-first** (service requests work offline and sync later)
   - **Push-enabled** (receive push notifications for service request updates)

---

## Dependency Graph

### Structural Dependencies (MUST be built in order)

```
Phase 1.1 (Core Infrastructure)
  ↓
Phase 1.2 (Core API Domains)
  ↓
Phase 1.3 (Super Admin Dashboard)
  ↓
Phase 1.4 (Partner Dashboard)
  ↓
Phase 1.5 (POS Suite)
  ↓
Phase 2.1 (CRM Primitive)
  ↓
Phase 2.2 (Automation Primitive)
  ↓
Phase 2.3 (Affiliate System)
  ↓
Phase 3.1 (Education Suite)
Phase 3.2 (Health Suite)
Phase 3.3 (Civic Suite)
```

### Optional Dependencies (can be built in parallel)

- Phase 3.1, 3.2, 3.3 can be built in parallel (no dependencies between them)

---

## Governance & Enforcement

### Governance Rule #1: Build Order Must Be Followed

**Rule:** The build order MUST be followed strictly. No phase can be started until the previous phase is complete.

**Enforcement:** All operators (Manus, Emergent, Replit) MUST validate that the previous phase is complete before starting the next phase. Any PR that violates the build order MUST be rejected.

### Governance Rule #2: PWA and Offline Requirements Must Be Met

**Rule:** All surfaces MUST be PWA-installable and support offline-first for core actions.

**Enforcement:** All operators MUST validate PWA and offline support during development. Any PR that introduces a non-PWA surface or a surface without offline support for core actions MUST be rejected.

### Governance Rule #3: Push Notification Support Must Be Included

**Rule:** All surfaces MUST support push notifications (receive and/or send).

**Enforcement:** All operators MUST validate push notification support during development. Any PR that introduces a surface without push notification support MUST be rejected.

---

## Summary

The build order is **strict and sequential**. Architecture supports the full scope from day one; only implementation is phased. All surfaces MUST be PWA-installable, support offline-first for core actions, and support push notifications. This is not negotiable.
