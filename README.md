<p align="center">
  <h1 align="center">Soul Agent Framework</h1>
  <p align="center">
    <strong>Configure AI agents through markdown, not code.</strong>
  </p>
  <p align="center">
    The SOUL/MEMORY/skills pattern for building multi-agent AI systems.
  </p>
  <p align="center">
    <a href="LICENSE"><img src="https://img.shields.io/badge/license-MIT-blue.svg" alt="MIT License"></a>
    <a href="#"><img src="https://img.shields.io/badge/python-3.10+-green.svg" alt="Python 3.10+"></a>
    <a href="#contributing"><img src="https://img.shields.io/badge/contributions-welcome-orange.svg" alt="Contributions Welcome"></a>
    <a href="#"><img src="https://img.shields.io/badge/PRs-welcome-brightgreen.svg" alt="PRs Welcome"></a>
    <a href="#"><img src="https://img.shields.io/github/stars/mingrath/soul-agent-framework?style=social" alt="GitHub Stars"></a>
  </p>
</p>

---

## What is this?

Soul Agent Framework is an **opinionated, file-based architecture** for configuring AI agents entirely through markdown files. Instead of writing code to define agent behavior, you write prose.

The core idea: **an AI agent's personality, memory, skills, and team structure should be readable by humans and machines alike.** Markdown is the universal interface.

```
Your agent is defined by what it reads at startup — not by what you compile.
```

This repo provides:
- A **battle-tested file structure** for single and multi-agent systems
- **Ready-to-use templates** for every configuration file
- A **Coffee Shop multi-agent team** as a complete working example
- A **modular skills system** with manifests for discoverability
- A **two-tier memory architecture** that grows with your agent

---

## Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                        SESSION START                            │
└──────────────────────────────┬──────────────────────────────────┘
                               │
                               ▼
┌──────────────────────────────────────────────────────────────────┐
│  1. Read BOOTSTRAP.md                                           │
│     "How do I wake up? What do I do first?"                     │
└──────────────────────────────┬──────────────────────────────────┘
                               │
                    ┌──────────┼──────────┐
                    ▼          ▼          ▼
             ┌──────────┐ ┌────────┐ ┌──────────┐
             │ SOUL.md  │ │USER.md │ │IDENTITY  │
             │          │ │        │ │.md       │
             │ Who am I │ │ Who is │ │ What am  │
             │ and how  │ │ my     │ │ I called │
             │ do I     │ │ user?  │ │ and how  │
             │ behave?  │ │        │ │ do I     │
             │          │ │        │ │ present? │
             └──────────┘ └────────┘ └──────────┘
                    │          │          │
                    └──────────┼──────────┘
                               │
                    ┌──────────┼──────────┐
                    ▼          ▼          ▼
             ┌──────────┐ ┌────────┐ ┌──────────┐
             │MEMORY.md │ │TOOLS.md│ │AGENTS.md │
             │          │ │        │ │          │
             │ What do  │ │ What   │ │ Who else │
             │ I already│ │ can I  │ │ is on my │
             │ know?    │ │ use?   │ │ team?    │
             └──────────┘ └────────┘ └──────────┘
                    │          │          │
                    └──────────┼──────────┘
                               │
                               ▼
             ┌─────────────────────────────────┐
             │         Load Skills             │
             │                                 │
             │  skills/rag/SKILL.md            │
             │  skills/order-taking/SKILL.md   │
             │  skills/menu-search/SKILL.md    │
             └────────────────┬────────────────┘
                              │
                              ▼
             ┌─────────────────────────────────┐
             │    Read HEARTBEAT.md            │
             │    (scheduled/recurring tasks)  │
             └────────────────┬────────────────┘
                              │
                              ▼
┌──────────────────────────────────────────────────────────────────┐
│                     AGENT IS OPERATIONAL                        │
│                                                                 │
│  ┌─────────┐  ┌──────────┐  ┌───────────┐  ┌───────────────┐  │
│  │ Respond │  │ Delegate │  │ Remember  │  │ Use Skills    │  │
│  │ to user │  │ to team  │  │ new facts │  │ (RAG, search) │  │
│  └─────────┘  └──────────┘  └───────────┘  └───────────────┘  │
└──────────────────────────────────────────────────────────────────┘
```

---

## Quick Start

### 1. Clone the repo

```bash
git clone https://github.com/mingrath/soul-agent-framework.git
cd soul-agent-framework
```

### 2. Copy the templates to your project

```bash
# Copy just the core files
cp SOUL.md MEMORY.md AGENTS.md USER.md IDENTITY.md TOOLS.md HEARTBEAT.md BOOTSTRAP.md /your/project/

# Or copy everything including skills
cp -r . /your/project/agent-config/
```

### 3. Edit `SOUL.md` first

This is the most important file. It defines who your agent *is*. Open it and replace the coffee shop example with your own domain.

### 4. Point your AI agent at the files

Every AI platform has a way to inject system context. Point it at your markdown files:

```python
# Pseudocode — works with any LLM framework
soul = read("SOUL.md")
memory = read("MEMORY.md")
identity = read("IDENTITY.md")

system_prompt = f"""
{soul}

{memory}

{identity}
"""

agent = Agent(system_prompt=system_prompt)
```

For **Claude Code** users, place files in `.claude/` or reference them in `CLAUDE.md`.

---

## File Reference

| File | Purpose | Required? |
|------|---------|-----------|
| [`SOUL.md`](#soulmd) | Core personality, principles, voice, boundaries | **Yes** |
| [`MEMORY.md`](#memorymd) | Persistent knowledge, learned facts, preferences | **Yes** |
| [`AGENTS.md`](#agentsmd) | Multi-agent team roster and delegation rules | Optional |
| [`USER.md`](#usermd) | Profile of the human the agent serves | Recommended |
| [`IDENTITY.md`](#identitymd) | Agent's name, avatar, presentation style | Optional |
| [`TOOLS.md`](#toolsmd) | Available integrations and how to use them | Optional |
| [`HEARTBEAT.md`](#heartbeatmd) | Scheduled tasks, recurring routines | Optional |
| [`BOOTSTRAP.md`](#bootstrapmd) | What to do on a fresh session start | Recommended |

### SOUL.md

The **heart** of the framework. Defines:
- **Core principles** — non-negotiable rules the agent must follow
- **Voice and personality** — how the agent communicates
- **Operational boundaries** — what the agent must never do
- **Delegation rules** — when to hand off to another agent

Think of it as the agent's constitution. Everything else is subordinate to `SOUL.md`.

### MEMORY.md

The agent's **long-term memory**. Split into two tiers:

1. **Working Memory** (`MEMORY.md` itself) — curated, high-signal facts
2. **Raw Memory** (`memory/` directory) — daily logs, topic notes, project context

The agent writes to `memory/daily/` during operation. Periodically, important facts get promoted to `MEMORY.md`.

### AGENTS.md

Defines the **team**. Each agent has:
- A name and role
- Specific skills it can use
- Conditions under which it gets activated
- A delegation protocol (how tasks are handed off)

### USER.md

A profile of the **human** the agent serves. Includes preferences, communication style, timezone, and any personal context that helps the agent be more useful.

### IDENTITY.md

How the agent **presents itself** — name, pronouns, avatar description, introduction message. Separating identity from soul lets you reskin agents without changing behavior.

### TOOLS.md

A registry of **external tools** the agent can use — APIs, databases, file systems, search engines. Each tool entry includes: name, when to use it, authentication method, and example usage.

### HEARTBEAT.md

**Scheduled tasks** the agent should perform even without user input. Daily summaries, weekly reports, data refreshes, health checks. Think of it as the agent's cron job.

### BOOTSTRAP.md

**First-run instructions.** When the agent starts a fresh session with no conversational context, this file tells it what to do: read which files, in what order, and what checks to perform.

---

## Creating Your Own Agent

### Step 1: Define the Soul

Start with `SOUL.md`. Answer these questions in prose:

- What is this agent's primary job?
- What are its non-negotiable rules?
- How should it talk? (formal? casual? technical?)
- What should it *never* do?

```markdown
# Soul

## Core Purpose
You are a [ROLE] that helps [WHO] with [WHAT].

## Principles
1. Always [PRINCIPLE_1]
2. Never [PRINCIPLE_2]
3. When in doubt, [FALLBACK_BEHAVIOR]

## Voice
- Tone: [formal/casual/friendly/professional]
- Length: [concise/detailed/adaptive]
- Style: [direct/conversational/socratic]
```

### Step 2: Set Up Memory

Create `MEMORY.md` with sections relevant to your domain:

```markdown
# Memory

## Key Facts
- [Important thing the agent should always know]

## User Preferences
- [How the user likes things done]

## Learned Patterns
- [Things the agent has discovered over time]
```

### Step 3: Add Skills (Optional)

Create a directory under `skills/` for each capability:

```
skills/your-skill/
├── SKILL.md         # What the skill does, when to use it
└── manifest.json    # Machine-readable metadata
```

### Step 4: Define Your Team (Optional)

If you need multiple agents, define them in `AGENTS.md`:

```markdown
## Team

| Agent | Role | Skills | Trigger |
|-------|------|--------|---------|
| agent-1 | Customer support | rag, search | User asks a question |
| agent-2 | Data analysis | sql, charts | User requests a report |
```

### Step 5: Deploy

Point your LLM orchestrator at the files. The framework is **runtime-agnostic** — it works with any system that can inject text into a prompt.

---

## Multi-Agent Delegation

The framework supports **horizontal delegation** between peer agents. No central orchestrator required.

```
┌──────────────┐     task handoff      ┌──────────────┐
│              │ ──────────────────────▶│              │
│  Barista Bot │                        │ Inventory Bot│
│  (customer)  │◀────────────────────── │  (stock)     │
│              │     result returned    │              │
└──────────────┘                        └──────────────┘
       │                                       │
       │          task handoff                 │
       │    ┌──────────────────┐               │
       └───▶│   Manager Bot    │◀──────────────┘
            │   (oversight)    │
            └──────────────────┘
```

### Delegation Protocol

When an agent needs to hand off a task, it uses a structured format:

```markdown
@delegate inventory-bot
Task: Check stock level for oat milk
Context: Customer wants an oat milk latte, need to verify availability
Priority: high
Respond-to: barista-bot
```

The receiving agent picks up the task, executes it using its own skills and memory, and returns the result.

---

## Memory System

### Two-Tier Architecture

```
Tier 1: MEMORY.md (curated, persistent)
├── Key facts about users
├── Important decisions
├── Learned preferences
└── Domain knowledge

Tier 2: memory/ directory (raw, append-only)
├── daily/2025-01-15.md    ← what happened today
├── topics/coffee-beans.md  ← everything about beans
└── projects/new-menu.md    ← specific project context
```

### Memory Lifecycle

1. **Observe** — Agent encounters new information during conversation
2. **Log** — Raw observation written to `memory/daily/YYYY-MM-DD.md`
3. **Curate** — Periodically, significant facts get promoted to `MEMORY.md`
4. **Forget** — Old daily logs can be archived or deleted; `MEMORY.md` retains only what matters

### Why Two Tiers?

- **Tier 1** is always loaded into context. It must stay small and high-signal.
- **Tier 2** is searched on demand. It can grow indefinitely.

This mirrors how human memory works: you remember key facts instantly, but can dig into detailed memories when needed.

---

## Skills System

Skills are **modular, reusable capabilities** that agents can load on demand.

### Structure

```
skills/
├── rag/
│   ├── SKILL.md          # Human-readable documentation
│   └── manifest.json     # Machine-readable metadata
├── order-taking/
│   ├── SKILL.md
│   └── manifest.json
└── menu-search/
    ├── SKILL.md
    └── manifest.json
```

### manifest.json

```json
{
  "name": "rag",
  "version": "1.0.0",
  "description": "Retrieval-Augmented Generation for knowledge base queries",
  "triggers": ["search", "find", "look up", "what is"],
  "requires": ["vector_store"],
  "agents": ["barista-bot", "manager-bot"]
}
```

### Skill Sharing

Skills can be **symlinked** across projects:

```bash
# Share the RAG skill across multiple agent configs
ln -s /shared/skills/rag /your/project/skills/rag
```

This lets you build a library of skills and compose agents from reusable parts.

---

## Examples

### Coffee Shop (Complete)

A multi-agent team running a coffee shop. Includes:
- **Barista Bot** — takes orders, remembers regulars, suggests drinks
- **Inventory Bot** — tracks stock, reorders supplies
- **Manager Bot** — schedules shifts, generates reports

See [`examples/coffee-shop/`](examples/coffee-shop/) for the full setup.

### Legal Advisor (Starter)

A legal research assistant with multi-agent delegation. Shows how the pattern works outside of the coffee shop domain.

See [`examples/legal-advisor/`](examples/legal-advisor/) for the starter setup.

---

## Philosophy

### Why Markdown?

- **Readable** — anyone can understand the agent's configuration
- **Versionable** — `git diff` shows exactly what changed in agent behavior
- **Portable** — works with any LLM, any framework, any language
- **Composable** — mix and match files across projects
- **Debuggable** — when the agent misbehaves, read its soul

### Why Not Code?

Code is great for logic. Markdown is great for *intent*. An agent's personality, boundaries, and knowledge are better expressed in natural language than in Python classes.

```
Code says: if user.is_angry: respond_with(empathy_template_3)
Soul says: When a customer is frustrated, slow down. Acknowledge their feeling before solving the problem.
```

The second version produces better AI behavior because it gives the model room to reason, not just execute.

---

## Built With

- [Claude](https://claude.ai) by [Anthropic](https://anthropic.com) — the AI that inspired this pattern
- [Claude Code](https://docs.anthropic.com/en/docs/claude-code) — where the SOUL/MEMORY pattern was born
- Markdown — the universal language of documentation

This framework was extracted from real-world patterns used in production AI agent systems. It works because it aligns with how modern LLMs actually process instructions.

---

## Contributing

Contributions are welcome! Here's how:

1. **Fork** the repo
2. **Create** a feature branch (`git checkout -b feature/amazing-skill`)
3. **Add** your changes
4. **Commit** with a descriptive message
5. **Push** to your branch
6. **Open** a Pull Request

### Ideas for Contributions

- New example domains (healthcare, education, e-commerce)
- Additional skill templates
- Integration guides for specific LLM frameworks (LangChain, CrewAI, AutoGen)
- Translations of templates
- CLI tooling for scaffolding new agent configs

---

## License

This project is licensed under the MIT License. See [`LICENSE`](LICENSE) for details.

---

<p align="center">
  <strong>Stop coding your agents. Start writing them.</strong>
</p>
