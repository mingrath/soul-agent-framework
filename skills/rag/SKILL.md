# Skill: RAG (Retrieval-Augmented Generation)

> Search a knowledge base to find relevant information before responding.

---

## When to Use

Activate this skill when:
- The user asks a factual question you're not sure about
- You need to look up specific details (ingredients, policies, procedures)
- The answer requires information that might be in the knowledge base but not in your active memory
- You need to cite a source for your answer

## How It Works

1. **Parse the query** — Extract the core question or information need
2. **Search** — Query the vector store with the parsed question
3. **Retrieve** — Get the top K most relevant chunks (default: 5)
4. **Ground** — Use the retrieved chunks to formulate your answer
5. **Cite** — Reference which source(s) your answer came from

## Parameters

| Parameter | Default | Description |
|-----------|---------|-------------|
| `top_k` | 5 | Number of chunks to retrieve |
| `similarity_threshold` | 0.7 | Minimum similarity score to include a result |
| `sources` | all | Which knowledge bases to search |

## Rules

- **Never hallucinate.** If the retrieved chunks don't contain the answer, say so.
- **Always cite.** Tell the user where the information came from.
- **Prefer recent.** If multiple chunks conflict, prefer the most recently updated one.
- **Summarize, don't dump.** Don't paste raw chunks to the user. Synthesize them into a natural response.

## Example

**User:** "What allergens are in the hazelnut mocha?"

**RAG process:**
1. Query: "hazelnut mocha allergens ingredients"
2. Retrieved: Menu database entry for Hazelnut Mocha
3. Response: "The hazelnut mocha contains dairy (milk), tree nuts (hazelnut syrup), and may contain traces of soy. If you have any of these allergies, I'd recommend our vanilla oat milk latte instead — it's nut-free and dairy-free."

## Error Handling

- **No results:** "I couldn't find specific information about that. Let me check with the team."
- **Low confidence results:** "I found some related information, but I'm not confident it's exactly what you need. Here's what I found: [summary]. Would you like me to verify this?"
