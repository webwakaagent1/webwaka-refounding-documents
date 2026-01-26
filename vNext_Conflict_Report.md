# CONFLICT REPORT: Prior Recommendations vs. Founder Directives

**Analysis Date:** 2026-01-26  
**Analyst:** Manus Re-Founding Phase (WRF-0 Authoritative)

---

## Purpose

This report identifies **all prior recommendations** from the original Blueprint that **conflict with the two Founder Non-Negotiable Directives**:

1. **AWS-First, Single-Bill Architecture**
2. **Design for Maximum Scale from Day One**

---

## Category 1: Tooling Conflicts (AWS-First Directive)

### Conflict 1.1: Clerk (Authentication)

**Original Recommendation:** KEEP Clerk (unless Founder has strong preference for alternative).

**Conflict:** Clerk is a third-party SaaS. AWS Cognito is the AWS-native alternative.

**Status:** ❌ **INVALID under AWS-First directive**

**Corrected Recommendation:** REPLACE with AWS Cognito.

---

### Conflict 1.2: Neon (Database)

**Original Recommendation:** KEEP PostgreSQL. RECONSIDER Neon as provider.

**Conflict:** Neon is a third-party SaaS. AWS RDS or AWS Aurora are the AWS-native alternatives.

**Status:** ❌ **INVALID under AWS-First directive**

**Corrected Recommendation:** REPLACE with AWS Aurora (PostgreSQL-compatible).

---

### Conflict 1.3: Fly.io (Backend Hosting)

**Original Recommendation:** KEEP Fly.io (unless Founder has strong preference for alternative).

**Conflict:** Fly.io is a third-party SaaS. AWS ECS or AWS Fargate are the AWS-native alternatives.

**Status:** ❌ **INVALID under AWS-First directive**

**Corrected Recommendation:** REPLACE with AWS Fargate.

---

### Conflict 1.4: Vercel (Frontend Hosting)

**Original Recommendation:** KEEP Vercel (unless Founder has strong preference for alternative).

**Conflict:** Vercel is a third-party SaaS. AWS Amplify or AWS S3 + CloudFront are the AWS-native alternatives.

**Status:** ❌ **INVALID under AWS-First directive**

**Corrected Recommendation:** REPLACE with AWS Amplify.

---

### Conflict 1.5: Resend (Email)

**Original Recommendation:** USE Resend (planned for email campaigns).

**Conflict:** Resend is a third-party SaaS. AWS SES is the AWS-native alternative.

**Status:** ❌ **INVALID under AWS-First directive**

**Corrected Recommendation:** USE AWS SES.

---

### Conflict 1.6: PostHog (Analytics)

**Original Recommendation:** USE PostHog (planned for product analytics).

**Conflict:** PostHog is a third-party SaaS. AWS CloudWatch + Athena are the AWS-native alternatives.

**Status:** ❌ **INVALID under AWS-First directive**

**Corrected Recommendation:** USE AWS CloudWatch + Athena.

---

### Conflict 1.7: Sentry (Error Tracking)

**Original Recommendation:** USE Sentry (planned for error tracking).

**Conflict:** Sentry is a third-party SaaS. AWS CloudWatch Logs + X-Ray are the AWS-native alternatives.

**Status:** ❌ **INVALID under AWS-First directive**

**Corrected Recommendation:** USE AWS CloudWatch + X-Ray.

---

## Category 2: Architecture Conflicts (Max-Scale-First Directive)

### Conflict 2.1: Tenant-Centric Architecture

**Original Recommendation:** Tenant-Centric architecture was presented as a valid option (Decision 3, Option A).

**Conflict:** Tenant-Centric architecture does not support **thousands of Partners** and **millions of Tenants**. Partner-Centric architecture is required.

**Status:** ❌ **INVALID under Max-Scale-First directive**

**Corrected Recommendation:** Partner-Centric architecture is the only valid option.

---

### Conflict 2.2: Vertical SaaS Model

**Original Recommendation:** Vertical SaaS model was presented as a valid option (Decision 1, Option A).

**Conflict:** Vertical SaaS model is not a **Platform for Building Platforms**. Meta-Platform model is required.

**Status:** ❌ **INVALID under Max-Scale-First directive**

**Corrected Recommendation:** Meta-Platform (Hybrid) is the only valid option.

---

### Conflict 2.3: Commerce-Only Scope

**Original Recommendation:** Commerce-Only scope was presented as a valid option (Decision 4, Option A).

**Conflict:** Commerce-Only scope does not support **industry-agnostic modules** and **composable primitives**. Multi-Industry scope is required.

**Status:** ❌ **INVALID under Max-Scale-First directive**

**Corrected Recommendation:** Multi-Industry scope (Composable Primitives + Phased Rollout) is the only valid option.

---

### Conflict 2.4: Super Admin-Only Tenant Provisioning

**Original Recommendation:** Super Admin-Only tenant provisioning was presented as a valid option (Decision 6, Option A).

**Conflict:** Super Admin-Only provisioning is a bottleneck and does not support **thousands of Partners** and **millions of Tenants**. Partner Self-Service provisioning is required.

**Status:** ❌ **INVALID under Max-Scale-First directive**

**Corrected Recommendation:** Partner Self-Service provisioning is the only valid option.

---

### Conflict 2.5: Single Domain (No Custom Domains)

**Original Recommendation:** Single Domain was presented as a valid option (Decision 7, Option A).

**Conflict:** Single Domain does not support **white-labeled SaaS resale**. Partner-Specific custom domains are required.

**Status:** ❌ **INVALID under Max-Scale-First directive**

**Corrected Recommendation:** Partner-Specific custom domains are the only valid option.

---

### Conflict 2.6: 24-Hour Offline-First

**Original Recommendation:** 24-Hour offline-first was presented as a valid option (Decision 9, Option A).

**Conflict:** 24-Hour offline is insufficient for **worst-case network conditions** (rural Nigeria, power outages). Indefinite offline is required.

**Status:** ⚠️ **RISKY under Max-Scale-First directive**

**Corrected Recommendation:** Indefinite offline is the recommended option.

---

### Conflict 2.7: Shared Dev/Prod Infrastructure

**Original Recommendation:** Shared dev/prod infrastructure was mentioned in the original Constitution.

**Conflict:** Shared dev/prod infrastructure has high blast-radius risk and does not support **enterprise-grade reliability**. Isolated dev/prod infrastructure is required.

**Status:** ❌ **INVALID under Max-Scale-First directive**

**Corrected Recommendation:** Isolated dev/prod infrastructure is the only valid option.

---

## Category 3: Governance Conflicts

### Conflict 3.1: Vibecoding Governance Model

**Original Recommendation:** The original Constitution defined a complex vibecoding governance model (Emergent-01, Lovable-01, Replit-01).

**Conflict:** This governance model was **not being followed in practice**. All recent execution prompts were executed by Manus.

**Status:** ❌ **DISCARDED** (not being followed in practice)

**Corrected Recommendation:** Manus-Only for Phase 1. Multi-Operator for Phase 2+ (if needed).

---

## Summary: Conflicts Identified

| Category | Conflict | Original Recommendation | Corrected Recommendation |
|----------|----------|--------------------------|--------------------------|
| **Tooling** | Clerk (Authentication) | KEEP Clerk | REPLACE with AWS Cognito |
| **Tooling** | Neon (Database) | KEEP Neon | REPLACE with AWS Aurora |
| **Tooling** | Fly.io (Backend Hosting) | KEEP Fly.io | REPLACE with AWS Fargate |
| **Tooling** | Vercel (Frontend Hosting) | KEEP Vercel | REPLACE with AWS Amplify |
| **Tooling** | Resend (Email) | USE Resend | USE AWS SES |
| **Tooling** | PostHog (Analytics) | USE PostHog | USE AWS CloudWatch + Athena |
| **Tooling** | Sentry (Error Tracking) | USE Sentry | USE AWS CloudWatch + X-Ray |
| **Architecture** | Tenant-Centric Architecture | Valid option | INVALID (Partner-Centric only) |
| **Architecture** | Vertical SaaS Model | Valid option | INVALID (Meta-Platform only) |
| **Architecture** | Commerce-Only Scope | Valid option | INVALID (Multi-Industry only) |
| **Architecture** | Super Admin-Only Provisioning | Valid option | INVALID (Partner Self-Service only) |
| **Architecture** | Single Domain | Valid option | INVALID (Partner-Specific only) |
| **Architecture** | 24-Hour Offline | Valid option | RISKY (Indefinite recommended) |
| **Architecture** | Shared Dev/Prod | Mentioned | INVALID (Isolated only) |
| **Governance** | Vibecoding Governance | Defined | DISCARDED (not being followed) |

---

## Conclusion

**15 conflicts identified.** All conflicts have been corrected in the updated Blueprint.

**The updated Blueprint is now fully aligned with the two Founder Non-Negotiable Directives:**

1. **AWS-First, Single-Bill Architecture**
2. **Design for Maximum Scale from Day One**

---

**End of Conflict Report**
