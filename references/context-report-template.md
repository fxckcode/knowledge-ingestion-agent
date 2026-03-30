# Context Report Template

Copy this template when creating a new context report at `.claude/plans/context-{topic}-report.md`.

---

```markdown
# {Topic} — Project Context Report

**Created**: {YYYY-MM-DD}
**Session**: {session_id}
**Requested by**: {agent name or "user"}

---

## 1. Project Context

{Relevant background about the project's architecture as it relates to this topic.
Keep this to 2–3 sentences. The reader already knows the project — focus on
what's relevant to the specific topic.}

---

## 2. Current State

**Existing Modules / Apps**:
- `{module}` — {one-line purpose}
- `{module}` — {one-line purpose}

**Infrastructure / Services**:
- {What's deployed or configured today that's relevant to this topic}

**Conventions in Use**:
- {Relevant patterns and standards already established}

---

## 3. Business Logic

**Domain Rules**:
- {Relevant business rules and constraints}

**Data Models**:
- {Entity relationships that apply to this topic}

**Operational Constraints**:
- {Limits, requirements, dependencies}

---

## 4. Architecture Analysis

**Component Relationships**:
{How the relevant parts fit together}

**Dependency Flow**:
{What depends on what — e.g., controller → service → repository}

**Patterns in Use**:
{Guards, interceptors, middleware, pipes, etc. relevant to this topic}

### Architecture Diagram (ASCII)

```
{Simple ASCII diagram of relevant component relationships}

Example:
┌──────────────┐     ┌──────────────┐     ┌──────────────┐
│  Controller  │────▶│   Service    │────▶│  Repository  │
└──────────────┘     └──────────────┘     └──────────────┘
                            │
                            ▼
                     ┌──────────────┐
                     │   Database   │
                     └──────────────┘
```

---

## 5. Alignment Assessment

**Aligns with project standards**: Yes / Partial / No

**Reasoning**:
{Why it does or doesn't align — be specific about which standards apply}

**Conflicts identified**:
- {Any contradictions with existing decisions, or "None"}

---

## 6. Recommendation

**Approach**: {Recommended path forward}

**Rationale**: {Why this approach best fits the project}

**Trade-offs**:
- Gain: {What you gain}
- Lose: {What you give up or complicate}

---

## 7. Risks & Considerations

**Technical Risks**:
- {Potential issues with implementation}

**Business / API Risks**:
- {Impact on consumers, breaking changes, environment differences}

**Technical Debt**:
- {Debt this might introduce or resolve}

---

## 8. Facts vs Assumptions

### Confirmed Facts (grounded in codebase)

- {fact}: found in `{file path}`
- {fact}: found in `{file path}`

### Assumptions (not documented — labeled as inference)

> Assumption: {statement} — inferred from {evidence}
> Assumption: {statement} — needs confirmation from team

---

## 9. Related Knowledge

**Relevant Files**:
- `{file}`: {why it's relevant}
- `{file}`: {why it's relevant}

**Related Decisions**:
- {Previous architectural decision that affects this}

**Documentation Gaps**:
- {What's missing from .claude/knowledge/ and should be added}

---

## 10. Important Notes

⚠️ **Critical Alignment Issues**:
- {Anything that conflicts with project direction, or "None"}

💡 **Opportunities**:
- {Improvements this context enables}

📝 **Needs Documentation**:
- {Decisions or rules that should be formally ingested into .claude/knowledge/}
```

---

## Notes on filling the template

- **Section 8 is mandatory** — every report must explicitly separate facts from assumptions
- **Section 10 "Needs Documentation"** drives follow-up knowledge ingestion — always fill it
- Keep the ASCII diagram simple — it should convey structure at a glance, not replace docs
- If a section doesn't apply to the topic, write "Not applicable" — don't delete the section
- Reports are read-only once written — never edit an existing report; create a new one if the topic evolves
