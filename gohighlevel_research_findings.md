# GoHighLevel Research Findings

**Date:** 2026-01-26  
**Purpose:** Deep structural study of GoHighLevel to understand platform architecture and inform WebWaka strategic scope

---

## Key Findings from Initial Research

### 1. Platform Positioning

GoHighLevel positions itself as an **"all-in-one business operating system"** for marketers and agencies. Key characteristics:

- **Target Users:** Marketing agencies, freelancers, coaches, local service providers
- **Core Value Proposition:** Consolidate all marketing tools into one platform (eliminate "duct-taping" multiple tools)
- **Business Model:** SaaS platform with white-label reselling capabilities

### 2. Account Hierarchy (Multi-Tier Structure)

From research, GoHighLevel has a clear multi-tier account structure:

**Agency Level (Top Tier):**
- Agency account is the primary account
- Can create and manage multiple sub-accounts
- Has white-label capabilities (custom branding, custom domain)
- Can resell platform access to clients

**Sub-Account Level (Client Level):**
- Each client gets their own sub-account
- Sub-accounts are isolated (separate data, separate users)
- Agency can manage sub-accounts centrally
- Each sub-account has its own automations, pipelines, contacts

**User Level:**
- Multiple users can exist within agency account
- Multiple users can exist within each sub-account
- Granular permissions (agency admins can be assigned to specific sub-accounts)

**Pricing Tiers:**
- **Starter Plan ($97/month):** Up to 3 sub-accounts
- **Unlimited Plan ($297/month):** Unlimited sub-accounts, API access, branded desktop app

### 3. Core Capabilities (Capture → Nurture → Close Flow)

GoHighLevel organizes capabilities around a **lead lifecycle flow**:

#### Capture (Lead Generation)
- **Landing Pages:** Full-featured page builder
- **Websites:** Create full websites with custom menus
- **Forms & Surveys:** Drag-and-drop form builder, embeddable
- **Calendars:** Online appointment scheduling
- **Inbound Phone System:** (mentioned but not detailed)

#### Nurture (Lead Engagement)
- **Multi-Channel Campaigns:** Email, SMS/MMS, Voicemail Drops, Phone Connect, Facebook Messenger
- **Automation:** Event-driven workflows, automated follow-ups
- **Two-Way Communication:** Mobile app for communication on all devices
- **AI-Powered Conversations:** Automated nurture conversations with AI
- **Fully Automated Booking:** Auto-book leads to calendar without human interaction

#### Close (Conversion & Revenue)
- **CRM & Pipeline Management:** Track leads through sales funnel stages
- **Payment Collection:** Stripe integration for payments on websites, funnels, appointments
- **Analytics & Reports:** Dashboard showing lead status and revenue per phase
- **Membership Platform:** Create courses, offer free/paid courses, unlimited video hosting

### 4. White-Label & Reselling Model

GoHighLevel's white-label capabilities are central to its business model:

**White-Label Features:**
- **Desktop Application:** Custom branding, custom domain
- **Mobile App:** Create custom app in App Stores (additional fees apply)
- **Reselling:** Agencies can sell platform access to clients at their own price
- **Unlimited Accounts:** Sell to unlimited clients for one flat fee ($297/month)
- **Additional Revenue:** Charge clients platform access fee on top of services

**Agency Growth Model:**
- Agencies use GoHighLevel to deliver services to clients
- Agencies can white-label and resell as their own SaaS
- Recurring revenue model (monthly fees from clients)
- Platform access fee becomes additional revenue stream

### 5. Community & Support

GoHighLevel emphasizes community-driven development:

- **Facebook Community:** Network of marketers sharing tips and ideas
- **Ideas Board:** Community-driven feature requests and voting
- **24/7 Support:** Live chat, email, phone support
- **Training & Resources:** Courses, documentation, success stories

### 6. Integrations

GoHighLevel integrates with:
- Stripe (payments)
- Facebook Messenger (communication)
- Various marketing tools (one-click import from previous tools)
- API access (Unlimited plan only)

---

## Structural Insights

### Why GoHighLevel is Structured This Way

1. **Agency-First Design:** Platform is built for agencies who serve multiple clients, not for individual businesses
2. **White-Label Revenue Model:** Agencies become resellers, creating recurring revenue for both GoHighLevel and agencies
3. **Consolidation Value:** Eliminates need for multiple tools (funnel builder, CRM, email marketing, SMS, scheduling, etc.)
4. **Automation-First:** Reduces manual work for agencies, allowing them to scale without hiring more staff
5. **Sub-Account Isolation:** Each client is fully isolated, allowing agencies to manage hundreds of clients from one account

### How Features Interlock System-Wide

1. **Lead Capture → CRM:** Leads captured through forms/calendars automatically enter CRM
2. **CRM → Automation:** Leads in CRM trigger automated workflows (email, SMS, calls)
3. **Automation → Booking:** Automated conversations can book appointments without human interaction
4. **Booking → Payment:** Appointments can collect payments via Stripe integration
5. **Payment → Analytics:** Revenue tracked per pipeline stage in dashboard
6. **Agency → Sub-Accounts:** Agency can clone funnels/campaigns and deploy to sub-accounts instantly

---

## Gaps Identified (Further Research Needed)

1. **Multi-Level Affiliate Structure:** No clear information on multi-level affiliate program (Partner → Sub-Partner → Agent → Merchant chains)
2. **Commission Splits:** No details on how commissions are split across affiliate levels
3. **Fraud Prevention:** No information on fraud prevention mechanisms for affiliate system
4. **Industry-Specific Rules:** No evidence of industry-specific commission rules or configurations
5. **Sub-Agency Structure:** Feature request exists for "Sub-Agency-Sub-Accounts" (not yet implemented)
6. **Workflow/Automation Details:** Need deeper dive into workflow builder and automation triggers
7. **CRM Pipeline Details:** Need more information on pipeline stages, opportunity management, lead scoring

---

## Next Steps

1. Research GoHighLevel sub-account management in detail
2. Research GoHighLevel automation/workflow builder
3. Research GoHighLevel CRM and pipeline management
4. Research GoHighLevel affiliate program structure
5. Research GoHighLevel billing and monetization model
6. Create GoHighLevel Capability Decomposition Table

---

**End of Initial Research Findings**


---

## GoHighLevel Affiliate Program Structure (Detailed)

### Tier Structure

GoHighLevel has a **two-tier affiliate program**:

**Tier 1 Affiliates (Direct Referrals):**
- **Commission Rate:** 40% monthly recurring commission
- **Calculation:** 40% of the monthly subscription fee paid by the referred agency
- **Example:** If you refer an agency on the $297/month Unlimited plan, you earn $118.80/month for as long as they remain a customer
- **Duration:** Recurring (every month the customer remains active)

**Tier 2 Affiliates (Sub-Affiliates):**
- **Commission Rate:** 5% monthly recurring commission
- **Calculation:** 5% of the monthly subscription fee paid by agencies referred by your Tier 1 affiliates
- **Example:** If your Tier 1 affiliate refers an agency on the $297/month plan, you earn $14.85/month
- **Duration:** Recurring (every month the sub-affiliate's customer remains active)

### Key Characteristics

1. **Recurring Commissions:** Unlike one-time referral bonuses, GoHighLevel pays commissions every month for the lifetime of the customer
2. **Multi-Level (Two Tiers):** Affiliates earn from their direct referrals (Tier 1) AND from referrals made by their affiliates (Tier 2)
3. **100% Margin:** Affiliates don't need to provide services or support—just refer and earn
4. **Open to Non-Customers:** You don't need to be a GoHighLevel customer to become an affiliate
5. **Affiliate Portal:** Real-time tracking of referrals, commissions, and payouts

### Revenue Model

**For Affiliates:**
- Build recurring income by referring agencies
- Leverage network effects (your affiliates' referrals generate Tier 2 commissions)
- No cap on number of referrals
- Monthly payouts to affiliate account

**For GoHighLevel:**
- Affiliates become evangelists and sales force
- Low customer acquisition cost (pay only for successful conversions)
- Sticky customers (agencies build their business on the platform)
- Network effects (affiliates recruit more affiliates)

### Affiliate Support

- Ready-to-use banners, copy, and creative assets
- Dedicated affiliate support team
- Bonuses, contests, and early access to promotions
- Community of affiliates sharing strategies

---

## Sub-Account Structure (Detailed)

From the sub-account guide research:

### Sub-Account Definition

A **sub-account** (also called "client account" or "location") is an **isolated workspace** created under an agency's main HighLevel account.

### Sub-Account Characteristics

**Isolation:**
- Each sub-account has its own CRM database
- Each sub-account has its own pipelines, opportunities, funnels, websites, calendars
- Each sub-account has its own email/SMS automations
- Each sub-account has its own integrations and billing settings
- **Nothing is shared unless explicitly copied**

**User Management:**
- A single sub-account can have multiple users
- A single agency can manage hundreds of sub-accounts
- Users can be assigned to specific sub-accounts (not all sub-accounts)
- Granular permissions per user per sub-account

**Scaling:**
- Agencies can create sub-accounts in minutes using **snapshots** (pre-configured templates)
- Snapshots include pipelines, workflows, funnels, forms, calendars, email/SMS sequences
- Bulk actions allow agencies to manage multiple sub-accounts at once (enable SaaS mode, pause accounts, apply settings)

### Sub-Account Plans

| Plan | Sub-Accounts | Notes |
|------|--------------|-------|
| Starter ($97/month) | Up to 3 | Suitable for small agencies |
| Agency Unlimited ($297/month) | Unlimited | Core agency use |
| Agency Pro | Unlimited | Includes SaaS mode and automation |

### Sub-Account Transfers

- Sub-accounts can be transferred from one agency to another
- Transfers move the **entire account** (data, assets, automations remain intact)
- Billing responsibility changes after transfer
- Integrations (email, SMS, Stripe) usually require re-authentication after transfer

### Sub-Account Use Cases

1. **Agencies managing lead generation per client:** Each client gets their own sub-account
2. **Consultants running isolated CRM setups:** Each consulting client gets their own sub-account
3. **Multi-location businesses separating brands:** Each location gets its own sub-account
4. **White-label SaaS offers:** Agencies resell sub-accounts as their own branded SaaS

---

## Workflow & Automation (Initial Findings)

From search results, GoHighLevel has a comprehensive workflow builder:

### Workflow Builder Features

1. **Trigger-Based Automation:** Workflows are triggered by specific events (form submission, appointment booked, tag added, etc.)
2. **Multi-Channel Actions:** Workflows can send emails, SMS, voicemails, make calls, update CRM, assign tasks, etc.
3. **Visual Canvas:** Advanced builder with drag-and-drop interface, freeform canvas
4. **AI-Powered:** Workflow AI Builder generates complete automations from plain language descriptions
5. **Parallel Paths:** Support for multiple trigger paths and parallel actions
6. **Drip Mode:** Support for scheduled actions over time (e.g., 30-day nurture sequence)

### Automation Use Cases

- Lead nurture sequences (email + SMS follow-ups)
- Automated appointment booking (conversation → calendar booking without human interaction)
- Pipeline stage automation (move leads through stages based on actions)
- Client onboarding (automated welcome sequences, document requests, etc.)

---

## CRM & Pipeline Management (Initial Findings)

From search results:

### CRM Components

1. **Contacts:** Centralized contact database with custom fields, tags, and notes
2. **Pipelines:** Visual sales pipeline with customizable stages
3. **Opportunities:** Potential sales/deals at various stages of the pipeline
4. **Tasks:** Assign tasks to users, set due dates, track completion
5. **Appointments:** Calendar integration for booking and scheduling

### Pipeline Features

- **Visual Pipeline Management:** Drag-and-drop leads between stages
- **Multiple Pipelines:** Create different pipelines for different services or client types
- **Stage Automation:** Trigger workflows when leads enter/exit specific stages
- **Revenue Tracking:** Track monetary value of opportunities per stage
- **Reporting:** Dashboard showing pipeline health, conversion rates, revenue per stage

---

## White-Label & SaaS Mode

From research:

### White-Label Capabilities

1. **Desktop Application:** Custom branding (logo, colors, domain)
2. **Mobile App:** Create custom app in App Stores (additional fees apply)
3. **Client-Facing URLs:** Clients see agency's domain, not gohighlevel.com
4. **Custom Branding:** Logo, colors, email templates, etc.

### SaaS Mode

- Agencies can resell HighLevel as their own SaaS product
- Agencies set their own pricing (e.g., charge clients $497/month while paying GoHighLevel $297/month)
- Agencies handle billing and support for their clients
- Agencies can offer tiered pricing (basic, pro, enterprise) with different feature sets

### Revenue Model for Agencies

**Traditional Agency Model:**
- Charge clients for services (setup, management, optimization)
- One-time or monthly retainer fees
- Revenue tied to billable hours

**SaaS Model (White-Label):**
- Charge clients for platform access (monthly subscription)
- Charge clients for services on top of platform access
- Recurring revenue (platform fee + service fee)
- **Example:** Charge $497/month platform fee + $1,000/month management fee = $1,497/month per client

---

**End of Affiliate & Sub-Account Research**
