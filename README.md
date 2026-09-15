# AI Web Development Master Knowledge Base

> **The Agent-Agnostic Operating System for Modern Web Development**
> Designed for autonomous coding agents (Arena, Claude Code, Cursor, Codex, Windsurf, Devin, Gemini CLI) and human software architects.

[![Status](https://img.shields.io/badge/status-active-brightgreen.svg)]()
[![Standard](https://img.shields.io/badge/standard-AGENTS.md%20compatible-blue.svg)]()
[![License](https://img.shields.io/badge/license-MIT-purple.svg)]()
[![Coverage](https://img.shields.io/badge/disciplines-11%20domains-orange.svg)]()

---

## 🎯 Purpose & Philosophy

This repository is **not** a traditional documentation wiki or a beginner tutorial. It is a **deterministic, machine-executable knowledge base and operating system** engineered specifically to guide AI coding agents through high-grade commercial web development.

When an AI agent is pointed to this repository, it does not hallucinate arbitrary architectures or generate disjointed "AI slop". Instead, it follows a structured, stage-gated lifecycle:
1. **Classifies Project Type** (Corporate Web, SaaS, E-Commerce, Data Dashboard, High-Conversion Landing Page).
2. **Loads Only Relevant Skills** (optimizing LLM context windows and token expenditure).
3. **Applies Strict Engineering Principles** (deterministic rules, typed contracts, verified accessibility standards, zero-trust security).
4. **Validates & Self-Critiques** (running automated test matrices, visual QA audits, performance benchmarks, and security checks before completion).

---

## 🧱 Repository Architecture

```text
AI-Web-Master-Knowledge-Base/
├── README.md                     # Overview, architecture & quick start
├── AGENTS.md                     # Universal agent bootloader & contract
├── MASTER.md                     # Orchestration kernel: decision trees & lifecycle
├── SOURCES.md                    # Audited source registry (Tier 1/2/3 & Rejected)
├── CHANGELOG.md                  # Versioning, evolution, and audit history
│
├── skills/                       # Deterministic capability modules (Actionable rules)
│   ├── core/                     # Agent cognition (Planning, critique, verification, context)
│   ├── ui/                       # Visual design, typography, spacing, responsive, a11y
│   ├── frontend/                 # React 19, Next.js App Router, TypeScript strict, Tailwind
│   ├── backend/                  # REST/tRPC, Database/ORMs, AuthN/AuthZ, Error domains
│   ├── seo/                      # Technical SEO, JSON-LD Schema, Core Web Vitals, canonicals
│   ├── performance/              # LCP/INP/CLS budgets, asset pipelines, caching topologies
│   ├── security/                 # OWASP Top 10, CSP, CSRF, auth boundaries, secret sanitization
│   ├── testing/                  # Playwright E2E, Vitest, component isolation, visual diffs
│   ├── architecture/             # Component hierarchies, multi-tier decoupling, scalability
│   ├── ecommerce/                # Cart engines, checkout flows, PDP/PLP conversion, Stripe
│   └── saas/                     # Multi-tenancy, RBAC/PBAC, onboarding, billing webhooks
│
├── references/                   # Curated standards, external benchmarks & repo mappings
│   ├── ai-agents/                # Prompt conventions, tool-use specifications
│   ├── frontend/                 # Modern stack blueprints (Next.js, React Server Components)
│   ├── ui/                       # Visual hierarchy & interaction design standards
│   ├── design-systems/           # shadcn/ui, Radix Primitives, Lucide specifications
│   ├── accessibility/            # WCAG 2.2 AA checklists & ARIA pattern index
│   ├── seo/                      # Google Search Essentials & Schema.org blueprints
│   ├── performance/              # Web Vitals metrics, network budgets & hydration guides
│   ├── security/                 # OWASP application security verification standards (ASVS)
│   ├── testing/                  # Playwright best-practice fixtures & axe-core rules
│   ├── ecommerce/                # Baymard Institute conversion benchmarks & funnel designs
│   └── saas/                     # B2B SaaS layout conventions, tenant isolation strategies
│
├── checklists/                   # Stage-gated verification protocols
│   ├── pre-development.md        # Requirement clarification & stack selection
│   ├── architecture-review.md    # Boundary separation, schema validation, data flow
│   ├── ui-review.md              # Contrast, typography scale, 4/8pt alignment, micro-copy
│   ├── responsive-review.md      # Mobile (375px), Tablet (768px), Desktop (1280px, 1440px)
│   ├── seo-audit.md              # Meta, Schema JSON-LD, sitemap, robots, OpenGraph
│   ├── performance-audit.md      # LCP (<2.5s), INP (<200ms), CLS (<0.1), bundle size
│   ├── security-audit.md         # Injection, CSRF, XSS, auth headers, rate limits
│   ├── accessibility-audit.md    # Keyboard navigation, ARIA roles, focus visible, screen readers
│   ├── testing-audit.md          # Unit, integration, E2E journey, and failure recovery tests
│   └── final-release.md          # Production sign-off verification checklist
│
└── templates/                    # Production-ready scaffolding templates
    ├── website-project.md        # Corporate / brand presence specification
    ├── saas-project.md           # Multi-tenant B2B subscription platform specification
    ├── ecommerce-project.md      # High-performance commerce platform specification
    ├── dashboard-project.md      # High-density data analytics workspace specification
    └── landing-page.md           # High-converting lead generation page specification
```

---

## ⚡ Agent Quick Start Guide

### How Any AI Agent Should Use This Knowledge Base:

1. **Read `AGENTS.md` First**: This establishes your operating constraints, instruction parsing hierarchy, and non-negotiable rules.
2. **Consult `MASTER.md`**: Match the incoming user prompt to the exact project profile (e.g. SaaS vs. Landing Page) to extract the **minimal sufficient skill set**.
3. **Do NOT Load Every Skill**: Ingest only the specific files declared in the project's profile matrix to conserve context window and focus reasoning.
4. **Follow the Standard Execution Pipeline**:
   $$	ext{RESEARCH} \longrightarrow 	ext{PLAN} \longrightarrow 	ext{ARCHITECTURE} \longrightarrow 	ext{BUILD} \longrightarrow 	ext{TEST} \longrightarrow 	ext{AUDIT} \longrightarrow 	ext{VERIFY}$$
5. **Execute Validation Checklists**: Run relevant checklists from `/checklists` prior to marking any milestone complete.

---

## 🛡️ Core Tenets & Standards

- **Agent-Agnostic**: Compatible with Anthropic Claude Code, OpenAI Operator / Codex, Cursor IDE, Windsurf, Gemini CLI, and custom LLM toolchains.
- **Strictly Deterministic**: Rules are formulated as actionable imperatives (`DO`, `DO NOT`, `IF/THEN`), eliminating vague aesthetic generalities.
- **Official-First Provenance**: Built on verified, canonical documentation from React, Vercel, Tailwind Labs, Radix, Microsoft Playwright, Google Web Vitals, and OWASP.
- **Zero Hallucination Tolerance**: Every design pattern and architectural boundary requires explicit structural rationale and verification steps.

---

## 📜 License & Governance

Distributed under the **MIT License**. Maintained under continuous integration standards to guarantee perpetual accuracy against modern web standards (React 19+, Next.js 15+, Tailwind v4+, WCAG 2.2).
