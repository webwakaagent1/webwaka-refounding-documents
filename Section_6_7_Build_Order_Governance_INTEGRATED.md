# SECTION 6: STRICT, SEQUENTIAL BUILD ORDER

**Status:** This section defines the implementation phasing that respects all Canonically Locked Founder Decisions.

---

## 6.1 Build Order Philosophy

### Architecture vs. Implementation

**CRITICAL DISTINCTION:**

- **Architecture is designed for full scope from day one** (Max-Scale-First, 🔒 Locked)
- **Implementation is phased** (to manage complexity and risk)

**What This Means:**

- All data models support max scale (1,000+ partners, 1,000,000+ tenants)
- All APIs support full feature set (even if not all features are implemented yet)
- All infrastructure supports max load (even if current load is low)

**Implementation phases are about WHEN we build features, not HOW we architect them.**

---

## 6.2 Phase 1: Core Infrastructure + Commerce Suites (3-6 months)

### 6.2.1 Goals

- Establish AWS-native infrastructure
- Build core platform primitives
- Implement Commerce Suite (POS, Inventory, Orders)
- Launch MVP for Nigerian market

### 6.2.2 Prerequisites

**MUST be completed before Phase 1 starts:**

1. ✅ Founder Decisions finalized (DONE)
2. ✅ Blueprint approved (DONE)
3. AWS accounts provisioned
4. Domain names purchased
5. GitHub repository initialized

### 6.2.3 Phase 1 Deliverables

#### Infrastructure (AWS-Native)

| Component | Service | Status |
|-----------|---------|--------|
| **Backend Hosting** | AWS Fargate | Required |
| **Frontend Hosting** | AWS Amplify | Required |
| **Database** | AWS Aurora (PostgreSQL) | Required |
| **Authentication** | AWS Cognito | Required |
| **Email** | AWS SES | Required |
| **SMS** | AWS SNS | Required |
| **Storage** | AWS S3 + CloudFront | Required |
| **Queues** | AWS SQS | Required |
| **Events** | AWS EventBridge | Required |
| **Monitoring** | AWS CloudWatch + X-Ray | Required |

#### Platform Primitives (Phase 1 Subset)

| Primitive | Phase 1 Scope | Locked Decisions |
|-----------|---------------|------------------|
| **CRM Domain** | Contacts, Companies, basic pipeline | Recursive usage (🔒 #14) |
| **Billing Domain** | Invoicing, payments (NGN-only) | Centralized billing (🔒 #9), Hierarchical pricing (🔒 #8), NGN-only (🔒 #10) |
| **Affiliate Domain** | Closure table, fixed percentages, up to 10 levels | Closure table (🔒 #1), Hierarchical override (🔒 #2), Fixed percentages (🔒 #3), Platform-managed payouts (🔒 #4) |
| **Communication Domain** | Email templates, SMS (basic) | Recursive usage (🔒 #14) |
| **Reporting Domain** | Basic dashboards | Recursive usage (🔒 #14) |

#### Industry Suites (Phase 1)

| Suite | Phase 1 Scope | Locked Decisions |
|-------|---------------|------------------|
| **Commerce Suite** | POS, Inventory, Orders, Payments | Platform-only modules (🔒 #5) |

#### Partner Portal (Phase 1)

| Feature | Phase 1 Scope | Locked Decisions |
|---------|---------------|------------------|
| **Tenant Management** | Create/edit/delete clients | Platform kill-switch (🔒 #13) |
| **Pricing Management** | Set retail prices | Hierarchical pricing (🔒 #8) |
| **Branding Management** | Logo, colors, custom domain | Full white-label (🔒 #6) |
| **Affiliate Management** | Configure commission rules | Hierarchical override (🔒 #2) |

### 6.2.4 Phase 1 Constraints

**Module Creation Authority:** Platform-only (🔒 Locked Decision #5)

- Partners CANNOT create custom modules in Phase 1
- All modules are built by WebWaka
- This ensures quality, consistency, and security

**Currency:** NGN-only (🔒 Locked Decision #10)

- All pricing, billing, and payments in Nigerian Naira
- Architecture must be multi-currency ready (for Phase 2+)

**Data Isolation:** Shared Database + Row-Level Security (🔒 Locked Decision #7)

- All partners/tenants in same database
- PostgreSQL RLS enforces isolation

**Identity Model:** Tenant-Scoped Identity (🔒 Locked Decision #11)

- Users belong to specific tenants
- No global user directory

### 6.2.5 Phase 1 Success Criteria

1. ✅ 10+ partners onboarded
2. ✅ 100+ clients (tenants) active
3. ✅ 1,000+ end users
4. ✅ 10,000+ transactions processed
5. ✅ 99.9% uptime
6. ✅ All Canonically Locked decisions implemented

---

## 6.3 Phase 2: Composable Primitives + Affiliate System (6-9 months)

### 6.3.1 Goals

- Complete all platform primitives
- Enable partner extensibility (with approval)
- Expand to multi-currency
- Launch 3+ additional industry suites

### 6.3.2 Phase 2 Deliverables

#### Platform Primitives (Phase 2 Completion)

| Primitive | Phase 2 Scope | Locked Decisions |
|-----------|---------------|------------------|
| **Automation Domain** | Workflows, triggers, actions | Recursive usage (🔒 #14) |
| **Site Builder Domain** | Landing pages, funnels | Recursive usage (🔒 #14) |
| **Forms Domain** | Lead capture, surveys | Recursive usage (🔒 #14) |
| **Calendar Domain** | Appointments, scheduling | Recursive usage (🔒 #14) |
| **API Gateway** | Webhooks, integrations | Recursive usage (🔒 #14) |

#### Industry Suites (Phase 2)

| Suite | Phase 2 Scope | Locked Decisions |
|-------|---------------|------------------|
| **Education Suite** | Courses, Students, Assignments | Partner-extensible (🔒 #5) |
| **Health Suite** | Patients, Appointments, Records | Partner-extensible (🔒 #5) |
| **Civic Suite** | Citizens, Services, Permits | Partner-extensible (🔒 #5) |

#### Partner Extensibility (Phase 2)

**Module Creation Authority:** Partner-extensible (🔒 Locked Decision #5)

- Partners CAN create custom modules in Phase 2
- Approval workflow required (WebWaka reviews and approves)
- Module marketplace vision

**Approval Workflow:**

1. Partner submits module for review
2. WebWaka reviews code, security, quality
3. WebWaka approves or rejects
4. Approved modules published to marketplace
5. Other partners can install approved modules

#### Multi-Currency Support (Phase 2)

**Currency Expansion:** (🔒 Locked Decision #10)

- Add USD, KES, ZAR, GHS
- Multi-currency invoices
- Currency conversion
- Multi-currency reporting

### 6.3.3 Phase 2 Success Criteria

1. ✅ 50+ partners onboarded
2. ✅ 1,000+ clients (tenants) active
3. ✅ 100,000+ end users
4. ✅ 1,000,000+ transactions processed
5. ✅ 99.95% uptime
6. ✅ 10+ partner-created modules approved

---

## 6.4 Phase 3: Multi-Industry Expansion (9-12 months)

### 6.4.1 Goals

- Launch remaining industry suites
- Expand to additional African countries
- Enable advanced partner features
- Achieve GoHighLevel differentiation

### 6.4.2 Phase 3 Deliverables

#### Industry Suites (Phase 3)

| Suite | Phase 3 Scope |
|-------|---------------|
| **Hospitality Suite** | Reservations, Guests, Rooms |
| **Logistics Suite** | Shipments, Tracking, Routes |

#### Geographic Expansion

- Kenya (KES)
- South Africa (ZAR)
- Ghana (GHS)
- Additional African countries

#### Advanced Partner Features

- Partner-to-partner marketplace
- White-label mobile apps
- Advanced automation (AI-powered)
- Advanced reporting (predictive analytics)

### 6.4.3 Phase 3 Success Criteria

1. ✅ 200+ partners onboarded
2. ✅ 10,000+ clients (tenants) active
3. ✅ 1,000,000+ end users
4. ✅ 10,000,000+ transactions processed
5. ✅ 99.99% uptime
6. ✅ 100+ partner-created modules approved

---

## 6.5 Dependency Graph

### Structural Dependencies (MUST be built in order)

```
Phase 1.1: Core Infrastructure (AWS)
    ↓
Phase 1.2: Authentication & Multi-Tenancy (AWS Cognito + RLS)
    ↓
Phase 1.3: Core Primitives (CRM, Billing, Affiliate, Communication, Reporting)
    ↓
Phase 1.4: Commerce Suite (POS, Inventory, Orders)
    ↓
Phase 1.5: Partner Portal (Tenant Management, Pricing, Branding, Affiliate)
    ↓
Phase 2.1: Complete Primitives (Automation, Site Builder, Forms, Calendar, API Gateway)
    ↓
Phase 2.2: Additional Suites (Education, Health, Civic)
    ↓
Phase 2.3: Partner Extensibility (Module Marketplace)
    ↓
Phase 2.4: Multi-Currency Support
    ↓
Phase 3.1: Remaining Suites (Hospitality, Logistics)
    ↓
Phase 3.2: Geographic Expansion
    ↓
Phase 3.3: Advanced Partner Features
```

### Optional Dependencies (Can be built in parallel)

- **Reporting Domain** can be built in parallel with other primitives
- **Communication Domain** can be built in parallel with CRM
- **Industry Suites** can be built in parallel with each other (after primitives are complete)

---

## 6.6 Build Order Enforcement

### Forbidden Actions

**DO NOT:**

1. ❌ Build Phase 2 features before Phase 1 is complete
2. ❌ Enable partner extensibility before Phase 2
3. ❌ Add multi-currency support before Phase 2
4. ❌ Build industry suites before primitives are complete
5. ❌ Skip any structural dependencies

### Allowed Flexibility

**CAN:**

1. ✅ Build primitives in parallel (if no dependencies)
2. ✅ Build industry suites in parallel (after primitives are complete)
3. ✅ Adjust phase timelines based on progress
4. ✅ Add new features to existing phases (if no dependencies)

---

*End of Section 6: Strict, Sequential Build Order*

---

# SECTION 7: GOVERNANCE & OPERATOR RULES

**Status:** This section defines governance rules that enforce all Canonically Locked Founder Decisions.

---

## 7.1 Governance Principles

### Principle #1: Canonically Locked Decisions Are Final

**All 15 Founder Decisions are CANONICALLY LOCKED and may not be revisited without explicit Founder approval.**

**Operators MUST:**
- Read and understand all 15 decisions before starting work
- Align all implementation with locked decisions
- Escalate any conflicts to Founder immediately

**Operators MUST NOT:**
- Revisit locked decisions without Founder approval
- Implement alternatives to locked decisions
- Propose changes to locked decisions (without Founder approval)

### Principle #2: AWS-First, Always

**All tooling decisions must be AWS-first unless AWS-native options are insufficient.**

**Operators MUST:**
- Justify why AWS-native options are insufficient (if proposing third-party tools)
- Prioritize AWS-native services for all new features
- Consolidate all infrastructure costs on single AWS bill

**Operators MUST NOT:**
- Default to third-party SaaS for convenience
- Add new third-party tools without justification
- Ignore AWS-native alternatives

### Principle #3: Max-Scale-First, Always

**All architecture decisions must assume max scale from day one.**

**Operators MUST:**
- Design for 1,000+ partners, 1,000,000+ tenants, 100,000,000+ end users
- Ensure all data models support max scale
- Ensure all APIs are stateless and horizontally scalable

**Operators MUST NOT:**
- Make "we'll scale later" decisions
- Assume small-scale architecture is sufficient
- Ignore scalability implications

### Principle #4: Recursive System Usage, Always

**ALL platform primitives must be recursively usable across all hierarchy levels.**

**Operators MUST:**
- Ensure all primitives support multi-tenancy
- Ensure all primitives support white-labeling
- Ensure Super Admin → Partners → Clients → End Users all use same systems

**Operators MUST NOT:**
- Build "admin-only" features
- Build "partner-only" features
- Build non-recursive systems

---

## 7.2 Operator Roles & Responsibilities

### 7.2.1 Manus AI Operator

**Role:** Primary AI operator for WebWaka platform development

**Responsibilities:**
- Implement all features according to Blueprint
- Enforce all Canonically Locked decisions
- Commit all code/documents to GitHub immediately
- Escalate conflicts to Founder

**Authority:**
- Can implement any feature in current phase
- Can make technical decisions within locked constraints
- Cannot revisit locked decisions without Founder approval

### 7.2.2 Emergent AI Operator (if needed)

**Role:** Specialized AI operator for complex tasks

**Responsibilities:**
- Same as Manus AI Operator
- Focus on specialized tasks (e.g., complex algorithms, performance optimization)

**Authority:**
- Same as Manus AI Operator

### 7.2.3 Replit AI Operator (if needed)

**Role:** Specialized AI operator for rapid prototyping

**Responsibilities:**
- Same as Manus AI Operator
- Focus on rapid prototyping and experimentation

**Authority:**
- Same as Manus AI Operator

### 7.2.4 Founder

**Role:** Final decision authority

**Responsibilities:**
- Approve/reject all major decisions
- Resolve conflicts between operators
- Approve changes to Canonically Locked decisions

**Authority:**
- Can override any decision
- Can change any locked decision
- Can add/remove operators

---

## 7.3 Multi-Operator Coordination (if needed)

### 7.3.1 When to Use Multiple Operators

**Use multiple operators when:**
- Task requires specialized expertise (e.g., complex algorithms)
- Task requires parallel work (e.g., frontend + backend)
- Single operator is blocked or unavailable

**DO NOT use multiple operators when:**
- Task is simple and can be done by single operator
- Task requires deep context (single operator is better)
- Coordination overhead exceeds benefits

### 7.3.2 Coordination Rules

**If multiple operators are used:**
- Each operator must read entire Blueprint before starting
- Each operator must commit code/documents to GitHub immediately
- Each operator must communicate via GitHub issues/PRs
- Founder resolves conflicts between operators

---

## 7.4 Repository Structure & Workflow

### 7.4.1 Monorepo (Phase 1)

**Repository Structure:**

```
webwaka-platform/
├── docs/                    # All documentation
│   ├── blueprint.md         # This Blueprint
│   ├── architecture.md      # Architecture diagrams
│   └── decisions/           # Decision records
├── backend/                 # Backend API (Node.js + TypeScript)
│   ├── src/
│   ├── tests/
│   └── package.json
├── frontend/                # Frontend (React + TypeScript + TailwindCSS)
│   ├── src/
│   ├── tests/
│   └── package.json
├── infrastructure/          # AWS infrastructure (Terraform or CDK)
│   ├── terraform/
│   └── cdk/
├── database/                # Database migrations (Prisma or Drizzle)
│   ├── migrations/
│   └── schema.prisma
└── README.md
```

### 7.4.2 Git Workflow

**Branch Strategy:**

- **main:** Production-ready code
- **develop:** Development branch
- **feature/*:** Feature branches

**Commit Rules:**

- All commits must be pushed immediately before stopping work
- All commits must have descriptive messages
- All commits must reference related issues (if applicable)

**Pull Request Rules:**

- All PRs must be reviewed by Founder (or designated reviewer)
- All PRs must pass CI/CD checks
- All PRs must update documentation (if needed)

### 7.4.3 Documentation Discipline

**MANDATORY:**

- All documents must be committed to GitHub
- All code changes must be pushed immediately before stopping work
- All future operators must be able to reconstruct context from repo alone

**Operators MUST:**
- Update Blueprint when making architectural changes
- Update README when adding new features
- Update decision records when making decisions

**Operators MUST NOT:**
- Leave uncommitted work
- Leave undocumented decisions
- Leave incomplete features in main branch

---

## 7.5 Configuration Authority Enforcement

### 7.5.1 Hierarchical Override Rules

**Configuration Authority:** Global → Partner → Contract → Org (🔒 Locked Decision #2)

**Operators MUST:**
- Implement hierarchical override for ALL configurable settings
- Ensure most specific configuration wins
- Ensure configuration inheritance works correctly

**Operators MUST NOT:**
- Hardcode configuration values
- Ignore hierarchical override rules
- Allow configuration conflicts

### 7.5.2 Pricing Authority Enforcement

**Pricing Authority:** Global → Partner → Client (🔒 Locked Decision #8)

**Operators MUST:**
- Implement hierarchical pricing for ALL pricing plans
- Ensure partners can set their own retail prices
- Ensure pricing inheritance works correctly

**Operators MUST NOT:**
- Hardcode pricing values
- Ignore pricing authority rules
- Allow pricing conflicts

### 7.5.3 Affiliate Configuration Enforcement

**Affiliate Configuration:** Hierarchical Override (🔒 Locked Decision #2)

**Operators MUST:**
- Implement hierarchical override for affiliate commission rules
- Ensure most specific configuration wins
- Ensure affiliate configuration inheritance works correctly

**Operators MUST NOT:**
- Hardcode affiliate commission percentages
- Ignore affiliate configuration rules
- Allow affiliate configuration conflicts

---

## 7.6 Kill-Switch Authority Enforcement

**Platform Kill-Switch:** Super Admin authority (🔒 Locked Decision #13)

**Operators MUST:**
- Implement "active" status flag on all entities (partners, clients, users)
- Check "active" status before processing any request
- Display "account suspended" message when inactive

**Operators MUST NOT:**
- Allow inactive entities to access platform
- Allow inactive entities to process transactions
- Bypass "active" status checks

---

## 7.7 Data Ownership & Export Enforcement

**Data Ownership:** Tenant owns data with full export rights (🔒 Locked Decision #12)

**Operators MUST:**
- Implement data export API for all tenants
- Support JSON, CSV, SQL export formats
- Include all entities in exports (contacts, deals, invoices, etc.)

**Operators MUST NOT:**
- Restrict tenant data exports
- Use tenant data for other purposes (without consent)
- Deny export requests

---

## 7.8 Recursive System Usage Enforcement

**Recursive Usage:** ALL platform primitives (🔒 Locked Decision #14)

**Operators MUST:**
- Ensure all primitives support multi-tenancy
- Ensure all primitives support white-labeling
- Ensure Super Admin → Partners → Clients → End Users all use same systems

**Operators MUST NOT:**
- Build "admin-only" features
- Build "partner-only" features
- Build non-recursive systems

---

## 7.9 Conflict Resolution

### 7.9.1 Technical Conflicts

**If operators disagree on technical approach:**

1. Operators discuss and try to reach consensus
2. If no consensus, escalate to Founder
3. Founder makes final decision
4. All operators must align with Founder decision

### 7.9.2 Locked Decision Conflicts

**If implementation conflicts with locked decision:**

1. Operator must stop work immediately
2. Operator must escalate to Founder
3. Founder decides whether to:
   - Change implementation to align with locked decision
   - Change locked decision (rare)
4. Operator resumes work after Founder decision

### 7.9.3 AWS-First Conflicts

**If AWS-native option is insufficient:**

1. Operator must document why AWS-native option is insufficient
2. Operator must propose third-party alternative
3. Operator must escalate to Founder
4. Founder approves or rejects third-party alternative

---

## 7.10 Quality Standards

### 7.10.1 Code Quality

**Operators MUST:**
- Write clean, readable, maintainable code
- Follow TypeScript/JavaScript best practices
- Write unit tests for all business logic
- Write integration tests for all APIs

### 7.10.2 Documentation Quality

**Operators MUST:**
- Update documentation when making changes
- Write clear, concise, accurate documentation
- Include examples and diagrams where helpful

### 7.10.3 Performance Standards

**Operators MUST:**
- Ensure all APIs respond within 200ms (p95)
- Ensure all pages load within 2 seconds (p95)
- Ensure all background jobs complete within 5 minutes

### 7.10.4 Security Standards

**Operators MUST:**
- Follow OWASP Top 10 security guidelines
- Implement authentication/authorization correctly
- Sanitize all user inputs
- Use HTTPS for all communications

---

*End of Section 7: Governance & Operator Rules*
