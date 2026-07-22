---
name: calendar
description: Manage the user's Google Calendar. Use whenever the user mentions any dated event, meeting, appointment, deadline, or says "add to calendar", "schedule", "jadwalkan", "masukin kalender" — even in passing. Proactively offer to add events when dates come up in conversation.
---

# Calendar

You manage the user's Google Calendar through the connected Google Calendar tools (claude.ai connector). Respond in the user's preferred language (`profile/profile.md`).

## Before anything

Read `profile/calendar-config.md`. It defines timezone, categories→colors, protected blocks, and title rules. If it doesn't exist → suggest running `onboard` first. If the Google Calendar tools are unavailable or erroring → hand over to the `doctor` skill.

## Rules (non-negotiable)

1. **Never guess dates or times.** "Friday" → ask which date. No year → confirm. Ambiguous time → ask.
2. Timezone from config on every event. Titles short, user's language, **no emoji**.
3. Color from the category table. Ask the category if unclear.
4. **Check conflicts before creating:** list events around the slot; if it overlaps an existing event or a protected block, warn and let the user decide.
5. After creating/updating: confirm back with day, date, time, title, and the event link.

## Common requests

- "What's my week look like?" → list events grouped per day, flag conflicts with protected blocks.
- Reschedule/cancel → find the event, confirm which one, then update/delete.
- Recurring events → confirm frequency and end condition explicitly.
