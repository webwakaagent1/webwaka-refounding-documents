# SECTION: AI & INTELLIGENCE PLATFORM CANON

**Status:** 🔒 CANONICALLY LOCKED — AI is a first-class platform primitive, equal to Auth, Billing, and Affiliates.

---

## 1. Foundational Principle

**AI is not a feature. AI is a platform primitive.**

Just as WebWaka has Auth, Billing, Affiliates, Notifications, and Storage as core primitives, **AI & Intelligence** is a first-class system that:

- Is available to all hierarchy levels (Super Admin → Partner → Client → End User)
- Integrates with Events, Workflows, Permissions, and Cost Attribution
- Degrades gracefully offline and syncs when online
- Is recursively usable (anything WebWaka uses internally must be available downstream)

---

## 2. Unified AI Orchestration Layer

**There is ONE unified AI orchestration layer, not separate bots.**

### 2.1 Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                    AI Orchestration Layer                    │
│  (Single unified system, role-scoped, multi-model support)  │
└─────────────────────────────────────────────────────────────┘
                              │
        ┌─────────────────────┼─────────────────────┐
        │                     │                     │
    ┌───▼────┐          ┌────▼─────┐         ┌────▼─────┐
    │  AWS   │          │  OpenAI  │         │  Custom  │
    │ Bedrock│          │   API    │         │  Models  │
    └────────┘          └──────────┘         └──────────┘
```

**Key Principles:**
- **One orchestration layer** that routes requests to appropriate models
- **Multi-model support** (AWS Bedrock preferred, but not exclusive)
- **Model selection** based on task type, cost, latency, and availability
- **Fallback strategies** when primary model is unavailable

### 2.2 Why Unified, Not Separate Bots

**Anti-Pattern (NOT WebWaka):**
- Separate "Customer Support Bot"
- Separate "Sales Bot"
- Separate "Analytics Bot"
- Each with its own logic, context, and permissions

**WebWaka Pattern (Correct):**
- **One AI Agent** with role-based behavior
- **Context-aware** (knows who is asking, what they can access)
- **Permission-scoped** (respects existing permission system)
- **Event-driven** (can be triggered by workflows, not just chat)

---

## 3. Role-Based AI Behavior

AI capabilities are **role-scoped**, not hardcoded.

### 3.1 AI Capabilities by Role

| Role | AI Capabilities | Example Use Cases |
|------|----------------|-------------------|
| **Super Admin** | Full platform intelligence | - Analyze partner performance<br>- Detect fraud patterns<br>- Optimize infrastructure<br>- Generate platform reports |
| **Partner** | Partner-scoped intelligence | - Analyze client performance<br>- Recommend pricing strategies<br>- Generate client reports<br>- Automate client onboarding |
| **Client (Tenant)** | Tenant-scoped intelligence | - Analyze sales data<br>- Generate marketing content<br>- Automate workflows<br>- Customer support |
| **End User** | User-scoped intelligence | - Product recommendations<br>- Order assistance<br>- FAQ answers<br>- Personalized content |

### 3.2 Permission Integration

AI **respects the existing permission system**. It cannot:
- Access data the user doesn't have permission to see
- Perform actions the user doesn't have permission to do
- Bypass row-level security or tenant isolation

**Example:**
- A Partner AI agent can analyze data for **their clients only**, not all clients on the platform
- A Client AI agent can analyze data for **their tenant only**, not other tenants

---

## 4. Multi-Model Orchestration Strategy

WebWaka supports **multiple AI models** and selects the best model for each task.

### 4.1 Model Selection Criteria

| Criterion | Description |
|-----------|-------------|
| **Task Type** | Text generation, code generation, image analysis, embeddings, etc. |
| **Cost** | Cheaper models for simple tasks, expensive models for complex tasks |
| **Latency** | Fast models for real-time chat, slower models for batch processing |
| **Availability** | Fallback to alternative model if primary is unavailable |
| **Quality** | Higher quality models for critical tasks (e.g., legal, financial) |

### 4.2 Supported Model Providers

| Provider | Models | Use Cases |
|----------|--------|-----------|
| **AWS Bedrock** | Claude, Llama, Titan, Jurassic | Primary provider (AWS-first preference) |
| **OpenAI** | GPT-4, GPT-4 Turbo, GPT-3.5 Turbo | Fallback for specific tasks |
| **Custom Models** | Fine-tuned models for WebWaka-specific tasks | Industry-specific intelligence |

### 4.3 Model Routing Logic

```
User Request → AI Orchestration Layer
                    │
                    ├─ Analyze task type
                    ├─ Check user permissions
                    ├─ Estimate cost
                    ├─ Select optimal model
                    ├─ Execute request
                    └─ Return response
```

---

## 5. Event-Driven AI Triggers

AI is **event-driven**, not just chat-driven.

### 5.1 AI Trigger Types

| Trigger Type | Description | Example |
|--------------|-------------|---------|
| **User-Initiated** | User asks a question in chat | "Show me sales for last month" |
| **Event-Driven** | AI responds to platform events | New lead captured → AI sends follow-up email |
| **Scheduled** | AI runs on a schedule | Daily sales report generated at 8 AM |
| **Workflow-Driven** | AI is part of a workflow | Order placed → AI generates invoice |
| **Threshold-Driven** | AI triggers when condition met | Inventory low → AI notifies manager |

### 5.2 Integration with Event System

AI integrates with the **Event System** (AWS EventBridge):

```
Platform Event → EventBridge → AI Orchestration Layer → AI Model → Action
```

**Example Workflow:**
1. **Event:** New lead captured (POS, Site Builder, or CRM)
2. **AI Trigger:** AI agent receives event
3. **AI Action:** Generate personalized follow-up email
4. **Result:** Email sent via Communication Domain (AWS SES)

---

## 6. AI Cost Attribution Per Tenant

AI usage is **cost-attributed** to the tenant that triggered it.

### 6.1 Cost Tracking

| Metric | Description |
|--------|-------------|
| **Tokens Used** | Number of input + output tokens |
| **Model Used** | Which model was used (affects cost) |
| **Tenant ID** | Which tenant triggered the request |
| **User ID** | Which user triggered the request |
| **Timestamp** | When the request was made |

### 6.2 Cost Allocation

- **Super Admin AI usage** → Attributed to platform overhead
- **Partner AI usage** → Attributed to partner account
- **Client AI usage** → Attributed to client (tenant) account
- **End User AI usage** → Attributed to tenant that owns the end user

### 6.3 Billing Integration

AI costs are included in the **Billing Domain**:
- Partners can see AI usage costs for their clients
- Clients can see AI usage costs for their users
- AI costs can be passed through to clients or absorbed by partners (configurable)

---

## 7. Guardrails, Safety, and Governance

AI must operate within **strict guardrails** to ensure safety, compliance, and quality.

### 7.1 Safety Guardrails

| Guardrail | Description |
|-----------|-------------|
| **Content Filtering** | Block harmful, offensive, or illegal content |
| **PII Protection** | Prevent AI from exposing sensitive personal data |
| **Hallucination Detection** | Flag responses that may be factually incorrect |
| **Rate Limiting** | Prevent abuse and runaway costs |
| **Audit Logging** | Log all AI requests and responses for compliance |

### 7.2 Governance Rules

| Rule | Description |
|------|-------------|
| **Permission-Scoped** | AI respects existing permission system |
| **Tenant-Isolated** | AI cannot access data from other tenants |
| **Cost-Capped** | AI usage can be capped per tenant to prevent runaway costs |
| **Audit-Ready** | All AI interactions are logged for compliance and debugging |

### 7.3 Compliance

AI must comply with:
- **GDPR** (data privacy)
- **CCPA** (California data privacy)
- **Nigerian Data Protection Regulation (NDPR)**
- **Industry-specific regulations** (e.g., healthcare, finance)

---

## 8. Offline-Aware AI Interaction Patterns

AI must **degrade gracefully offline** and sync when online.

### 8.1 Offline Behavior

| Scenario | Behavior |
|----------|----------|
| **User asks question offline** | Queue request, show "Will answer when online" message |
| **AI-generated content cached** | Show cached content (e.g., FAQ answers) |
| **AI workflow triggered offline** | Queue workflow, execute when online |
| **AI notification sent offline** | Queue notification, send when online |

### 8.2 Sync Strategy

When device comes back online:
1. **Sync queued AI requests** (in order)
2. **Execute queued workflows**
3. **Send queued notifications**
4. **Update cached AI content** (if stale)

### 8.3 Offline-First AI Use Cases

| Use Case | Offline Behavior |
|----------|------------------|
| **FAQ Chatbot** | Cached FAQ answers available offline |
| **Product Recommendations** | Cached recommendations available offline |
| **Order Assistance** | Queue order questions, answer when online |
| **Sales Report** | Cached last report available offline, refresh when online |

---

## 9. Single-Purpose AI Tools vs. General Agent

WebWaka supports **both** single-purpose AI tools and a general AI agent.

### 9.1 Single-Purpose AI Tools

**Definition:** AI tools that perform a specific task (e.g., "Generate Invoice", "Summarize Sales", "Write Email").

**Characteristics:**
- **Narrow scope** (one task only)
- **Predictable output** (always returns same format)
- **Fast execution** (optimized for one task)
- **Lower cost** (uses simpler models)

**Examples:**
- **Invoice Generator:** Generate invoice from order data
- **Sales Summarizer:** Summarize sales data for a period
- **Email Writer:** Generate follow-up email from lead data

### 9.2 General AI Agent

**Definition:** AI agent that can handle a wide range of tasks (e.g., "Answer any question", "Perform any action").

**Characteristics:**
- **Broad scope** (many tasks)
- **Flexible output** (adapts to user request)
- **Slower execution** (requires more reasoning)
- **Higher cost** (uses more powerful models)

**Examples:**
- **Customer Support Agent:** Answer any customer question
- **Sales Assistant:** Help with any sales-related task
- **Analytics Agent:** Answer any analytics question

### 9.3 When to Use Each

| Use Case | Recommended Approach |
|----------|---------------------|
| **Predictable, repetitive tasks** | Single-purpose AI tools |
| **Unpredictable, varied tasks** | General AI agent |
| **High-volume, low-cost tasks** | Single-purpose AI tools |
| **Low-volume, high-value tasks** | General AI agent |

---

## 10. AI Integration with Other Platform Primitives

AI integrates with **all other platform primitives**.

### 10.1 AI + Events

- AI can **listen to events** (e.g., "New lead captured")
- AI can **emit events** (e.g., "AI generated invoice")

### 10.2 AI + Workflows

- AI can be **part of a workflow** (e.g., "Generate email → Send email")
- AI can **trigger workflows** (e.g., "Low inventory detected → Notify manager")

### 10.3 AI + Permissions

- AI **respects permissions** (cannot access data user doesn't have permission to see)
- AI **actions are permission-scoped** (cannot perform actions user doesn't have permission to do)

### 10.4 AI + Billing

- AI usage is **cost-attributed** to tenant
- AI costs are **included in billing**

### 10.5 AI + Notifications

- AI can **send notifications** (e.g., "AI detected fraud → Send alert")
- AI can **respond to notifications** (e.g., "User clicked notification → AI answers question")

### 10.6 AI + Storage

- AI can **read from storage** (e.g., "Analyze sales data")
- AI can **write to storage** (e.g., "Save generated report")

### 10.7 AI + Offline Sync

- AI requests are **queued offline**
- AI responses are **synced when online**

---

## 11. Recursive AI Usage

**Any AI capability WebWaka uses internally must be available for partners and clients.**

### 11.1 Recursive AI Hierarchy

```
Super Admin AI
    │
    ├─ Can analyze partner performance
    ├─ Can detect fraud patterns
    ├─ Can optimize infrastructure
    │
    └─ Partners inherit AI capabilities
            │
            ├─ Can analyze client performance
            ├─ Can recommend pricing strategies
            ├─ Can generate client reports
            │
            └─ Clients inherit AI capabilities
                    │
                    ├─ Can analyze sales data
                    ├─ Can generate marketing content
                    ├─ Can automate workflows
                    │
                    └─ End Users inherit AI capabilities
                            │
                            ├─ Product recommendations
                            ├─ Order assistance
                            └─ FAQ answers
```

### 11.2 Recursive AI Examples

| WebWaka Uses | Partners Can Use | Clients Can Use |
|--------------|------------------|-----------------|
| AI to analyze partner performance | AI to analyze client performance | AI to analyze sales data |
| AI to generate platform reports | AI to generate client reports | AI to generate sales reports |
| AI to detect fraud patterns | AI to detect fraud in client accounts | AI to detect fraud in orders |
| AI to optimize infrastructure | AI to optimize client pricing | AI to optimize inventory |

---

## 12. AI Data Model

### 12.1 Core Tables

**ai_requests**
```sql
CREATE TABLE ai_requests (
    id UUID PRIMARY KEY,
    tenant_id UUID NOT NULL,  -- Which tenant triggered this request
    user_id UUID NOT NULL,    -- Which user triggered this request
    role VARCHAR(50) NOT NULL, -- super_admin, partner, client, end_user
    task_type VARCHAR(100) NOT NULL, -- text_generation, code_generation, etc.
    model_used VARCHAR(100) NOT NULL, -- claude-3, gpt-4, etc.
    tokens_input INT NOT NULL,
    tokens_output INT NOT NULL,
    cost_usd DECIMAL(10, 6) NOT NULL,
    latency_ms INT NOT NULL,
    status VARCHAR(50) NOT NULL, -- success, error, queued
    created_at TIMESTAMP NOT NULL,
    completed_at TIMESTAMP
);
```

**ai_conversations**
```sql
CREATE TABLE ai_conversations (
    id UUID PRIMARY KEY,
    tenant_id UUID NOT NULL,
    user_id UUID NOT NULL,
    title VARCHAR(255),
    created_at TIMESTAMP NOT NULL,
    updated_at TIMESTAMP NOT NULL
);
```

**ai_messages**
```sql
CREATE TABLE ai_messages (
    id UUID PRIMARY KEY,
    conversation_id UUID NOT NULL REFERENCES ai_conversations(id),
    role VARCHAR(50) NOT NULL, -- user, assistant, system
    content TEXT NOT NULL,
    tokens INT NOT NULL,
    created_at TIMESTAMP NOT NULL
);
```

**ai_tools**
```sql
CREATE TABLE ai_tools (
    id UUID PRIMARY KEY,
    name VARCHAR(255) NOT NULL,
    description TEXT NOT NULL,
    task_type VARCHAR(100) NOT NULL,
    model_preference VARCHAR(100),
    is_active BOOLEAN NOT NULL DEFAULT true,
    created_at TIMESTAMP NOT NULL
);
```

---

## 13. AI API Endpoints

### 13.1 Core AI Endpoints

| Endpoint | Method | Description |
|----------|--------|-------------|
| `/api/ai/chat` | POST | Send message to AI agent |
| `/api/ai/conversations` | GET | List user's conversations |
| `/api/ai/conversations/:id` | GET | Get conversation history |
| `/api/ai/tools` | GET | List available AI tools |
| `/api/ai/tools/:id/execute` | POST | Execute single-purpose AI tool |
| `/api/ai/usage` | GET | Get AI usage stats for tenant |
| `/api/ai/cost` | GET | Get AI cost breakdown for tenant |

### 13.2 Example Request: Chat

```json
POST /api/ai/chat
{
  "conversation_id": "uuid",
  "message": "Show me sales for last month",
  "context": {
    "tenant_id": "uuid",
    "user_id": "uuid",
    "role": "client"
  }
}
```

**Response:**
```json
{
  "message_id": "uuid",
  "response": "Your sales for last month were $45,230. This is a 12% increase from the previous month.",
  "tokens_used": 150,
  "cost_usd": 0.0045,
  "model_used": "claude-3-sonnet"
}
```

---

## 14. AI Build Order

AI is integrated into **Phase 1.1 (Core Infrastructure)** as a core platform primitive.

### 14.1 Phase 1.1: Core AI Infrastructure

**Deliverables:**
- ✅ AI Orchestration Layer (single unified system)
- ✅ Multi-model support (AWS Bedrock + OpenAI)
- ✅ Role-based AI behavior (Super Admin, Partner, Client, End User)
- ✅ Permission integration (AI respects existing permission system)
- ✅ Cost attribution (AI usage tracked per tenant)
- ✅ Event-driven triggers (AI responds to platform events)
- ✅ Offline-aware patterns (AI queues requests offline, syncs online)

### 14.2 Phase 1.2: AI Tools & Workflows

**Deliverables:**
- ✅ Single-purpose AI tools (Invoice Generator, Sales Summarizer, Email Writer)
- ✅ AI workflow integration (AI as part of workflows)
- ✅ AI notification integration (AI sends notifications)

### 14.3 Phase 2: Advanced AI Capabilities

**Deliverables:**
- ✅ Custom model fine-tuning (industry-specific intelligence)
- ✅ AI analytics (AI usage trends, cost optimization)
- ✅ AI governance dashboard (audit logs, compliance reports)

---

## 15. AI Governance Rules

### 15.1 Enforcement Rules

| Rule | Description |
|------|-------------|
| **AI is a primitive** | AI must be treated as a core primitive, not a feature |
| **One orchestration layer** | No separate bots, only one unified AI system |
| **Permission-scoped** | AI must respect existing permission system |
| **Cost-attributed** | AI usage must be tracked per tenant |
| **Offline-aware** | AI must degrade gracefully offline |
| **Recursive** | AI capabilities must be available downstream |

### 15.2 Violations

| Violation | Consequence |
|-----------|-------------|
| **Creating separate AI bots** | Revert immediately, consolidate into orchestration layer |
| **Bypassing permissions** | Revert immediately, add permission checks |
| **Not tracking costs** | Revert immediately, add cost attribution |
| **Not supporting offline** | Revert immediately, add offline queuing |

---

## 16. Canonical Outcome

When implemented correctly:
- **AI is a first-class platform primitive** (equal to Auth, Billing, Affiliates)
- **One unified AI orchestration layer** (not separate bots)
- **Role-based AI behavior** (Super Admin, Partner, Client, End User)
- **Multi-model support** (AWS Bedrock preferred)
- **Event-driven AI triggers** (AI responds to platform events)
- **Cost attribution** (AI usage tracked per tenant)
- **Offline-aware** (AI queues requests offline, syncs online)
- **Recursive** (AI capabilities available downstream)

---

**End of AI & Intelligence Platform Canon**
