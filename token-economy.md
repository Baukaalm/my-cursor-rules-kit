---
description: Token economy — minimize context usage, efficient tool use
alwaysApply: true
---

# Token Economy

- **Read before edit** — always read the file (or relevant section) before modifying
- **Use `explore` subagent** for broad codebase questions; don't grep manually across many files
- **Use `fast` model** for subagents on straightforward/scoped tasks (renames, simple edits, searches)
- **Batch parallel tool calls** — if calls are independent, send them in one message
- **Prefer glob-scoped rules** (`alwaysApply: false` + `globs`) over `alwaysApply: true` to reduce per-prompt token cost
- **Leverage `project-architecture.md`** — don't re-explore the monorepo structure, it's documented there
- **Targeted reads** — use line offsets for large files instead of reading the entire file
- **Don't over-search** — if you know the file path, read it directly; use Grep for exact symbols, SemanticSearch for conceptual questions
