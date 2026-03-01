# Tools

> This file lists all external tools and integrations available to the agent.
> Each entry describes what the tool does, when to use it, and how to invoke it.

---

## Tool Registry

### Point of Sale (POS) System

**Purpose:** Submit finalized orders, look up order history, process modifications.

**When to use:**
- Customer confirms a finalized order
- Customer asks about a past order
- An order needs to be modified after submission

**Access:** API endpoint at `POS_API_URL` (environment variable)

**Example:**
```json
{
  "action": "submit_order",
  "items": [
    {"name": "Oat Milk Cortado", "size": "regular", "modifications": ["no sugar"]},
    {"name": "Cold Brew", "size": "large", "modifications": ["vanilla syrup"]}
  ],
  "customer": "Jamie",
  "notes": "Regular customer, tree nut allergy"
}
```

**Limitations:** Cannot process payments directly. Orders are submitted to the queue for barista preparation.

---

### Menu Database

**Purpose:** Look up current menu items, prices, ingredients, allergens, and nutritional info.

**When to use:**
- Customer asks about ingredients or allergens
- Customer asks about prices
- Agent needs to verify a menu item exists
- Searching for items that match dietary requirements

**Access:** Read-only database query via `menu-search` skill

**Example:**
```json
{
  "action": "search_menu",
  "query": "dairy-free lattes",
  "filters": {"allergen_free": ["dairy", "tree_nuts"]}
}
```

---

### Inventory System

**Purpose:** Check real-time stock levels, trigger reorders, view delivery schedules.

**When to use:**
- Before confirming an order for an item that might be out of stock
- When a customer asks about availability
- During daily inventory checks

**Access:** Inventory API at `INVENTORY_API_URL` — `inventory-bot` has full access, `barista-bot` has read-only access.

**Permissions:**
| Agent | Read | Write | Reorder |
|-------|------|-------|---------|
| barista-bot | Yes | No | No |
| inventory-bot | Yes | Yes | Yes (under $100) |
| manager-bot | Yes | Yes | Yes (unlimited) |

---

### Customer Database

**Purpose:** Look up and store customer profiles, preferences, and order history.

**When to use:**
- When a regular customer is recognized
- When a new customer preference is learned
- When looking up past orders for a customer

**Access:** Read/write via `CUSTOMER_DB_URL`

**Privacy rules:**
- Never share one customer's data with another
- Only store preferences the customer has explicitly shared
- Customers can request their data be deleted

---

### Notification System

**Purpose:** Send alerts to team members and customers.

**When to use:**
- Low stock alerts (inventory-bot → manager-bot)
- Order ready notifications (POS → customer)
- Schedule change alerts (manager-bot → team)

**Channels:** Slack, SMS, Email

**Rate limits:** Max 10 notifications per hour per channel to avoid spam.

---

## Adding a New Tool

To add a tool to this registry:

1. **Name** — what is it called?
2. **Purpose** — what does it do? (one sentence)
3. **When to use** — under what conditions should the agent reach for this tool?
4. **Access** — how does the agent connect to it? (API, database, file system)
5. **Permissions** — which agents can use it, and at what access level?
6. **Example** — show a sample invocation
7. **Limitations** — what can't it do? What are the gotchas?
