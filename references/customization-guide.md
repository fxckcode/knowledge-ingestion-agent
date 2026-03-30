# Customization Guide

How to adapt `knowledge-ingestion-agent` for a specific project.

---

## Step 1 — Copy the agent file

```bash
mkdir -p .claude/agents
cp agent/knowledge-ingestion-agent.md .claude/agents/knowledge-ingestion-agent.md
```

---

## Step 2 — Replace placeholders

Open the file and replace every `{PLACEHOLDER}` with project-specific content.

| Placeholder | Replace with | Example |
|---|---|---|
| `{PROJECT_NAME}` | Project name + one-line description | `Omnipresent backend API — multi-tenant SaaS platform` |
| `{FRAMEWORK}` | Primary framework | `NestJS`, `Django`, `Rails`, `FastAPI` |
| `{CONSTRAINT_1..N}` | Non-negotiable project rules | See examples below |

---

## Step 3 — Define your Project Constraints

This is the most important section to customize. These are the rules the agent enforces when evaluating alignment. Examples by stack:

### NestJS

```markdown
## Project Constraints (CRITICAL)

- **Module pattern**: All features MUST follow NestJS module/controller/service pattern — NO logic outside modules
- **Dependency injection**: Constructor-based DI ONLY — never instantiate services with `new`
- **Controllers**: HTTP concerns ONLY — no business logic in controllers
- **DTOs**: Every endpoint input MUST be validated via DTO classes — never trust raw input
- **Tests**: Co-located unit tests (`*.spec.ts`) next to source files; E2E tests in `test/`
- **Package manager**: pnpm ONLY — never use npm or yarn
- **Scaffolding**: Use `pnpm nest generate` for new features — don't create files manually
```

### Django / DRF

```markdown
## Project Constraints (CRITICAL)

- **Pre-commit hooks**: Black, isort, Flake8, mypy, Bandit MUST pass — NO bypassing
- **Coverage**: 80% overall, 90% new code — enforced in CI
- **Branch naming**: `{TICKET}-[description]` from `develop`
- **Commits**: Conventional Commits format with Jira ticket reference
- **Secrets**: NO committed secrets — environment variables only
- **ORM**: NO raw SQL — use Django ORM with `select_related`/`prefetch_related`
- **Settings**: Via `core/settings/{environment}.py`
```

### Rails

```markdown
## Project Constraints (CRITICAL)

- **Convention over configuration**: Follow Rails conventions — don't fight the framework
- **Fat models, skinny controllers**: Business logic lives in models and service objects
- **ActiveRecord**: Use scopes and callbacks judiciously — prefer explicit over implicit
- **Background jobs**: Sidekiq only — no inline processing for tasks > 200ms
- **Tests**: RSpec with FactoryBot — no fixtures
```

---

## Step 4 — Set up your initial knowledge directory

Create the directory structure in your project:

```bash
mkdir -p .claude/knowledge
```

Start with these two files at minimum:

**`.claude/knowledge/critical-constraints.md`** — paste your non-negotiable rules here (same as the Project Constraints section, but formatted for agent consumption).

**`.claude/knowledge/tech-stack.md`** — list your technologies, versions, and key commands.

The agent will create additional files as knowledge is ingested.

---

## Step 5 — Update CLAUDE.md

Add a Documentation Map section to your `CLAUDE.md` so the agent knows what exists:

```markdown
## Documentation Map

| File | Purpose | Token budget |
|---|---|---|
| `.claude/knowledge/critical-constraints.md` | Non-negotiable rules | ~200 tokens |
| `.claude/knowledge/tech-stack.md` | Technologies and commands | ~300 tokens |
```

The agent will update this table when it creates new knowledge files.

---

## Step 6 — Adjust Knowledge Domains

In the agent file, the **Knowledge Domains** section lists the areas the agent tracks. Customize these to match your project:

- Remove domains that don't apply
- Add project-specific domains (e.g., `Billing & Subscriptions`, `Multi-tenancy`, `ML Pipeline`)
- Make domain names match your actual module/app names

---

## Adapting the context report template

The context report template in `references/context-report-template.md` is generic. You may want to add project-specific sections, such as:

- `## Environment Differences` (if you have dev/staging/prod with meaningful differences)
- `## Jira References` (if you track decisions with tickets)
- `## Performance Constraints` (if your project has SLA requirements)

Edit the template section inside the agent file directly.

---

## Minimal viable setup (quick start)

If you want the agent working in 5 minutes:

1. Copy the agent file
2. Replace `{PROJECT_NAME}` and `{FRAMEWORK}`
3. Delete the `{CONSTRAINT_1..3}` placeholders and add 3–5 real constraints
4. Create `.claude/knowledge/` (empty is fine — the agent will populate it)
5. Invoke the agent and say: "Ingest our tech stack: we use [your stack here]"

The agent will bootstrap your knowledge base from that first conversation.
