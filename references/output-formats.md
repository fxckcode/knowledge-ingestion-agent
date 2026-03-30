# Output Formats Reference

Standard output formats for the knowledge-ingestion-agent. The agent uses these exact formats so other agents and users can parse responses reliably.

---

## Workflow A — Context Report Complete

```
✅ Project Context Report Complete

**Report**: `.claude/plans/context-{topic}-report.md`
**Context Updated**: `.claude/tasks/context_session_{session_id}.md`

**Highlights**:
- Alignment: {Yes/Partial/No with project standards}
- Key finding: {most important insight in one sentence}
- Recommendation: {summarized approach in one sentence}
- Risks: {count} identified

**Next Steps**: Parent reviews report, then proceeds with informed decisions
```

### Example

```
✅ Project Context Report Complete

**Report**: `.claude/plans/context-authentication-report.md`
**Context Updated**: `.claude/tasks/context_session_abc123.md`

**Highlights**:
- Alignment: Partial — JWT strategy exists but refresh token rotation is not implemented
- Key finding: Auth module uses Passport.js but lacks token blacklisting on logout
- Recommendation: Add Redis-backed token blacklist before implementing refresh rotation
- Risks: 2 identified (breaking API contract for mobile clients, session invalidation edge cases)

**Next Steps**: Parent reviews report, then proceeds with informed decisions
```

---

## Workflow B — Knowledge Ingested Successfully

```
✅ Knowledge Ingested Successfully

**Date**: YYYY-MM-DD
**Classification**: {business-rule | architecture | convention | process | tool-tech | bug-pattern | security}
**File**: `.claude/knowledge/{filename}.md`
**Action**: {Created new file | Updated existing file}
**CLAUDE.md**: {Updated Documentation Map | No update needed}

**What was ingested**:
- {Summary bullet of each piece of knowledge captured}

**Cross-References Updated**: {Yes — updated {file1}, {file2} | No}
**Conflicts Detected**: {Yes — {description} | No}
**Token estimate**: ~{n} tokens
```

### Example — new rule added

```
✅ Knowledge Ingested Successfully

**Date**: 2025-06-15
**Classification**: security
**File**: `.claude/knowledge/critical-constraints.md`
**Action**: Updated existing file
**CLAUDE.md**: No update needed

**What was ingested**:
- Payment service MUST tokenize card numbers via Stripe before persisting — never store raw PAN data

**Cross-References Updated**: Yes — added note to `business-rules.md ## Payment Rules`
**Conflicts Detected**: No
**Token estimate**: ~210 tokens
```

### Example — new file created

```
✅ Knowledge Ingested Successfully

**Date**: 2025-06-15
**Classification**: architecture
**File**: `.claude/knowledge/event-sourcing.md`
**Action**: Created new file
**CLAUDE.md**: Updated Documentation Map — added `.claude/knowledge/event-sourcing.md` (~400 tokens)

**What was ingested**:
- Events are append-only and stored in the `events` table — never deleted or mutated
- Projections are rebuilt from events on startup in test environments
- Command handlers emit events via the EventBus — never write to read models directly

**Cross-References Updated**: Yes — noted in `architecture-overview.md ## Data Flow`
**Conflicts Detected**: No
**Token estimate**: ~380 tokens
```

---

## Conflict detected (before writing)

Used when new content contradicts existing knowledge. The agent stops and asks for confirmation before writing.

```
⚠️ Conflict Detected — Confirmation Required

**Topic**: {what the conflict is about}

**Existing rule** (in `.claude/knowledge/{filename}.md ## {section}`):
> {exact text of the existing rule}

**New rule** (from your message):
> {exact text of the new rule}

**Impact**: {what changes if the new rule takes precedence}

Which rule takes precedence? Please confirm and I will update the file accordingly.
If the new rule supersedes the old one, I will document the change inline.
```

---

## Skipped — already documented

Used when the user provides knowledge that's already captured in the knowledge base.

```
ℹ️ Already Documented

**File**: `.claude/knowledge/{filename}.md ## {section}`

This rule is already in the knowledge base:
> {existing content}

No changes made. Let me know if the rule has changed or needs clarification.
```

---

## Status prefixes

The agent uses these prefixes consistently so outputs are scannable:

| Prefix | Meaning |
|---|---|
| `✅` | Action completed successfully |
| `⚠️` | Warning — requires user attention or confirmation |
| `ℹ️` | Informational — no action taken |
| `❌` | Error or blocked — describes why |
| `> Assumption:` | Inline assumption label in reports |
