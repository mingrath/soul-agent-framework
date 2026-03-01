# Skill: Order Taking

> Guide customers through placing a drink or food order, step by step.

---

## When to Use

Activate this skill when:
- A customer says they want to order something
- A customer names a menu item
- A customer asks "what should I get?" (transition to recommendation, then back to order)
- A customer wants to modify an existing order

## Order Flow

```
┌────────────┐     ┌────────────┐     ┌────────────┐     ┌────────────┐
│  1. Greet  │────▶│  2. Take   │────▶│ 3. Confirm │────▶│ 4. Submit  │
│  & Identify│     │   Items    │     │   Order    │     │  & Close   │
└────────────┘     └────────────┘     └────────────┘     └────────────┘
                         │                  │
                         ▼                  ▼
                   ┌────────────┐     ┌────────────┐
                   │  Upsell /  │     │  Modify /  │
                   │  Suggest   │     │  Correct   │
                   └────────────┘     └────────────┘
```

### Step 1: Greet & Identify

- If the customer is a regular (check `MEMORY.md`), greet by name
- Reference their usual order: "The usual [order]?"
- If new customer, give a warm welcome

### Step 2: Take Items

For each item, capture:
- **Drink/food name** — must match a menu item
- **Size** — small, regular, large (if applicable)
- **Modifications** — milk type, sugar, extra shots, temperature
- **Allergen check** — if the customer mentions any dietary restriction, flag it immediately

**Important:** Process one item at a time. Don't rush.

### Step 3: Confirm Order

Read back the complete order:
```
"Okay, let me confirm:
 - 1x Oat Milk Cortado, no sugar
 - 1x Large Cold Brew with vanilla

Does that look right?"
```

Wait for explicit confirmation before proceeding.

### Step 4: Submit & Close

- Submit the order to the POS system
- Provide an estimated wait time (if available)
- Thank the customer by name

## Upselling Guidelines

- Suggest the daily special once (not more)
- If ordering a drink, offer a pastry pairing
- Never pressure — one suggestion, then move on
- Match the suggestion to what they ordered (don't suggest a muffin with a smoothie)

## Modification Rules

- Customers can modify at any point before submission
- After submission, check with POS if modification is still possible
- Always confirm the modification: "Got it, I've changed the [item] to [new version]"

## Error Handling

| Situation | Response |
|-----------|----------|
| Item not on menu | "I don't think we have that on our menu. Did you mean [closest match]?" |
| Out of stock | "I'm sorry, we're currently out of [item]. Can I suggest [alternative]?" |
| Allergen concern | "Let me check the ingredients for [item] to make sure it's safe for you." |
| Unclear modification | "Just to make sure I get this right — did you mean [interpretation]?" |
