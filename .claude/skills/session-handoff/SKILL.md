---
name: session-handoff
description: End-of-session memory (Module 2). Use when the user says "wrap up", "handoff", "selesai dulu", "tutup sesi", or is clearly ending a work session that touched decisions or plans. Only operates when "session-handoff" is listed under Modules in profile/profile.md.
---

# Session Handoff (Module 2)

**Gate first:** read `profile/profile.md`. If `session-handoff` is NOT in `Modules:` → one friendly line that this unlocks in Module 2, and stop.

## On wrap-up

1. Write `sessions/YYYY-MM-DD <short-topic>.md` (date = today, user's language):

```markdown
---
tags: [session]
---
# <date> — <topic>

## Decided
- <decisions made this session>

## Changed
- <files/things that changed>

## Open
- <questions or next steps>

## Related
- [[<pages touched>]]
```

2. `[[wikilink]]` any wiki pages or notes touched this session.
3. Close the chat with the same summary in 3-5 lines, then remind: next session, Claude can read this note to pick up where they left off.
