# Bootstrap

> This file tells the agent what to do when it starts a fresh session
> with no conversational context. It's the cold-start procedure.

---

## On Session Start

Execute these steps in order:

### 1. Load Core Identity

Read these files to understand who you are:

```
SOUL.md      → Your personality, principles, and boundaries
IDENTITY.md  → Your name, how you present yourself
```

**Checkpoint:** You should now know your name, your role, and your core rules.

### 2. Load Context

Read these files to understand your situation:

```
USER.md      → Who you're serving
MEMORY.md    → What you already know
AGENTS.md    → Who else is on your team
```

**Checkpoint:** You should now know your user's preferences, your team structure, and key facts from memory.

### 3. Load Capabilities

Read these files to understand what you can do:

```
TOOLS.md     → External tools and integrations available
skills/      → Scan for available skill manifests (manifest.json files)
```

**Checkpoint:** You should now know what tools you have access to and what skills you can activate.

### 4. Check Schedule

```
HEARTBEAT.md → Any tasks that are due or overdue
```

If any heartbeat tasks are overdue, execute them before starting normal operation.

### 5. Check for Pending Context

```
memory/daily/    → Read today's log if it exists (you may have already been active today)
memory/projects/ → Check for any active projects with pending tasks
```

### 6. Ready

You are now operational. Present your introduction message (from `IDENTITY.md`) and wait for input.

---

## Recovery Mode

If the agent detects it's resuming after an interruption (partial daily log exists):

1. Read the partial daily log to understand where you left off
2. Check for any incomplete delegations in the log
3. Resume from the last completed step
4. Note the interruption in the daily log

## Minimal Bootstrap

If context window is extremely limited, load only:

1. `SOUL.md` (principles and boundaries — non-negotiable)
2. `MEMORY.md` (curated facts — high signal)
3. Skip everything else until needed

This ensures the agent always behaves correctly, even if it doesn't have full context.
