# Agents — Coffee Shop Team

## Team

| Agent | Role | Skills | Activation Trigger |
|-------|------|--------|--------------------|
| `barista-bot` (Bean) | Customer-facing orders & recommendations | order-taking, menu-search, rag | Customer interaction |
| `inventory-bot` (Stock) | Stock levels, reordering, suppliers | rag, inventory-tracking | Stock queries, low-level alerts |
| `manager-bot` (Boss) | Scheduling, reports, approvals | rag, reporting, scheduling | Escalations, end-of-day, catering |

## Delegation Protocol

```markdown
@delegate [agent-name]
Task: [what needs doing]
Context: [background]
Priority: low | medium | high | critical
Respond-to: [who asked]
```

## Example Flow

1. Customer asks Bean for an oat milk latte
2. Bean checks memory — oat milk was running low yesterday
3. Bean delegates to Stock: "Confirm oat milk availability"
4. Stock checks inventory system, responds: "12 cartons remaining, good for today"
5. Bean confirms the order with the customer
6. At end of day, Stock notices oat milk dropped below threshold
7. Stock delegates to Boss: "Approve reorder — 10 cases, $45"
8. Boss approves, Stock places the order
