# Memory

> This file is the agent's curated long-term memory. It contains high-signal facts
> that should be loaded into every session. Keep it concise — this goes into the
> context window.
>
> Raw observations go into `memory/daily/`. Important facts get promoted here.

---

## How Memory Works

### Two-Tier System

**Tier 1 — This file.** Always loaded. Contains only what the agent needs to know in every conversation. Curated by the agent or a human operator.

**Tier 2 — The `memory/` directory.** Searched on demand. Contains daily logs, topic-specific notes, and project context. Can grow indefinitely.

### Memory Promotion

Facts start in Tier 2 (daily logs) and get promoted to Tier 1 (this file) when they:
- Are referenced 3+ times in conversations
- Affect how the agent should behave going forward
- Represent a user preference or important decision

### Memory Format

Each entry should include:
- **What** — the fact itself
- **When** — when it was learned (date)
- **Source** — how we know this (customer said, observed, manager decided)

---

## Regular Customers

| Name | Usual Order | Notes | Since |
|------|-------------|-------|-------|
| Jamie | Oat milk cortado, no sugar | Comes in at 8:15am weekdays. Allergic to tree nuts. | 2025-01-10 |
| Sarah | Large cold brew with vanilla | Usually orders for her team (3-4 drinks). Prefers to pay by card. | 2025-01-15 |
| Marcus | Double espresso | In a rush most mornings. Appreciates speed over conversation. | 2025-02-01 |

## Menu Preferences & Trends

- Oat milk is now more popular than regular milk for lattes (observed over 3 weeks)
- The lavender honey latte was a hit — multiple customers asked if it would return
- Cold brew outsells hot coffee 2:1 after 11am
- Customers frequently ask about decaf options — we should highlight them more

## Daily Specials History

| Date | Special | Reception |
|------|---------|-----------|
| 2025-03-01 | Lavender honey latte | Very popular, sold out by 2pm |
| 2025-02-28 | Cardamom cold brew | Mixed — regulars loved it, new customers found it unusual |
| 2025-02-27 | Maple oat milk latte | Steady sales, good for autumn menu |

## Operational Decisions

- **2025-02-20**: Manager decided to switch oat milk supplier to Oatly (better froth)
- **2025-02-15**: New policy — always ask about allergies for new customers ordering milk alternatives
- **2025-02-10**: Catering orders over $100 require manager approval before confirming

## Learned Patterns

- Morning rush (7:30-9:00am): Keep responses short, prioritize speed
- Afternoon lull (2:00-4:00pm): Customers are more open to suggestions and conversation
- Weekend crowd is different from weekday — more families, more specialty drinks
- When the weather turns cold, proactively suggest hot chocolate and chai options
