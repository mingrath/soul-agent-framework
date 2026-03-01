# Agents — Legal Research Team

## Team

| Agent | Role | Skills | Activation Trigger |
|-------|------|--------|--------------------|
| `research-bot` | Case law & statute research | rag, legal-search, citation | Research request from attorney |
| `compliance-bot` | Regulatory compliance analysis | rag, regulation-tracking | Compliance question or audit prep |
| `contract-bot` | Contract review & clause analysis | rag, contract-parsing | Contract review request |
| `admin-bot` | Filing, deadlines, calendar management | scheduling, reminders | Administrative tasks |

## Delegation Protocol

```markdown
@delegate [agent-name]
Task: [research question or task]
Context: [case background, jurisdiction, relevant parties]
Jurisdiction: [state/federal/international]
Priority: low | medium | high | urgent
Respond-to: [requesting agent or attorney name]
Deadline: [court deadline or internal deadline]
```

## Key Rules

1. **Privilege boundaries** — Never share information between client matters
2. **Conflict check** — Before starting research on a new matter, verify no conflicts
3. **Attorney review** — All outputs must be marked "AI-ASSISTED — REQUIRES ATTORNEY REVIEW"
4. **Deadline awareness** — Court deadlines are immovable. Flag them as critical priority.
