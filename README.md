# knowledge-ingestion-agent

![Version](https://img.shields.io/badge/version-v1.0.0-blue)
![License](https://img.shields.io/badge/license-MIT-green)
![Model](https://img.shields.io/badge/model-claude--sonnet-purple)
![Last Commit](https://img.shields.io/github/last-commit/fxckcode/knowledge-ingestion-agent)

A reusable Claude agent that acts as the **authoritative knowledge base manager** for any project. It ingests business rules, architecture decisions, and conventions into `.claude/knowledge/`, and produces structured context reports for other agents to consume.

---

## What it does

- **Ingests knowledge** from conversations into structured `.claude/knowledge/` files
- **Creates context reports** at `.claude/plans/` when consulted about a topic
- **Manages institutional memory** so other agents always have accurate, up-to-date project context
- **Detects conflicts** between new information and existing knowledge before writing
- **Enforces token budgets** per file to keep knowledge efficient for agents

This agent does **not** write application code. It owns the knowledge layer only.

---

## Install

Installation is a copy-paste. Copy the agent file to your project's agents directory:

```bash
cp agent/knowledge-ingestion-agent.md /path/to/your-project/.claude/agents/knowledge-ingestion-agent.md
```

Or fetch it directly from your project root:

```bash
mkdir -p .claude/agents && curl -o .claude/agents/knowledge-ingestion-agent.md \
  https://raw.githubusercontent.com/fxckcode/knowledge-ingestion-agent/main/agent/knowledge-ingestion-agent.md
```

Then **customize** the placeholders. See [Customization](#customization) below.

---

## Project Structure

```
knowledge-ingestion-agent/
├── agent/
│   └── knowledge-ingestion-agent.md   ← install this in your project
├── references/
│   ├── customization-guide.md         ← how to adapt the agent for your stack
│   ├── knowledge-structure.md         ← .claude/knowledge/ directory conventions
│   ├── workflows.md                   ← Workflow A & B explained in depth
│   ├── output-formats.md              ← output format reference
│   └── context-report-template.md     ← full context report template
├── AGENT.md                           ← agent design philosophy
├── CHANGELOG.md
├── CONTRIBUTING.md
└── LICENSE
```

---

## Customization

The agent file uses `{PLACEHOLDER}` markers for project-specific content. After copying:

1. Replace `{PROJECT_NAME}` with your project's name and one-line description
2. Replace `{FRAMEWORK}` with your primary framework (e.g., NestJS, Django, Rails)
3. Replace the **Project Constraints** section with your non-negotiable rules
4. Adjust the **Knowledge Directory Structure** to match your project
5. Update the **Knowledge Domains** section with your actual domain areas

See [`references/customization-guide.md`](references/customization-guide.md) for a step-by-step walkthrough with stack-specific examples.

---

## Usage

Once installed, invoke the agent from Claude Code:

```
/agents knowledge-ingestion-agent
```

### Ingest knowledge

Tell the agent about a new business rule or architecture decision:

> "The payment service must never store raw card numbers — always tokenize via Stripe before persisting."

The agent will classify it, select the right knowledge file, check for conflicts, write the content, and confirm exactly what was stored and where.

### Request a context report

Ask the agent to research a topic before making decisions:

> "Give me a context report on how authentication currently works in this project."

The agent researches the codebase, analyzes alignment with project standards, and produces a structured report at `.claude/plans/context-authentication-report.md`.

---

## What it produces

| Output | Location | Purpose |
|---|---|---|
| Knowledge files | `.claude/knowledge/*.md` | Institutional memory for all agents |
| Context reports | `.claude/plans/context-{topic}-report.md` | Pre-decision research |
| Context session logs | `.claude/tasks/context_session_{id}.md` | Append-only audit trail |

---

## Real-world examples

This agent has been deployed in production on:

- **NestJS backends** — managing module inventory, DI conventions, DTO patterns, and NestJS-specific constraints
- **Django REST API backends** — managing app structure, ORM rules, pre-commit hook enforcement, and Jira workflow conventions

It scales to any project where multiple Claude agents need to share accurate, structured project knowledge.

---

## Requirements

- [Claude Code](https://claude.ai/code) — CLI or web app
- A `.claude/agents/` directory in your project root
- A `CLAUDE.md` at project root (recommended — the agent updates its Documentation Map section)

---

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md).
