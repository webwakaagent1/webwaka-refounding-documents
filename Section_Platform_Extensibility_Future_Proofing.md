# SECTION: PLATFORM EXTENSIBILITY, COMPOSABILITY & FUTURE-PROOFING

**Status:** 🔒 CANONICALLY LOCKED — This is a hard architectural invariant, not an aspiration.

---

## 1. Foundational Principle (Non-Negotiable)

**WebWaka is designed as a Platform for Building Platforms.**

Therefore:

**Every system, module, service, UI, workflow, AI capability, and integration built today MUST be designed such that unknown future capabilities can be added later as plug-ins, without breaking, refactoring, or rewriting existing systems.**

This is a **hard architectural invariant**, not an aspiration.

---

## 2. What This Means in Practice

All implementations MUST comply with the following rules:

### 2.1 No Closed Systems

- ❌ No feature may be implemented as a "final form"
- ❌ No system may assume it is complete
- ❌ No module may hard-code business logic that cannot be extended, overridden, or composed

**If something cannot be extended safely, it is considered incorrectly implemented.**

### 2.2 Everything Is a Primitive or a Composition of Primitives

All functionality must fall into one of two categories:

**1. Platform Primitives**
- Auth, AI, Billing, Affiliates, Notifications, Storage, Events, Workflows, Identity, Permissions, PWA, Offline Sync

**2. Composed Systems**
- Built on top of primitives, never inside them

**No feature may bypass primitives or re-implement them locally.**

### 2.3 Plug-In First, Not Feature First

Every system must be designed as if:
- A future team
- A future partner
- A future client
- Or a future AI agent

…will want to:
- Extend it
- Replace part of it
- Hook into it
- Automate it
- Monetize it
- White-label it

**If this is not possible without code breakage, the design is invalid.**

---

## 3. Mandatory Architectural Patterns

All systems MUST use these patterns by default:

### 3.1 Event-Driven Architecture

- **Every meaningful action emits events**
- **New systems plug in by subscribing, not modifying core code**

**Example:**
```
Order Placed → Event Emitted → Multiple Subscribers
                                    │
                    ┌───────────────┼───────────────┐
                    │               │               │
              Inventory      Email Service    Analytics
              Decrement      Send Receipt     Track Sale
```

**Anti-Pattern (NOT WebWaka):**
```
Order Placed → Hardcoded Logic
                    │
        ┌───────────┼───────────┐
        │           │           │
    Inventory   Email      Analytics
    (in code)   (in code)  (in code)
```

### 3.2 Contract-First Interfaces

- **APIs, events, schemas, and messages are versioned and stable**
- **Backward compatibility is mandatory**

**Example:**
```json
// Event: order.placed (v1)
{
  "event_type": "order.placed",
  "version": "1.0",
  "data": {
    "order_id": "uuid",
    "tenant_id": "uuid",
    "total": 100.00
  }
}

// Event: order.placed (v2) - backward compatible
{
  "event_type": "order.placed",
  "version": "2.0",
  "data": {
    "order_id": "uuid",
    "tenant_id": "uuid",
    "total": 100.00,
    "currency": "NGN",  // NEW FIELD (optional)
    "items": []          // NEW FIELD (optional)
  }
}
```

### 3.3 Loose Coupling, Strong Contracts

- **Systems know what to expect, not how others work internally**

**Example:**
- **Billing Domain** knows that **Affiliate Domain** will emit `affiliate.commission.earned` events
- **Billing Domain** does NOT know how **Affiliate Domain** calculates commissions internally

### 3.4 Capability-Based Design

- **Features expose capabilities, not assumptions**
- **Permissions, roles, AI, workflows, and pricing attach to capabilities**

**Example:**
```json
// Capability: create_invoice
{
  "capability": "create_invoice",
  "permissions": ["billing.invoices.create"],
  "ai_enabled": true,
  "workflow_enabled": true,
  "pricing_model": "per_invoice"
}
```

---

## 4. Recursive Extensibility (Platform Within Platform)

This rule applies at every level:
- **Super Admin** → Platform tools for Partners
- **Partners** → Platform tools for Clients
- **Clients** → Platform tools for their own users

Therefore:

**Any extensibility WebWaka uses internally MUST be available downstream.**

### 4.1 Recursive Extensibility Examples

| WebWaka Uses | Partners Can Use | Clients Can Use |
|--------------|------------------|-----------------|
| Workflow builder | Workflow builder for client automation | Workflow builder for end-user automation |
| AI agent configuration | AI agent configuration for clients | AI agent configuration for end-users |
| Affiliate rule configuration | Affiliate rule configuration for clients | Affiliate rule configuration for sub-affiliates |
| Offline behavior configuration | Offline behavior configuration for clients | Offline behavior configuration for end-users |

### 4.2 No "Internal-Only" Shortcuts

**Anti-Pattern (NOT WebWaka):**
- WebWaka uses a "super admin workflow builder" that is NOT available to partners

**WebWaka Pattern (Correct):**
- WebWaka uses the SAME workflow builder that partners use
- WebWaka's workflows are just "super admin-scoped" workflows
- Partners can create "partner-scoped" workflows
- Clients can create "client-scoped" workflows

---

## 5. Future Systems This Must Support (Non-Exhaustive)

This extensibility guarantee exists specifically to ensure that future unknown systems can be added, including but not limited to:

- **New industry-specific suites** (e.g., Real Estate, Legal, Construction)
- **New monetization models** (e.g., usage-based, freemium, enterprise)
- **New AI agents and tools** (e.g., voice agents, image generation)
- **New communication channels** (e.g., Telegram, Discord, Slack)
- **New compliance layers** (e.g., HIPAA, SOC 2, ISO 27001)
- **New partner business models** (e.g., white-label, reseller, franchise)
- **New geographies and regulations** (e.g., EU, US, Asia)
- **New device classes** (e.g., wearables, kiosks, POS hardware, IoT)
- **New delivery models** (e.g., native apps, voice, agents)

**If today's design blocks any of these, it is wrong by definition.**

---

## 6. Enforcement Rules

### 6.1 Prohibited Patterns

| Pattern | Why It's Prohibited |
|---------|---------------------|
| **Hardcoded business logic** | Cannot be extended without code changes |
| **Direct module dependencies** | Cannot replace modules without breaking others |
| **Special-case logic in dashboards** | Should be in primitives, not UI |
| **Non-versioned APIs** | Cannot evolve without breaking clients |
| **Non-event-driven actions** | Cannot be extended by new systems |

### 6.2 Required Patterns

| Pattern | Why It's Required |
|---------|-------------------|
| **Event-driven architecture** | New systems plug in by subscribing |
| **Contract-first interfaces** | APIs evolve without breaking clients |
| **Loose coupling, strong contracts** | Systems can be replaced independently |
| **Capability-based design** | Features can be extended via capabilities |

### 6.3 Violation Consequences

| Violation | Consequence |
|-----------|-------------|
| **System requires refactoring to add new functionality** | Revert immediately, redesign as extensible |
| **Module directly depends on internal logic of another module** | Revert immediately, use events or contracts |
| **Dashboard implements "special logic" that primitives already cover** | Revert immediately, use primitives |
| **Non-versioned API breaks clients** | Revert immediately, add versioning |

---

## 7. Extensibility Mechanisms

All extensibility MUST happen through:

### 7.1 Events

**Definition:** Systems emit events when meaningful actions occur. Other systems subscribe to events.

**Example:**
```
Order Placed → Event: order.placed → Subscribers:
                                        - Inventory (decrement stock)
                                        - Email (send receipt)
                                        - Analytics (track sale)
                                        - AI (generate recommendations)
```

### 7.2 Hooks

**Definition:** Systems expose hooks where custom logic can be injected.

**Example:**
```
Before Invoice Created → Hook: before_invoice_create → Custom Logic:
                                                          - Validate data
                                                          - Apply discount
                                                          - Add custom fields
```

### 7.3 Policies

**Definition:** Systems expose policies that can be configured per tenant, partner, or user.

**Example:**
```
Affiliate Commission Policy → Configurable:
                                - Fixed percentage
                                - Tiered percentage
                                - Custom formula
```

### 7.4 Config

**Definition:** Systems expose configuration options that can be changed without code changes.

**Example:**
```
Offline Sync Policy → Configurable:
                        - Sync interval
                        - Conflict resolution strategy
                        - Max queue size
```

### 7.5 Plug-ins

**Definition:** Systems expose plug-in interfaces where new functionality can be added.

**Example:**
```
Payment Gateway Plug-in → Interface:
                            - authorize_payment()
                            - capture_payment()
                            - refund_payment()
```

### 7.6 Versioned Contracts

**Definition:** Systems expose versioned APIs, events, and schemas that evolve without breaking clients.

**Example:**
```
API: /api/v1/orders → Version 1
API: /api/v2/orders → Version 2 (backward compatible)
```

---

## 8. Composability Patterns

### 8.1 Primitive Composition

**Definition:** Complex systems are built by composing primitives, not by creating monolithic features.

**Example: POS System**
```
POS System = Auth + Billing + Inventory + Offline Sync + PWA + AI
```

**NOT:**
```
POS System = Custom monolithic code
```

### 8.2 Suite Composition

**Definition:** Industry-specific suites are built by composing primitives and modules.

**Example: Commerce Suite**
```
Commerce Suite = POS + Site Builder + CRM + Affiliate + Analytics
```

### 8.3 Recursive Composition

**Definition:** Systems can be composed recursively at all levels.

**Example:**
- **Super Admin** composes primitives to create Partner Portal
- **Partner** composes primitives to create Client Dashboard
- **Client** composes primitives to create End-User App

---

## 9. Future-Proofing Checklist

Before implementing any system, ask:

### 9.1 Extensibility Questions

- ❓ Can this system be extended without code changes?
- ❓ Can this system be replaced without breaking others?
- ❓ Can this system be composed with other systems?
- ❓ Can this system be used recursively at all levels?

### 9.2 Event-Driven Questions

- ❓ Does this system emit events for all meaningful actions?
- ❓ Can new systems subscribe to these events?
- ❓ Are events versioned and backward compatible?

### 9.3 Contract Questions

- ❓ Are APIs versioned?
- ❓ Are schemas versioned?
- ❓ Are events versioned?
- ❓ Is backward compatibility guaranteed?

### 9.4 Primitive Questions

- ❓ Is this system built on primitives, or does it bypass them?
- ❓ Does this system re-implement logic that primitives already provide?
- ❓ Can this system be decomposed into primitives?

### 9.5 Recursive Questions

- ❓ Can partners use this system for their clients?
- ❓ Can clients use this system for their users?
- ❓ Is this system available at all hierarchy levels?

**If the answer to any of these questions is "No", the design is invalid.**

---

## 10. Canonical Outcome

When implemented correctly:

- **WebWaka can evolve for 10–20 years**
- **New ideas plug in instead of breaking things**
- **Scale increases without fragility**
- **AI, partners, and clients can innovate independently**
- **The platform compounds instead of decaying**

**This is how WebWaka avoids becoming "legacy software".**

---

## 11. Governance Rules

### 11.1 Enforcement

| Rule | Description |
|------|-------------|
| **All systems must be extensible** | No closed systems, no final forms |
| **All systems must be event-driven** | No hardcoded logic, only event subscribers |
| **All systems must be versioned** | No breaking changes, only backward-compatible evolution |
| **All systems must be composable** | No monolithic features, only primitive compositions |
| **All systems must be recursive** | No internal-only shortcuts, only recursive patterns |

### 11.2 Review Process

Before merging any code, reviewers MUST ask:
1. ❓ Is this system extensible?
2. ❓ Is this system event-driven?
3. ❓ Is this system versioned?
4. ❓ Is this system composable?
5. ❓ Is this system recursive?

**If the answer to any of these questions is "No", the code MUST be rejected.**

### 11.3 Violations

| Violation | Consequence |
|-----------|-------------|
| **Closed system** | Revert immediately, redesign as extensible |
| **Hardcoded logic** | Revert immediately, use events or config |
| **Non-versioned API** | Revert immediately, add versioning |
| **Monolithic feature** | Revert immediately, decompose into primitives |
| **Internal-only shortcut** | Revert immediately, make recursive |

---

## 12. How to Use This Going Forward

### 12.1 In Execution Prompts

- **This section must be referenced in every execution prompt**
- **Any proposal must explicitly state how it preserves this invariant**
- **Any design that violates this must be rejected, regardless of speed or convenience**

### 12.2 In Code Reviews

- **Reviewers must check for extensibility, events, versioning, composability, and recursion**
- **Code that violates these principles must be rejected**

### 12.3 In Architecture Decisions

- **All architecture decisions must preserve extensibility**
- **No shortcuts that block future extensibility**

---

**End of Platform Extensibility, Composability & Future-Proofing Section**
