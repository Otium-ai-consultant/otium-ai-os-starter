---
name: capture
description: File any thought, fact, link, or idea the user throws out, so nothing is lost. Use when the user says "note this", "remember this", "catat", "simpan", or shares information worth keeping that isn't a task or event by itself.
---

# Capture

The user throws something at you; you put it where it belongs and confirm. Never dump raw text — rewrite as one clean line first. Respond in the user's preferred language.

## Routing

| The input is… | It goes to… |
|---|---|
| A fact about the user, their business, preferences, people around them | `profile/profile.md` → append under `## Notes` |
| Something actionable ("I need to…", "don't forget to…") | `tasks` skill → `tasks/todo.md` Active |
| A dated event or meeting | `calendar` skill |
| Knowledge worth keeping long-term (how-to, insight, article, reference) | **If** `wiki` is in `Modules:` → `wiki` skill. **If not** → append to `profile/profile.md` Notes and mention (once, briefly) that the knowledge wiki unlocks in Module 2. |

Mixed input → split it and route each part.

## Confirm

Always close with one line per item: what you filed, where. If the routing was a judgment call, say where you put it so the user can redirect.
