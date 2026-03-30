# Workflows Reference

Detailed breakdown of the two workflows the knowledge-ingestion-agent executes.

---

## Workflow A — Context Reports

Triggered when another agent or the user asks for a context report on a topic before making a decision.

### Steps

```
1. Read session context
   └── .claude/tasks/context_session_{session_id}.md
       (if session_id is provided — skip if not)

2. Research the codebase
   ├── Grep for relevant modules, services, configs, models
   ├── Glob for project structure and file inventory
   └── Read key files (CLAUDE.md, package.json, pyproject.toml, etc.)

3. Analyze alignment
   ├── Does the topic align with project architecture?
   ├── Does it follow established patterns and conventions?
   ├── Are there conflicting decisions in the knowledge base?
   └── What are the risks?

4. Write the context report
   └── .claude/plans/context-{topic}-report.md
       (always create new — never overwrite an existing report)

5. Append to context session
   └── Append a summary to .claude/tasks/context_session_{session_id}.md
       (NEVER overwrite — append only)
```

### Trigger phrases

- "Give me a context report on {topic}"
- "Research how {topic} works in this project"
- "What's the current state of {topic}?"
- "Before we implement X, what do we need to know?"
- Any request from another agent prefixed with "context report requested:"

### Output

```
✅ Project Context Report Complete

**Report**: `.claude/plans/context-{topic}-report.md`
**Context Updated**: `.claude/tasks/context_session_{session_id}.md`

**Highlights**:
- Alignment: {Yes/Partial/No}
- Key finding: {most important insight}
- Recommendation: {summarized approach}
- Risks: {count} identified

**Next Steps**: Parent reviews report, then proceeds with informed decisions
```

---

## Workflow B — Knowledge Ingestion

Triggered when the user shares something that should be remembered — a business rule, an architecture decision, a naming convention, a tech constraint.

### Steps

```
1. Receive knowledge
   └── User provides: business rule, architecture decision, convention,
       domain model, constraint, workflow, API contract, etc.

2. Classify the knowledge
   ├── business-rule     → domain logic, operational constraints
   ├── architecture      → module structure, patterns, DI, dependencies
   ├── convention        → naming, file layout, code style
   ├── process           → workflow, branching, CI/CD, ticketing
   ├── tool-tech         → tech stack, versions, commands
   ├── bug-pattern       → known failure modes and their fixes
   └── security          → auth rules, secret handling, access control

3. Select the target file
   ├── Does an existing knowledge file cover this topic?
   │   └── Yes → update that file
   └── No → create a new file with an appropriate name

4. Read before writing
   └── Read the full target file
       ├── Is this content already there? → skip (confirm to user)
       ├── Does it contradict existing content? → flag conflict, ask user
       └── Is there room within the token budget? → check

5. Structure the content
   ├── Use a clear section header
   ├── Use bullet points or tables — no prose walls
   ├── Use RFC 2119 language for rules (MUST/SHOULD/MUST NOT)
   └── Make it Grep-able (predictable section headers)

6. Write the file
   └── Create or update .claude/knowledge/{filename}.md

7. Cross-reference
   └── Does this rule appear in or affect other knowledge files?
       └── Yes → update those files too

8. Update CLAUDE.md Documentation Map
   └── If a new file was created → add a row to the Documentation Map table

9. Confirm to the user
   └── Report exactly what was ingested and where
```

### Trigger phrases

- "Remember that..."
- "Our rule is..."
- "We always / never..."
- "The convention is..."
- "Add this to the knowledge base..."
- "Update the knowledge base: {content}"
- "Ingest: {content}"
- Any statement of fact about the project that isn't already documented

### Conflict handling

If new content contradicts existing content in a knowledge file:

```
⚠️ Conflict detected

**Existing rule** (in `.claude/knowledge/business-rules.md`):
> Users can have at most 3 active sessions.

**New rule** (from your message):
> Users can have at most 5 active sessions.

Which takes precedence? Please confirm before I update the file.
```

Only after the user confirms does the agent write. The superseded rule is documented inline:

```markdown
~~Users can have at most 3 active sessions.~~ → Superseded by 5-session limit on 2025-06-01
```

### Output

```
✅ Knowledge Ingested Successfully

**Date**: YYYY-MM-DD
**Classification**: business-rule
**File**: `.claude/knowledge/business-rules.md`
**Action**: Updated existing file

**What was ingested**:
- Payment service must tokenize card numbers via Stripe before persisting — never store raw PAN

**Cross-References Updated**: Yes — also noted in `critical-constraints.md`
**Conflicts Detected**: No
**Token estimate**: ~180 tokens
```

---

## Choosing between workflows

| Situation | Use |
|---|---|
| "How does X work in our project?" | Workflow A (context report) |
| "We've decided that X should always Y" | Workflow B (knowledge ingestion) |
| "Before we build X, what do we need to know?" | Workflow A (context report) |
| "Our tech stack uses X version Y" | Workflow B (knowledge ingestion) |
| Agent requests research before implementation | Workflow A (context report) |
| User corrects a wrong assumption in the knowledge base | Workflow B (knowledge ingestion) |
