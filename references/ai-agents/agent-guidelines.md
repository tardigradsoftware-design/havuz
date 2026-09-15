# AI Agent Guidelines & Execution Protocol Reference

> **Specification**: Universal Coding Agent Directive Set  
> **Target Models**: Claude 3.5/3.7 Sonnet, GPT-4o, Codex, Cursor Agent, Devin, Gemini 1.5/2.0 Pro

---

## 1. Context Protocol

When an AI agent engages with a codebase:
1. **Locate Canonical Instruction Files**: Check for `AGENTS.md`, `CLAUDE.md`, or `.cursorrules`.
2. **Never Read Entire Repositories**: Read files selectively. Keep prompt token overhead minimal to maximize reasoning capacity.
3. **Cache Invariant Context**: Never re-read a file that hasn't changed within the session.

---

## 2. Command Execution Safety Hierarchy

| Danger Tier | Operation | Agent Behavior |
| :--- | :--- | :--- |
| **Safe / Auto-Execute** | `npm test`, `git status`, `tsc --noEmit`, file reading | Execute immediately without prompting |
| **Modification / Controlled** | File edits, creating files, installing packages | Execute following plan; report modifications |
| **Destructive / Mandatory Confirm** | `git reset --hard`, `rm -rf`, database drop, pushing to production | Stop and obtain explicit human confirmation |

---

## 3. Communication Patterns

- **State Intent Before Edits**: Announce the targeted files and the objective before applying changes.
- **Evidence-Based Reporting**: Conclude tasks with concrete outputs (test pass counts, compiler status, diff summaries) rather than generic declarations.
- **No Apology Loops**: If an agent makes an error, do not write elaborate apologies. State the root cause concisely, apply the fix, and verify.
