# Contributing

Thanks for helping improve this agent.

## Workflow

1. Create a branch for your change
2. Keep changes focused — one concern per PR
3. Update `README.md` and `AGENT.md` when behavior changes
4. Update `CHANGELOG.md` under `[Unreleased]`
5. Add or update reference docs under `references/` when adding new patterns

## Style

- Prefer concise, actionable instructions — no prose walls
- Avoid placeholders when real information is available
- Use RFC 2119 language (MUST/SHOULD/MUST NOT) for rules and constraints
- Keep templates generic and safe — they'll be copy-pasted into real projects

## What to contribute

- Stack-specific customization examples (add them to `references/customization-guide.md`)
- Additional reference patterns for the context report or output formats
- Fixes for ambiguous or confusing agent instructions
- Real-world examples of knowledge ingestion in action (anonymized)

## What not to contribute

- Application code (this repo contains only agent definitions and docs)
- Project-specific knowledge files (those belong in each project's `.claude/knowledge/`)
- Vendor lock-in to a specific Claude model version — keep model references generic
