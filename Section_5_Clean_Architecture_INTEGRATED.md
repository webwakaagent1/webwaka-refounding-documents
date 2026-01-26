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
