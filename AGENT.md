# AGENT.md — Design Philosophy

This document explains the design decisions behind the `knowledge-ingestion-agent` — what it is, why it exists, and how to think about it when adapting it to a new project.

---

## Why this agent exists

When multiple Claude agents work on a project, they each start with no shared memory. Without a structured knowledge layer, every agent operates on incomplete information: guessing at business rules, duplicating research, and making decisions that contradict each other.

The `knowledge-ingestion-agent` solves this by acting as the **single source of truth**. It is the only agent that writes to `.claude/knowledge/`. All other agents read from it.

This separation of concerns is intentional:

- **Implementation agents** (engineers, architects, test writers) read knowledge and act on it
- **The knowledge agent** captures decisions and keeps the record accurate

---

## Design principles

### 1. Ground everything in the actual codebase

The agent never guesses. Every claim is either drawn from a file it has read, or explicitly labeled as an assumption. This prevents hallucinated "facts" from polluting the knowledge base.

### 2. Append, never overwrite context

Session files at `.claude/tasks/` are append-only. This preserves the full audit trail of what was researched and decided, which is essential when debugging agent decisions later.

### 3. Token budgets per file

Each knowledge file has a target token count. Smaller files load faster and stay focused. When a file approaches its budget, create a new file rather than expanding the old one.

### 4. Own the knowledge layer completely

The agent is the sole writer to `.claude/knowledge/`. Other agents may read these files, but they never write to them directly. This prevents inconsistency.

### 5. Flag conflicts before acting

If new information contradicts existing knowledge, the agent stops and asks the user which takes precedence. It never silently overwrites a rule.

### 6. Distinguish facts from assumptions

Any claim that isn't directly grounded in a read file is labeled as an assumption. This keeps the knowledge base honest and makes it clear what needs verification.

---

## Two workflows, two responsibilities

### Workflow A — Context Reports

Before any significant implementation decision, an agent (or the user) can ask the knowledge agent to produce a context report. This report:

- Summarizes the current state of the relevant system
- Analyzes alignment with project constraints
- Lists confirmed facts and explicitly labeled assumptions
- Provides a recommendation with trade-off analysis

The report goes to `.claude/plans/` and is appended to the session context. It is **read-only input** for the agent requesting it — the implementation agent reads the report and proceeds, but does not modify it.

### Workflow B — Knowledge Ingestion

When the user shares something the project needs to remember — a business rule, an architecture decision, a naming convention — the knowledge agent:

1. Reads the relevant existing knowledge file
2. Checks for conflicts
3. Writes the new content in a structured format
4. Updates the CLAUDE.md Documentation Map if a new file was created
5. Confirms exactly what was stored

The key insight: **if it wasn't ingested, it doesn't exist for other agents**. The knowledge agent makes this explicit.

---

## What this agent does NOT do

- It does not write application code (controllers, services, models, views, etc.)
- It does not run shell commands
- It does not push to git
- It does not make implementation decisions — it informs them

The agent is a researcher and archivist, not an implementer.

---

## How to adapt this for a new project

1. Copy `agent/knowledge-ingestion-agent.md` into your project's `.claude/agents/` directory
2. Replace all `{PLACEHOLDER}` values with project-specific content
3. Define your project's non-negotiable constraints in the **Project Constraints** section
4. Set up your initial knowledge files in `.claude/knowledge/` (start with `critical-constraints.md` and `tech-stack.md`)
5. Add the knowledge base Documentation Map to your `CLAUDE.md`

See `references/customization-guide.md` for a detailed walkthrough.

---

## File ownership map

| Directory | Owner | Consumers |
|---|---|---|
| `.claude/knowledge/` | knowledge-ingestion-agent | All agents |
| `.claude/plans/` | knowledge-ingestion-agent | All agents, user |
| `.claude/tasks/` | All agents (append-only) | All agents, user |
| `src/` / `apps/` | Implementation agents | — |
