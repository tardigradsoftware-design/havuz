# Architecture & Data Flow Review Checklist

> **Stage**: Technical Architecture Verification  
> **Mandatory**: Must be verified before assembling views.

---

## 1. Structural Boundaries
- [ ] Code organized into feature-first modular domains (`src/features/*`).
- [ ] Presentational components decoupled from direct database or network queries.
- [ ] Maximum component size capped (< 200 lines).
- [ ] Zero circular dependencies between feature directories.

## 2. Data Flow & Contracts
- [ ] All external API inputs and form payloads validated via Zod schemas.
- [ ] TypeScript interfaces inferred directly from Zod schemas (`z.infer`).
- [ ] State appropriately partitioned across URL, Server, and Local UI tiers.
- [ ] Database queries protected against N+1 bottlenecks.

---
*Sign-off: Architect / Lead Agent*
