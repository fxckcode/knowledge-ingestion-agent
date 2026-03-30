# Knowledge Structure Reference

Conventions for `.claude/knowledge/` — the project's institutional memory.

---

## Directory Overview

```
.claude/knowledge/
├── critical-constraints.md     — Non-negotiable rules (~200 tokens)
├── tech-stack.md               — Technologies, versions, key commands (~300–700 tokens)
├── folder-structure.md         — Project layout and naming conventions (~500 tokens)
├── architecture-overview.md    — High-level architecture diagram and narrative (~300 tokens)
├── architecture-patterns.md    — Module/service/DTO patterns in use (~500 tokens)
├── development-workflow.md     — Branching, commits, CI/CD, ticketing (~400 tokens)
├── testing-guidelines.md       — Coverage targets, patterns, fixtures (~300 tokens)
├── business-rules.md           — Domain rules (create when needed)
├── api-contracts.md            — API design decisions (create when needed)
└── {topic}.md                  — Additional files as the project grows
```

---

## File Naming

- Use kebab-case: `business-rules.md`, `api-contracts.md`
- Name by topic, not by date or version
- Prefer specific names over generic ones: `payment-processing.md` > `misc.md`
- Avoid numbered suffixes: create a more specific name instead of `business-rules-2.md`

---

## Token Budget Guidelines

Token budgets keep knowledge files fast to load and focused in scope.

| File | Target | Hard limit |
|---|---|---|
| `critical-constraints.md` | ~200 tokens | 300 tokens |
| `tech-stack.md` | ~300–700 tokens | 1000 tokens |
| `folder-structure.md` | ~500 tokens | 700 tokens |
| `architecture-overview.md` | ~300 tokens | 500 tokens |
| `architecture-patterns.md` | ~500 tokens | 700 tokens |
| `development-workflow.md` | ~400 tokens | 600 tokens |
| `testing-guidelines.md` | ~300 tokens | 500 tokens |
| Topic-specific files | ~300–500 tokens | 700 tokens |

When a file approaches its hard limit, split it by topic rather than expanding it.

---

## Content Standards

### Structure

Every knowledge file should follow this pattern:

```markdown
# {Title}

<!-- Updated: YYYY-MM-DD -->

One-sentence description of what this file covers.

---

## {Section Header}

Content here.

## {Section Header}

Content here.
```

### Style rules

- **Headers** for every logical section — make content Grep-able
- **Bullet points** for lists — no prose walls
- **Tables** for comparisons and mappings
- **RFC 2119 language** for constraints: MUST, SHOULD, MUST NOT, SHOULD NOT
- **Code blocks** for commands, file paths, and code snippets
- **No duplication** — if it's in CLAUDE.md, don't repeat it here

### What belongs in each file

| File | Include | Exclude |
|---|---|---|
| `critical-constraints.md` | Rules that cause bugs or violations if broken | Preferences, opinions |
| `tech-stack.md` | Versions, install commands, key CLI tools | Business logic |
| `architecture-patterns.md` | Patterns that are actually in use | Patterns under consideration |
| `business-rules.md` | Domain rules backed by product decisions | Implementation details |
| `development-workflow.md` | Commands, branch names, CI steps | Business logic |

---

## Searchability

Other agents find content via Grep. Write section headers that are predictable:

```markdown
## Authentication Flow        ← agents can grep for this
## Payment Rules              ← agents can grep for this
## Database Conventions       ← agents can grep for this
```

Avoid vague headers like `## Overview`, `## Notes`, `## Other`.

---

## Self-contained files

Each file must make sense without reading any other knowledge file. Assume the reader only loads one file at a time. Cross-reference by file path when context from another file is needed:

```markdown
See `critical-constraints.md ## Authentication Rules` for enforcement details.
```

---

## Conflict detection

Before writing to any knowledge file, the agent:

1. Reads the full existing file
2. Checks for contradictions with the new content
3. If a conflict exists, surfaces it to the user before writing
4. Documents superseded rules inline: `~~Old rule~~ → Superseded by [new rule] on YYYY-MM-DD`

---

## CLAUDE.md Documentation Map

Every knowledge file must be registered in CLAUDE.md. The agent maintains this table:

```markdown
## Documentation Map

| File | Purpose | Token budget |
|---|---|---|
| `.claude/knowledge/critical-constraints.md` | Non-negotiable rules | ~200 tokens |
| `.claude/knowledge/tech-stack.md` | Technologies and commands | ~500 tokens |
| `.claude/knowledge/architecture-patterns.md` | Module/service patterns | ~500 tokens |
```

When the agent creates a new knowledge file, it adds a row to this table.
