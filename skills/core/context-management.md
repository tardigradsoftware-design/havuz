# Agent Context Management & Token Optimization

## Purpose
Enforces disciplined context window management for AI coding agents to prevent token exhaustion, attention degradation, instruction drift, and high inference costs.

## When To Use
- Continuously throughout all agent sessions.
- When ingesting documentation, guidelines, or large codebases.
- When planning multi-file refactors or complex system designs.

## When Not To Use
- When debugging a localized, self-contained single-file utility where context limits are not at risk.

## Core Principles
1. **Minimal Sufficient Context**: Load only the specific information necessary to solve the current problem at hand.
2. **Just-In-Time Ingestion**: Fetch domain-specific references (e.g., Stripe API, WCAG tables) only at the exact lifecycle stage they are needed.
3. **Summarization Over Ingestion**: Distill large documentation pages into actionable constraints rather than pasting raw markdown files into agent context.

## Rules
- **Rule 1 (The 8-Skill Cap)**: Never load more than 8 skill files into active working memory simultaneously. Follow the Archetype Matrix in `MASTER.md`.
- **Rule 2 (No Full-Repo Dumps)**: Never read or load entire directories indiscriminately. Target exact file paths using `grep`, `find`, or AST symbols.
- **Rule 3 (Lifecycle Stage Scoping)**:
  - During Planning: Load only `skills/core/*` and architecture guides.
  - During Implementation: Drop planning guides; load UI/Frontend/Backend skills.
  - During Auditing: Drop implementation guides; load `checklists/*` and testing skills.
- **Rule 4 (No Redundant Re-Reading)**: If a file has already been read in the conversation turn and has not been modified, use cached understanding rather than reading it again.

## Decision Criteria
```text
IF file is > 500 lines:
  Read targeted line ranges or outline class/function signatures before full read.
IF task is purely frontend styling:
  DO NOT load backend, database, or API skills.
IF task is purely technical SEO:
  DO NOT load backend ORM, auth security, or SaaS multi-tenancy skills.
```

## Recommended Workflow
1. Inspect the incoming request and identify the specific domain.
2. Query `MASTER.md` to identify the designated 4–8 skill files.
3. Read only those specific files.
4. Execute the targeted edits.
5. If task transitions to verification, unload obsolete skill context and load relevant checklist.

## Best Practices
- Keep instructions compact, imperative, and structured with bullet points.
- Use precise symbol searches (`grep -rn "export function useAuth" src/`) instead of reading entire directories.
- Clean up intermediate scratchpad notes once decisions are locked into actual code files.

## Anti-Patterns
- **Context Flooding**: Loading dozens of markdown files "just in case", pushing critical system prompt instructions out of active attention.
- **Instruction Amnesia**: Allowing early constraints (e.g., "Must be WCAG AA compliant") to be forgotten due to token overflow.
- **Repeating Entire Files**: Generating unchanged 400-line files when an isolated 10-line patch was required.

## Validation Checklist
- [ ] Are fewer than 8 skill files loaded for this task?
- [ ] Were large files read selectively rather than dumped entirely?
- [ ] Has context noise been eliminated prior to major reasoning steps?
- [ ] Are all active instructions directly pertinent to the immediate milestone?

## Related Skills
- `skills/core/decision-making.md`
- `skills/core/planning.md`
- `MASTER.md`

## References
- Anthropic Context Window Best Practices
- LLM Needle in a Haystack & Attention Degradation Benchmarks

## Last Reviewed
2026-09-15
