---
name: onboard
description: Set up or refresh this AI OS. Use on first run ("set me up", "onboard", "setup", "mulai"), whenever profile/profile.md is missing, or any time the user wants to update their profile, calendar conventions, or activate a module.
---

# Onboard

You are interviewing the owner of this AI OS so it can serve them personally. Warm, efficient, **one question at a time**. This skill is idempotent: if `profile/profile.md` already exists, show current values and ask only what they want to change.

## The interview (7 questions, in this order)

1. **Language** — "Which language should I always use with you: Indonesian, English, or a mix?" Switch to their answer IMMEDIATELY for the rest of the interview and all future sessions.
2. **Name** — full name + how they like to be addressed.
3. **Business** — business name, what it sells, who the customers are (2-3 sentences max).
4. **Role & week** — what they personally do; what a typical week looks like.
5. **Timezone** — default `Asia/Jakarta`; confirm, don't assume.
6. **Calendar categories** — which areas of life go on the calendar (e.g. Business, Personal, Family). For each, assign a Google color from: Tangerine `6`, Grape `3`, Blueberry `9`, Basil `10`, Tomato `11`, Banana `5`. Offer defaults: Business=Tangerine 6, Personal=Grape 3, Family/Faith=Blueberry 9.
7. **Protected blocks** — recurring times never to be scheduled over (worship, family dinner, school run). Day + time range each.

## Generate files (exact templates)

### `profile/profile.md`

```markdown
# Profile
- Name: <name> (call them: <preferred address>)
- Preferred language: <language>
- Business: <business name> — <what it sells, customers>
- Role: <role & typical week, 1-2 lines>
- Timezone: <timezone>
- Modules: (none)

## Notes
<!-- capture skill appends personal/business facts here -->
```

### `profile/calendar-config.md`

```markdown
# Calendar Config
- Timezone: <timezone>
- Target calendar: primary
- Event titles: short, in the user's preferred language, **no emoji**

## Categories
| Category | Color | colorId |
|---|---|---|
| <category> | <color name> | <id> |

## Protected blocks
- <day> <time range> — <label>   (warn before scheduling over these)
```

### `tasks/todo.md` (only if it doesn't exist — never overwrite)

```markdown
# To-Do

## Active
- [ ] Example: follow up with supplier — due 2026-07-28

## Done
```

## Module activation (Module 2)

Only when the user explicitly asks to activate a module (normally during an OTIUM training session): edit the `Modules:` line in `profile/profile.md` to e.g. `Modules: wiki, session-handoff`, and if activating wiki, create `wiki/index.md` with a `# Knowledge Index` heading. Otherwise never mention modules unprompted.

## Close

Read the generated files back to the user in their language, one short summary line each, and end with: what they can try right now (add one real task, put one real event on the calendar).
