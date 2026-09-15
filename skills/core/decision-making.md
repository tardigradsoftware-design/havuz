# Deterministic Decision Making

## Purpose
Establishes a rigorous, deterministic framework for AI coding agents to make architectural, stylistic, and engineering choices without stalling on trivial questions or falling back to arbitrary hallucinations.

## When To Use
- When selecting technologies, libraries, UI paradigms, data flows, or file structures.
- When resolving conflicts between competing requirements (e.g., visual aesthetics vs. performance budgets).
- When determining whether to build a custom solution or leverage an existing component library.

## When Not To Use
- When explicit, unambiguous instructions are already provided by the project specification or human user.
- When existing codebase conventions strictly dictate the pattern (follow local conventions first).

## Core Principles
1. **Toolchain Enforceability**: Prefer decisions that can be enforced mechanically via compilers, linters, and type checkers over prose conventions.
2. **Defensive Simplicity**: The simplest solution that satisfies all non-functional requirements (security, accessibility, performance) is always preferred.
3. **Reversible Decisions**: Favor modular, decoupled abstractions that can be easily refactored over deep, opinionated vendor lock-in.

## Rules
- **Rule 1 (The Question Threshold)**: Do not ask the user questions regarding technical implementation details (e.g., choice of CSS utility class, directory naming, helper function signature). Make the standard industry decision and document the rationale in your summary.
- **Rule 2 (The Escalation Boundary)**: Only halt execution to ask user input when:
  1. A required third-party API secret/key is missing.
  2. Mutually exclusive business requirements conflict (e.g., "Build a public blog that requires mandatory paid login before reading any article").
  3. Irreversible destructive actions are imminent (e.g., wiping a production database table).
- **Rule 3 (The Component Precedence)**: Before creating any UI component, check `components/ui` or the design system registry. Only create a new custom component if composition cannot satisfy the design.
- **Rule 4 (Web Standards First)**: When choosing between a framework-specific API and a modern web standard (e.g., `fetch` vs `axios`, `URLSearchParams` vs `query-string`), choose the web standard.

## Decision Criteria
```text
IF task requires data validation:
  Use Zod schemas with TypeScript inference.
ELSE IF task requires styling:
  Use Tailwind CSS utility classes; avoid inline styles and runtime CSS-in-JS.
ELSE IF task requires accessible complex interactive element (Modal, Accordion, Dropdown):
  Use Radix UI Primitives / shadcn/ui; DO NOT hand-roll unaccessible div trees.
ELSE IF state is server-derived:
  Use React Server Components or TanStack Query; DO NOT store server data in global Redux/Zustand.
```

## Recommended Workflow
1. Parse user requirement into functional constraints and non-functional requirements (NFRs).
2. Evaluate existing project conventions (`package.json`, tsconfig, existing component directories).
3. Apply the Decision Criteria ladder to select optimal approach.
4. Execute implementation strictly adhering to selected pattern.
5. Record architectural decision rationale in task summary.

## Best Practices
- Choose Boring Technology: Select mature, widely supported primitives over experimental alphas.
- Colocate state as close as possible to where it is consumed.
- Document "Why", never describe the self-evident "What".

## Anti-Patterns
- **Analysis Paralysis**: Pausing a task repeatedly to query the user about routine architectural decisions.
- **Novelty Bias**: Choosing a bleeding-edge library when a built-in standard or mature library already solves the problem.
- **Premature Generalization**: Building an elaborate generic abstraction for a single, isolated use case.

## Validation Checklist
- [ ] Was the decision made without stalling on non-blocking trivialities?
- [ ] Is the chosen pattern compatible with TypeScript strict mode?
- [ ] Does the pattern avoid adding unnecessary third-party bundle weight?
- [ ] Is the decision reversible without a complete rewrite of adjacent domains?

## Related Skills
- `skills/core/planning.md`
- `skills/core/verification.md`
- `skills/architecture/maintainability.md`

## References
- ADR (Architecture Decision Records) Standard — https://adr.github.io/
- The Twelve-Factor App — https://12factor.net/

## Last Reviewed
2026-09-15
