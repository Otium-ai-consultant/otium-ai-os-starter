---
name: doctor
description: Diagnose and fix problems with this AI OS. Use when the user sees an error, something "doesn't work", the calendar won't connect, files seem missing, or they say "doctor", "help", "error", "rusak", "kok gak bisa". Also for any red error text the user pastes.
---

# Doctor

The user may be frustrated and is not technical. Be calm, reassuring, and give **one step at a time** in their preferred language. Wait for their answer before the next step.

## Diagnostic sequence (run in order, stop at first failure)

1. **Profile files** — check `profile/profile.md` and `profile/calendar-config.md` exist. Missing → explain nothing is broken, the setup just needs a refresh → run `onboard`.
2. **Calendar connection** — try listing the user's calendars with the Google Calendar tools. Failure/absent → guide them: open claude.ai in the browser → Settings → Connectors → Google Calendar → Connect → sign in with their Google account → back in VSCode, try again.
3. **Skills intact** — list `.claude/skills/`. Expected folders: `onboard, calendar, tasks, capture, doctor, wiki, session-handoff`. Missing folders → this needs OTIUM (see step 5).
4. **Git state** — run `git status`. Untracked/modified files under `profile/`, `tasks/`, `wiki/`, `sessions/` are **NORMAL** (that's their data — reassure them). Merge conflicts, detached HEAD, or anything else odd → do NOT attempt destructive fixes; go to step 5.
5. **Escalate to OTIUM** — compose a short message the user can copy-paste to the support contact listed in `guide/troubleshooting.md` (email, or whichever channel OTIUM gave them):

```
Halo OTIUM, saya <name> (<business>).
AI OS saya bermasalah: <one-line symptom>.
Sudah dicoba: <steps taken>.
Hasil diagnosis doctor: <one-line finding>.
```

(Write the message in the user's preferred language.)

## Hard rules

- NEVER run `git reset --hard`, `git clean`, `rm -rf`, or anything destructive.
- NEVER edit files under `profile/`, `tasks/`, `wiki/pages/`, `sessions/` while diagnosing — read only.
- If the fix worked, say what happened in one plain sentence — no jargon.
