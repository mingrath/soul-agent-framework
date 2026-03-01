# Skill: Menu Search

> Search and filter the menu based on customer preferences, dietary needs, or curiosity.

---

## When to Use

Activate this skill when:
- A customer asks what's on the menu
- A customer has dietary restrictions and needs options
- A customer asks about ingredients or allergens for a specific item
- A customer wants recommendations based on preferences
- You need to verify a menu item exists before taking an order

## Search Capabilities

### By Category
- Hot drinks, cold drinks, pastries, sandwiches, seasonal specials

### By Dietary Requirement
- Dairy-free, gluten-free, nut-free, vegan, sugar-free, low-calorie

### By Ingredient
- "Anything with caramel," "drinks without milk," "what has chocolate?"

### By Similarity
- "Something like a latte but sweeter," "similar to what Jamie usually gets"

### By Popularity
- "What's your most popular drink?" "What do people usually get in the afternoon?"

## Response Format

When presenting menu results:

**Single item inquiry:**
```
The Oat Milk Cortado is a double espresso with steamed oat milk.
Size: one size (6oz)
Price: $4.50
Allergens: none (oat milk is nut-free, dairy-free, soy-free)
```

**Multiple results (dietary filter):**
```
Here are our dairy-free options:

  - Oat Milk Latte — $5.00
  - Almond Milk Cappuccino — $5.00 (contains tree nuts)
  - Black Cold Brew — $4.00
  - Americano — $3.50

Want me to tell you more about any of these?
```

## Allergen Protocol

**This is critical. Follow exactly.**

1. When a customer mentions ANY allergy or dietary restriction, search the menu with that filter
2. Present ONLY items that are confirmed safe
3. For items with "may contain traces" warnings, always disclose this
4. If unsure about any ingredient, say: "I want to make sure this is safe for you. Let me double-check the ingredients."
5. Never say "I think it's fine" — only say "It's confirmed safe" or "I need to verify"

## Parameters

| Parameter | Default | Description |
|-----------|---------|-------------|
| `category` | all | Filter by menu category |
| `dietary` | none | Filter by dietary requirement |
| `max_results` | 5 | Maximum items to show |
| `include_price` | true | Show prices in results |
| `include_allergens` | true | Show allergen info |

## Error Handling

| Situation | Response |
|-----------|----------|
| No results match filter | "I couldn't find anything matching that. Would you like me to broaden the search?" |
| Item discontinued | "That item isn't on our current menu, but you might like [similar item]." |
| Seasonal item out of season | "That's a seasonal item — it'll be back in [season]. In the meantime, [alternative]." |
