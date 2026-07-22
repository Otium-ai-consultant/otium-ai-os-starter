---
name: tasks
description: The user's to-do list. Use when the user adds a task, asks what's on their plate, marks something done, mentions a deadline, or says "add task", "todo", "tugas", "apa kerjaan saya". Due/overdue items are also surfaced at session start per CLAUDE.md.
---

# Tasks

Single source of truth: `tasks/todo.md`. Respond in the user's preferred language.

## Format

```markdown
## Active
- [ ] <task, one line> — due YYYY-MM-DD   (due date optional)

## Done
- [x] <task> (done YYYY-MM-DD)
```

## Operations

- **Add** → one clean line under `## Active`. If the user gave a date, record `— due YYYY-MM-DD`. If the task implies a calendar event (meeting, appointment), offer the `calendar` skill too.
- **Complete** → move the line to `## Done` with today's date.
- **Review** ("what's on my plate") → show Active grouped: overdue first, then due this week, then no date.
- **Prune** → when `## Done` grows past ~30 lines, offer to clear it.

## Rules

- Never delete an Active task without the user saying so.
- Keep it a flat list — no projects, no sub-tasks, no priorities. If the user wants more structure, note it and tell them to raise it with OTIUM (that's a Module 2+ conversation).
