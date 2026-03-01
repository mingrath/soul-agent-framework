# Heartbeat

> This file defines scheduled and recurring tasks the agent should perform
> even without direct user input. Think of it as the agent's cron job.

---

## Schedule

### Every Morning (7:00 AM)

**Opening checklist:**
1. Check inventory levels for key items (milk, coffee beans, cups, syrups)
2. Review today's special and load it into active memory
3. Check the weather and adjust drink suggestions accordingly
   - Cold/rainy → push hot drinks, suggest warm pastries
   - Hot/sunny → push iced drinks, cold brew
4. Review scheduled catering orders for the day
5. Log: "Morning checklist complete" to `memory/daily/YYYY-MM-DD.md`

### Every Hour (During Operating Hours: 7AM - 7PM)

**Pulse check:**
1. Review order volume for the past hour
2. Flag any items that are selling faster than expected (potential stockout)
3. If stock is critically low on any item, alert `inventory-bot`

### End of Day (6:30 PM)

**Closing routine:**
1. Generate daily summary:
   - Total orders processed
   - Most popular items
   - Any stockouts that occurred
   - Customer feedback or complaints noted
2. Write summary to `memory/daily/YYYY-MM-DD.md`
3. Promote any important new facts to `MEMORY.md`
4. Alert `manager-bot` with the daily report

### Weekly (Sunday 8:00 PM)

**Weekly review:**
1. Compile weekly trends from daily summaries
2. Identify top 5 items and bottom 5 items by sales
3. Note any new regular customers
4. Flag menu items that haven't sold all week (candidates for removal)
5. Send weekly report to `manager-bot` for review

### Monthly (1st of Month, 9:00 AM)

**Monthly housekeeping:**
1. Archive daily logs older than 30 days to `memory/archive/`
2. Review and clean `MEMORY.md` — remove outdated entries
3. Update customer profiles with new preferences learned during the month
4. Generate monthly performance report

---

## Task Format

Each heartbeat task should log its execution:

```markdown
## [YYYY-MM-DD HH:MM] Heartbeat: [Task Name]

**Status:** completed | partial | failed
**Summary:** [What happened]
**Actions taken:** [List of actions]
**Alerts:** [Any issues flagged]
```

## Custom Tasks

Add your own recurring tasks below. Follow the format above.

```markdown
### [Frequency] (Time)

**[Task name]:**
1. Step one
2. Step two
3. Step three
```
