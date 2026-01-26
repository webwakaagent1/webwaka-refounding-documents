# WebWaka Founder Decision Table

**Purpose:** This table lists every decision that must be re-made before the rebuild can proceed. Each decision includes context, options, and tradeoffs. The final choice is left **explicitly blank** for the founder to answer.

**Instructions for Founder:** Review each decision, consider the options and tradeoffs, and fill in the "Founder Decision" column with your choice.

---

## Decision 1: Demo Mode Strategy

**Context:**
The existing Constitution declares "No Demo Mode" as a platform invariant, yet the entire system is built around demo accounts, demo tenants, demo authentication policies, and demo mode indicators. This contradiction creates confusion and makes the invariant unenforceable.

**Why This Matters:**
Demo mode affects architecture (sandboxing, reset mechanisms), security (authentication policies), and user experience (onboarding flow). A clear decision is needed before any rebuild.

**Options:**

| Option | Description | Pros | Cons |
|--------|-------------|------|------|
| **A. Remove All Demo Infrastructure** | Require real tenants for testing and onboarding. No demo accounts, no demo mode. | Clean architecture, no special cases, forces real-world testing | Harder onboarding, requires payment/signup before trial, limits experimentation |
| **B. Embrace Demo Mode as Permanent** | Design demo mode properly: sandboxed demo tenants, reset mechanisms, clearly marked demo indicators, separate demo infrastructure | Easy onboarding, safe experimentation, clear separation from production | Adds complexity, requires ongoing maintenance, risk of demo/prod confusion |
| **C. Hybrid: Temporary Demo Accounts** | Allow demo accounts that expire after 7-30 days, then require conversion to real tenant | Balance between easy onboarding and forcing commitment | Adds expiration logic, may frustrate users who lose demo data |

**Founder Decision:**  
`[BLANK — FOUNDER TO FILL]`

---

## Decision 2: Multi-Tenancy Isolation Model

**Context:**
The platform is multi-tenant, but the isolation mechanism is not clearly defined. The Constitution states "Every database query must include WHERE tenantId = ?" but there's no automated enforcement, creating risk of cross-tenant data leakage.

**Why This Matters:**
Tenant isolation is critical for security, compliance, and trust. The isolation model affects database design, query patterns, and performance.

**Options:**

| Option | Description | Pros | Cons |
|--------|-------------|------|------|
| **A. Shared Tables + Row-Level Security (RLS)** | All tenants share tables, PostgreSQL RLS enforces tenantId filtering at database level | Efficient resource usage, automatic enforcement, no ORM changes needed | RLS complexity, potential performance impact, requires PostgreSQL expertise |
| **B. Shared Tables + ORM Middleware** | All tenants share tables, Prisma middleware automatically adds WHERE tenantId = ? to all queries | Simpler than RLS, works with any database, easier to debug | Relies on application-level enforcement, risk of bypass if middleware fails |
| **C. Separate Schemas Per Tenant** | Each tenant gets a separate PostgreSQL schema (e.g., tenant_123, tenant_456) | Strong isolation, no risk of cross-tenant leakage, easier to backup/restore per tenant | Schema proliferation, connection pooling complexity, harder to query across tenants |
| **D. Separate Databases Per Tenant** | Each tenant gets a separate PostgreSQL database | Strongest isolation, independent scaling, easy to migrate tenants | High operational overhead, expensive, connection pooling nightmare |

**Founder Decision:**  
`[BLANK — FOUNDER TO FILL]`

---

## Decision 3: Offline-First + Core Integration Model

**Context:**
The platform declares "Offline-First" as an invariant, but POS is now "Core-Aware" with 30-second sync intervals. This creates tension between offline autonomy and online integration. The relationship between offline operation and Core integration needs clarification.

**Why This Matters:**
Affects sync strategy, conflict resolution, data consistency, and user experience during network outages.

**Options:**

| Option | Description | Pros | Cons |
|--------|-------------|------|------|
| **A. Offline-First with Eventual Consistency** | POS operates fully offline, syncs to Core when online, Core is read-only replica, POS always wins conflicts | Maximum offline autonomy, simple conflict resolution, no Core dependency | Stale data in Core, potential divergence, harder to enforce platform-wide rules |
| **B. Online-First with Offline Fallback** | POS prefers online operation, falls back to offline only when Core is unreachable, Core is source of truth | Consistent data, easier to enforce platform rules, simpler sync | Requires reliable connectivity, degraded experience during outages |
| **C. Hybrid with Conflict-Free Replicated Data Types (CRDTs)** | POS and Core are peers, both can write, CRDTs ensure automatic conflict resolution | No "source of truth" needed, works offline and online, automatic merging | High complexity, requires CRDT expertise, harder to debug, limited CRDT support in PostgreSQL |
| **D. Offline-First with Operational Transformation (OT)** | POS operates offline, syncs operations (not state) to Core, OT resolves conflicts | Works well for collaborative editing, proven in Google Docs, fine-grained conflict resolution | Very high complexity, requires OT expertise, harder to implement for commerce data |

**Founder Decision:**  
`[BLANK — FOUNDER TO FILL]`

---

## Decision 4: Database Provider

**Context:**
Current system uses Neon for PostgreSQL hosting. Neon is working but is a relatively new provider with less track record than AWS RDS, Google Cloud SQL, or self-hosted PostgreSQL. Need to decide whether to stay with Neon or migrate.

**Why This Matters:**
Affects cost, reliability, backup/restore guarantees, vendor lock-in, and operational complexity.

**Options:**

| Option | Description | Pros | Cons |
|--------|-------------|------|------|
| **A. Stay with Neon** | Continue using Neon for PostgreSQL hosting | Serverless, auto-scaling, branching for dev/test, good developer experience | Newer provider, less track record, unclear pricing at scale, vendor lock-in |
| **B. Migrate to AWS RDS** | Use AWS RDS for PostgreSQL | Battle-tested, enterprise-grade, extensive backup/restore options, global presence | More expensive, requires AWS expertise, slower to provision |
| **C. Migrate to Google Cloud SQL** | Use Google Cloud SQL for PostgreSQL | Battle-tested, good integration with GCP services, automatic backups | More expensive, requires GCP expertise, slower to provision |
| **D. Migrate to Supabase** | Use Supabase (PostgreSQL + realtime + auth + storage) | All-in-one platform, good developer experience, open source | Vendor lock-in, less mature than AWS/GCP, pricing unclear at scale |
| **E. Self-Host PostgreSQL** | Run PostgreSQL on own infrastructure (e.g., DigitalOcean, Hetzner, or on-prem) | Full control, no vendor lock-in, potentially cheaper at scale | High operational overhead, requires DBA expertise, responsible for backups/HA |

**Founder Decision:**  
`[BLANK — FOUNDER TO FILL]`

---

## Decision 5: Frontend Hosting Provider

**Context:**
Current system uses Vercel for frontend deployment (Super Admin UI, Partner Dashboard, POS UI). Vercel is working but may not be cost-effective at scale and has vendor lock-in risk.

**Why This Matters:**
Affects cost, performance, vendor lock-in, and operational complexity.

**Options:**

| Option | Description | Pros | Cons |
|--------|-------------|------|------|
| **A. Stay with Vercel** | Continue using Vercel for frontend hosting | Excellent developer experience, automatic deployments, edge network, zero config | Expensive at scale, vendor lock-in, cold starts on free tier |
| **B. Migrate to Netlify** | Use Netlify for frontend hosting | Similar to Vercel, good developer experience, edge network | Similar pricing concerns, vendor lock-in |
| **C. Migrate to Cloudflare Pages** | Use Cloudflare Pages for frontend hosting | Cheaper than Vercel/Netlify, excellent global CDN, good developer experience | Less mature, fewer integrations |
| **D. Migrate to AWS Amplify** | Use AWS Amplify for frontend hosting | Tight AWS integration, enterprise-grade, good for large orgs | More complex setup, requires AWS expertise, slower iteration |
| **E. Self-Host with Nginx + CDN** | Deploy Next.js to own servers with Nginx, use Cloudflare/BunnyCDN for CDN | Full control, no vendor lock-in, potentially cheaper at scale | High operational overhead, requires DevOps expertise, responsible for uptime |

**Founder Decision:**  
`[BLANK — FOUNDER TO FILL]`

---

## Decision 6: Backend Hosting Provider

**Context:**
Current system uses Fly.io for Core API deployment. Fly.io is working but has had operational issues (restart exhaustion, SSL provisioning delays, machine crashes). Need to decide whether to stay with Fly.io or migrate.

**Why This Matters:**
Affects reliability, cost, operational complexity, and vendor lock-in.

**Options:**

| Option | Description | Pros | Cons |
|--------|-------------|------|------|
| **A. Stay with Fly.io** | Continue using Fly.io for backend hosting | Good developer experience, global edge deployment, reasonable pricing | Operational issues observed, smaller company, less enterprise support |
| **B. Migrate to Railway** | Use Railway for backend hosting | Excellent developer experience, simple pricing, good for startups | Smaller company, less mature, unclear reliability at scale |
| **C. Migrate to Render** | Use Render for backend hosting | Good developer experience, simple pricing, automatic deployments | Smaller company, less mature, cold starts on free tier |
| **D. Migrate to AWS ECS/Fargate** | Use AWS ECS with Fargate for backend hosting | Battle-tested, enterprise-grade, extensive AWS ecosystem | More complex setup, requires AWS expertise, slower iteration |
| **E. Migrate to Google Cloud Run** | Use Google Cloud Run for backend hosting | Serverless, auto-scaling, good pricing, simple deployment | Requires GCP expertise, less mature than AWS |
| **F. Self-Host with Docker + Kubernetes** | Deploy backend to own Kubernetes cluster (e.g., DigitalOcean, Hetzner, or on-prem) | Full control, no vendor lock-in, potentially cheaper at scale | Very high operational overhead, requires Kubernetes expertise, responsible for uptime |

**Founder Decision:**  
`[BLANK — FOUNDER TO FILL]`

---

## Decision 7: Backend Framework

**Context:**
Current system uses Fastify for Core API. Fastify is working but was declared "CANON LOCKED - FINAL" without clear rationale. Need to reconsider whether Fastify is the best choice or if alternatives (Express, NestJS, Hono) are better.

**Why This Matters:**
Affects performance, ecosystem, developer familiarity, and hiring pool.

**Options:**

| Option | Description | Pros | Cons |
|--------|-------------|------|------|
| **A. Stay with Fastify** | Continue using Fastify for backend | Fast, modern, good TypeScript support, schema-based validation | Smaller ecosystem than Express, fewer developers know it, less mature plugins |
| **B. Migrate to Express** | Use Express for backend | Largest ecosystem, most developers know it, battle-tested, extensive middleware | Slower than Fastify, older design, less TypeScript-friendly |
| **C. Migrate to NestJS** | Use NestJS (opinionated framework on top of Express/Fastify) | Opinionated structure, excellent TypeScript support, dependency injection, modular | Heavier, more boilerplate, steeper learning curve |
| **D. Migrate to Hono** | Use Hono (modern, fast, edge-first framework) | Very fast, edge-first, good TypeScript support, modern design | Newer, smaller ecosystem, less mature |
| **E. Migrate to tRPC + Next.js API Routes** | Use tRPC for type-safe API with Next.js API routes | End-to-end type safety, no API layer needed, simpler stack | Tight coupling between frontend and backend, harder to split later |

**Founder Decision:**  
`[BLANK — FOUNDER TO FILL]`

---

## Decision 8: Monolithic vs. Microservices Architecture

**Context:**
Current system has a monolithic Core API with all domains (Identity, Tenants, Roles, Permissions, Partners, Audit, Feature Flags, Entitlements, Modules, Pricing, Branding). Need to decide whether to stay monolithic or plan for microservices migration.

**Why This Matters:**
Affects scalability, deployment complexity, team structure, and operational overhead.

**Options:**

| Option | Description | Pros | Cons |
|--------|-------------|------|------|
| **A. Stay Monolithic** | Keep all domains in one Core API | Simpler to develop, easier to deploy, lower operational overhead, easier to refactor | May become unwieldy at scale, harder to scale individual domains, single point of failure |
| **B. Migrate to Microservices Now** | Split Core API into separate services (Identity Service, Tenant Service, etc.) | Better isolation, independent scaling, independent deployment, clearer ownership | High operational overhead, requires service mesh, harder to debug, premature at current scale |
| **C. Modular Monolith (Prepare for Microservices)** | Keep monolithic deployment but structure code as independent modules with clear boundaries | Simpler than microservices, easier to split later, clear boundaries | Requires discipline to maintain boundaries, risk of coupling creep |
| **D. Defer Decision (Start Monolithic, Split When Needed)** | Start with monolith, define criteria for splitting (e.g., team size > 20, deployment frequency > 10/day), split only when criteria are met | Pragmatic, avoids premature optimization, adapts to actual needs | Risk of delaying too long and making split harder |

**Founder Decision:**  
`[BLANK — FOUNDER TO FILL]`

---

## Decision 9: Billing Model and Payment Gateway

**Context:**
Current system has Pricing Plans but no billing integration. Platform cannot generate revenue. Need to choose billing model and payment gateway.

**Why This Matters:**
Affects revenue model, user experience, and integration complexity.

**Billing Model Options:**

| Option | Description | Pros | Cons |
|--------|-------------|------|------|
| **A. Subscription (Monthly/Annual)** | Tenants pay fixed monthly or annual fee for access | Predictable revenue, simple to understand, easy to implement | May not align with usage, hard to upsell |
| **B. Usage-Based (Per Transaction)** | Tenants pay per transaction (e.g., per sale, per order) | Aligns with value, scales with customer growth, fair pricing | Unpredictable revenue, harder to implement, requires metering |
| **C. Hybrid (Subscription + Usage)** | Base subscription + overage fees for high usage | Predictable base revenue + upside from high-usage customers | More complex to implement and explain |
| **D. Freemium (Free Tier + Paid Upgrades)** | Free tier with limited features, paid upgrades for more | Easy customer acquisition, viral growth potential | May cannibalize paid users, requires feature gating |

**Payment Gateway Options:**

| Option | Description | Pros | Cons |
|--------|-------------|------|------|
| **A. Paystack** | Nigerian payment gateway (cards, bank transfers, USSD) | Nigeria-first, local support, good for NGN payments | Limited international support |
| **B. Flutterwave** | African payment gateway (cards, mobile money, bank transfers) | Pan-African, good for multiple African markets | More expensive than Paystack |
| **C. Stripe** | Global payment gateway (cards, wallets, bank transfers) | Best developer experience, global reach, extensive features | Higher fees in Nigeria, requires USD pricing |
| **D. Multi-Gateway (Paystack + Stripe)** | Use Paystack for Nigerian customers, Stripe for international | Best of both worlds, local and global support | More complex integration, requires gateway abstraction |

**Founder Decision (Billing Model):**  
`[BLANK — FOUNDER TO FILL]`

**Founder Decision (Payment Gateway):**  
`[BLANK — FOUNDER TO FILL]`

---

## Decision 10: Deployment Promotion Mechanism

**Context:**
Current system has unclear deployment behavior (may auto-deploy on main branch). Constitution declares "Manual promotion is required" but it's not implemented. Need to decide on promotion mechanism.

**Why This Matters:**
Affects deployment safety, velocity, and operational overhead.

**Options:**

| Option | Description | Pros | Cons |
|--------|-------------|------|------|
| **A. Manual Promotion with Checklist** | Require manual approval before production deployment, enforce verification checklist | Maximum safety, human review before production | Slower velocity, requires on-call human, bottleneck |
| **B. Automated Deployment with Comprehensive Testing** | Auto-deploy to production if all tests pass (unit, integration, E2E), auto-rollback on failure | Fast velocity, no human bottleneck, forces good testing | Requires excellent test coverage, risk of bad deploy if tests miss issues |
| **C. Hybrid (Auto to Staging, Manual to Production)** | Auto-deploy to staging, require manual promotion to production | Balance between safety and velocity, staging catches most issues | Still requires human for production promotion |
| **D. Canary Deployment with Auto-Rollback** | Auto-deploy to 5% of users, monitor metrics, auto-rollback if metrics degrade, gradually roll out to 100% | Safe, fast, no human needed, catches production-only issues | Requires sophisticated monitoring and rollback automation |

**Founder Decision:**  
`[BLANK — FOUNDER TO FILL]`

---

## Decision 11: Secrets Management System

**Context:**
Current system stores plaintext credentials in Constitution document. This is a massive security risk. Need to migrate to proper secrets management.

**Why This Matters:**
Affects security, compliance, and credential rotation.

**Options:**

| Option | Description | Pros | Cons |
|--------|-------------|------|------|
| **A. 1Password (or similar password manager)** | Store credentials in 1Password, share with team via vaults | Simple, good UX, works for humans and CI/CD | Not designed for programmatic access, requires 1Password CLI |
| **B. HashiCorp Vault** | Use Vault for secrets management with dynamic secrets and rotation | Enterprise-grade, dynamic secrets, fine-grained access control, audit logs | High operational overhead, requires Vault expertise, overkill for small teams |
| **C. AWS Secrets Manager** | Use AWS Secrets Manager for secrets storage and rotation | Tight AWS integration, automatic rotation, good for AWS-heavy stacks | Vendor lock-in, requires AWS, more expensive |
| **D. GitHub Secrets (for CI/CD only)** | Use GitHub Secrets for CI/CD credentials, 1Password for human access | Simple, free, works well for CI/CD | Not suitable for runtime secrets (backend needs secrets at runtime) |
| **E. Environment Variables + Encrypted Files** | Store secrets in environment variables, commit encrypted .env files to repo | Simple, no external service needed, works everywhere | Manual rotation, risk of committing unencrypted secrets, no audit logs |

**Founder Decision:**  
`[BLANK — FOUNDER TO FILL]`

---

## Decision 12: Module System Implementation

**Context:**
Constitution declares "Modularity" as a platform invariant with installable, removable, verifiable modules. Module Registry exists but is not integrated into Core API. Need to decide whether to implement true runtime modularity or simplify.

**Why This Matters:**
Affects architecture complexity, extensibility, and development velocity.

**Options:**

| Option | Description | Pros | Cons |
|--------|-------------|------|------|
| **A. Implement True Runtime Modularity** | Build plugin system with dynamic loading, module manifests, dependency resolution, and verification | Maximum extensibility, third-party modules possible, aligns with original vision | Very high complexity, requires plugin architecture expertise, slower development |
| **B. Simplify to Logical Separation** | Organize code into modules (folders/packages) but no runtime loading, all modules compiled into Core API | Simpler, faster development, clear code organization | Not truly "installable/removable," requires redeployment to add/remove modules |
| **C. Defer Until Platform Reaches Scale** | Start with monolithic Core API, revisit modularity when there's clear need (e.g., third-party integrations) | Pragmatic, avoids premature optimization, adapts to actual needs | Risk of delaying too long and making modularization harder |

**Founder Decision:**  
`[BLANK — FOUNDER TO FILL]`

---

## Decision 13: Repository Structure

**Context:**
Current plan has 11+ repositories (webwaka-core-registry, webwaka-core-identity, webwaka-core-payments, etc.) but only 2 are active. Need to decide on repository structure for the rebuild.

**Why This Matters:**
Affects coordination overhead, CI/CD complexity, and code sharing.

**Options:**

| Option | Description | Pros | Cons |
|--------|-------------|------|------|
| **A. Monorepo (All Code in One Repo)** | Single repository with packages for Core API, Super Admin UI, Partner Dashboard, POS, etc. | Easy code sharing, atomic commits across packages, simpler CI/CD | Large repo, requires monorepo tooling (Turborepo, Nx), harder to enforce boundaries |
| **B. Minimal Split (Backend + Frontend)** | Two repositories: webwaka-backend (Core API), webwaka-frontend (all UIs) | Simple, clear separation, easy to manage | Harder to deploy UIs independently, frontend repo may become large |
| **C. Moderate Split (Core + Suites)** | Three repositories: webwaka-core-api, webwaka-suite-admin, webwaka-suite-pos | Balance between simplicity and independence, aligns with CORE/SUITE distinction | Requires shared packages (e.g., types, utilities), coordination overhead |
| **D. Full Split (One Repo Per Service)** | 11+ repositories as originally planned | Maximum independence, clear ownership, easy to deploy independently | High coordination overhead, harder to share code, version management nightmare |

**Founder Decision:**  
`[BLANK — FOUNDER TO FILL]`

---

## Decision 14: Development Environment Strategy

**Context:**
Current system has shared dev/prod infrastructure (same database, same Clerk instance, same Fly.io app) with no documented safeguards. Need to decide on dev/prod separation.

**Why This Matters:**
Affects blast-radius risk, development velocity, and cost.

**Options:**

| Option | Description | Pros | Cons |
|--------|-------------|------|------|
| **A. Shared Infrastructure with Safeguards** | Keep shared infrastructure, implement safeguards (tenant prefixes, RBAC, separate schemas) | Lower cost, simpler to manage, forces realistic testing | Risk of accidental production corruption, requires discipline |
| **B. Fully Isolated Dev/Prod** | Separate database, separate Clerk instance, separate backend, separate everything | Maximum safety, no risk of dev affecting prod | Higher cost, more operational overhead, dev/prod drift |
| **C. Hybrid (Shared Database, Isolated Services)** | Shared database with tenant prefixes, separate Clerk instances, separate backend deployments | Balance between cost and safety | Partial isolation, still some blast-radius risk |
| **D. Local Dev Only (No Shared Dev Environment)** | Developers run everything locally, only production environment exists | No shared dev environment to manage, no dev/prod drift | Harder to test integrations, requires local setup for all services |

**Founder Decision:**  
`[BLANK — FOUNDER TO FILL]`

---

## Decision 15: Operator Governance (Manus, Emergent, Replit)

**Context:**
Constitution defines vibecoding platform governance (Emergent builds logic, Lovable builds UI, Replit audits) but this is not being followed. Manus is doing all the work. Need to decide on operator governance for the rebuild.

**Why This Matters:**
Affects development velocity, tool selection, and coordination overhead.

**Options:**

| Option | Description | Pros | Cons |
|--------|-------------|------|------|
| **A. Manus Only** | Use Manus for all development (logic, UI, infrastructure, audits) | Simplest, no coordination overhead, single workflow | May not be optimal for all tasks, limited by Manus capabilities |
| **B. Manus + Specialized Tools (As Needed)** | Use Manus as primary, bring in specialized tools (Emergent, Lovable, v0) only when clearly beneficial | Pragmatic, adapts to task needs, avoids premature governance | Requires judgment calls, may introduce coordination overhead |
| **C. Strict Vibecoding Governance (As Originally Planned)** | Follow Repository Responsibility Atlas: Emergent for logic, Lovable for UI, Replit for audits | Clear separation of concerns, leverages tool strengths | High coordination overhead, requires all tools to be available and suitable |
| **D. Founder as Primary Developer (Minimal AI Assistance)** | Founder writes code directly, uses AI tools only for assistance (copilot, code review) | Maximum control, no AI limitations, deep understanding | Slower development, requires founder to have development skills |

**Founder Decision:**  
`[BLANK — FOUNDER TO FILL]`

---

## Summary

**Total Decisions Required:** 15

**Categories:**
- Architecture: 5 decisions (Demo Mode, Isolation, Offline/Online, Monolith/Microservices, Modules)
- Infrastructure: 4 decisions (Database, Frontend Hosting, Backend Hosting, Dev/Prod Strategy)
- Tooling: 3 decisions (Backend Framework, Secrets Management, Repository Structure)
- Business: 1 decision (Billing Model + Payment Gateway)
- Operations: 2 decisions (Deployment Promotion, Operator Governance)

**Next Steps:**
1. Founder reviews this table
2. Founder fills in all "Founder Decision" fields
3. Decisions are incorporated into the Re-Founding Blueprint
4. Blueprint becomes the new canonical execution authority

---

**End of Founder Decision Table**
