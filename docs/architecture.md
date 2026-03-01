# Architecture

> A deep dive into how the Soul Agent Framework works, why each piece exists,
> and how the pieces fit together.

---

## Overview

The Soul Agent Framework is a **file-based configuration system** for AI agents. Instead of defining agent behavior in code, you define it in markdown files that are injected into the agent's context at runtime.

```
┌─────────────────────────────────────────────────────┐
│                   Your AI Agent                     │
│                                                     │
│  ┌─────────────┐  ┌──────────┐  ┌───────────────┐  │
│  │   LLM       │  │  Runtime  │  │  External     │  │
│  │  (Claude,   │◀─│  (reads   │──│  Tools        │  │
│  │   GPT, etc) │  │  .md files│  │  (APIs, DBs)  │  │
│  └─────────────┘  │  into     │  └───────────────┘  │
│                   │  context) │                      │
│                   └──────────┘                       │
│                        ▲                             │
│                        │                             │
│              ┌─────────┴──────────┐                  │
│              │  Markdown Files    │                  │
│              │                    │                  │
│              │  SOUL.md           │                  │
│              │  MEMORY.md         │                  │
│              │  AGENTS.md         │                  │
│              │  USER.md           │                  │
│              │  IDENTITY.md       │                  │
│              │  TOOLS.md          │                  │
│              │  HEARTBEAT.md      │                  │
│              │  BOOTSTRAP.md      │                  │
│              │  skills/           │                  │
│              └────────────────────┘                  │
└─────────────────────────────────────────────────────┘
```

The framework is **runtime-agnostic**. It doesn't care what LLM you use, what programming language your orchestrator is written in, or what platform you deploy on. It only cares that your system can:

1. Read markdown files
2. Inject their contents into the LLM's context
3. Let the LLM write back to the memory files

---

## Core Concepts

### 1. The Soul

The `SOUL.md` file is the **constitution** of the agent. It defines:
- What the agent does (core purpose)
- What it must always do (principles)
- What it must never do (boundaries)
- How it communicates (voice)
- When to ask for help (delegation rules)

The soul is **immutable during a session**. The agent reads it at startup and follows it for the entire conversation. If you want to change behavior, you change `SOUL.md` — not the code.

**Design principle:** Write the soul as if you're onboarding a new employee. What would you tell them on their first day?

### 2. Memory

Memory is split into two tiers to balance **always-available context** with **unlimited storage**.

#### Tier 1: `MEMORY.md`

- Loaded into every session
- Must stay small (aim for <2000 tokens)
- Contains only high-signal, curated facts
- Updated periodically, not on every interaction

#### Tier 2: `memory/` directory

- Searched on demand using RAG or file search
- Can grow indefinitely
- Organized by time (`daily/`), topic (`topics/`), and project (`projects/`)
- Raw, append-only logs

#### Memory Lifecycle

```
Conversation happens
       │
       ▼
Observation logged to memory/daily/YYYY-MM-DD.md
       │
       ▼
After N references or explicit curation
       │
       ▼
Fact promoted to MEMORY.md (Tier 1)
       │
       ▼
Old daily logs archived or deleted
```

### 3. Skills

Skills are **modular capabilities** with two files:
- `SKILL.md` — human-readable documentation (what it does, when to use it, how it works)
- `manifest.json` — machine-readable metadata (name, version, triggers, requirements, compatible agents)

#### Why separate documentation from metadata?

The `SKILL.md` file goes into the LLM's context to teach it how to use the skill. The `manifest.json` is used by the orchestrator to decide *which* skills to load.

#### Skill Discovery

At startup, the agent scans `skills/*/manifest.json` to build a skill registry. When a user message matches a skill's trigger words, the corresponding `SKILL.md` is loaded into context.

```
User says: "What dairy-free options do you have?"
                    │
                    ▼
Orchestrator checks trigger words:
  - "dairy-free" matches menu-search skill
                    │
                    ▼
Load skills/menu-search/SKILL.md into context
                    │
                    ▼
Agent follows SKILL.md instructions to search the menu
```

#### Skill Composition

Skills can be shared across agents and projects using symlinks:

```bash
# Shared skill library
/shared/skills/
├── rag/
├── summarization/
└── scheduling/

# Project A uses rag and summarization
/project-a/skills/
├── rag -> /shared/skills/rag
└── summarization -> /shared/skills/summarization

# Project B uses rag and scheduling
/project-b/skills/
├── rag -> /shared/skills/rag
└── scheduling -> /shared/skills/scheduling
```

### 4. Multi-Agent Delegation

The framework supports **peer-to-peer delegation** between agents. There is no central orchestrator — agents communicate directly.

#### Why peer-to-peer?

A central orchestrator becomes a bottleneck and single point of failure. Peer-to-peer delegation is:
- **Resilient** — if one agent is down, others still work
- **Scalable** — add agents without reconfiguring the orchestrator
- **Debuggable** — follow the delegation chain in logs

#### Delegation Message Format

```markdown
@delegate [target-agent]
Task: [what to do]
Context: [why it's needed]
Priority: [low | medium | high | critical]
Respond-to: [who needs the result]
Deadline: [optional]
```

This format is designed to be:
- Parseable by both humans and machines
- Self-contained (the receiving agent has all the context it needs)
- Traceable (you can grep logs for `@delegate` to see the full chain)

### 5. The Heartbeat

The `HEARTBEAT.md` file defines **time-based behavior** — things the agent does on a schedule, not in response to user input.

This is essential for agents that need to:
- Monitor systems (stock levels, error rates)
- Generate periodic reports
- Perform maintenance (memory cleanup, data refresh)
- Send proactive alerts

The heartbeat is checked at session start and periodically during long sessions.

### 6. Bootstrap

The `BOOTSTRAP.md` file solves the **cold start problem**: when the agent wakes up with no conversational context, what should it do?

It defines:
- The order in which files should be loaded
- Checkpoints to verify the agent is correctly configured
- Recovery procedures for interrupted sessions
- A minimal bootstrap for limited-context environments

---

## Design Decisions

### Why markdown and not YAML/JSON/TOML?

- **Readability:** Markdown is natural language with structure. Non-technical stakeholders can read and edit it.
- **Expressiveness:** Agent behavior is better described in prose than in key-value pairs. "When a customer is frustrated, slow down" can't be expressed in YAML.
- **LLM compatibility:** LLMs are trained on massive amounts of markdown. They understand it natively.
- **Tooling:** Every editor, every platform, every version control system handles markdown.

### Why file-based and not database-backed?

- **Version control:** `git diff` shows you exactly what changed in agent behavior and when.
- **Portability:** Copy files to a new project, and the agent works.
- **Simplicity:** No database to set up, no ORM to configure, no migration scripts.
- **Debugging:** When the agent misbehaves, open the file and read it. The answer is right there.

### Why separate SOUL from IDENTITY?

These seem similar but serve different purposes:
- **SOUL** defines *behavior* — how the agent thinks, decides, and operates
- **IDENTITY** defines *presentation* — how the agent introduces itself, what it's called, how it looks

This separation lets you **reskin** an agent without changing its behavior. You could have the same barista soul powering "Bean" at one shop and "Brew" at another.

---

## Integration Patterns

### Pattern 1: Single File (Simplest)

For simple agents, you can combine everything into one file:

```
SOUL.md (includes identity, memory, and tool references)
```

### Pattern 2: Core Three

For most agents:

```
SOUL.md    — personality and rules
MEMORY.md  — persistent knowledge
TOOLS.md   — available capabilities
```

### Pattern 3: Full Framework

For production multi-agent systems:

```
All files + skills/ + memory/ + team agents
```

### Pattern 4: Micro-Agent

For single-purpose agents (e.g., a summarizer, a translator):

```
SOUL.md (10-20 lines)
skills/single-skill/SKILL.md
```

---

## Scaling Considerations

### Context Window Management

As agents accumulate memory, the total context can exceed LLM limits. Strategies:

1. **Prioritize loading:** `BOOTSTRAP.md` defines the priority order
2. **Lazy loading:** Only load skills when triggered, not at startup
3. **Memory compression:** Periodically summarize Tier 2 memory and promote to Tier 1
4. **Pruning:** Archive old daily logs, remove outdated facts

### Multi-Agent Coordination

For teams with many agents:

1. **Shared memory:** Agents can read from the same `memory/` directory
2. **Namespaced memory:** `memory/topics/inventory-bot/` for agent-specific knowledge
3. **Message queue:** For high-volume delegation, use a message queue instead of direct delegation
4. **Logging:** Every delegation should be logged for debugging and audit
