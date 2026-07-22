# SETUP — OTIUM Install Session Runbook

Internal document for the OTIUM trainer (not the client). Follow top-to-bottom; target durations keep the session inside 3 hours.

## Step 0 — one-time template prep (before your FIRST client, not per client)

- [ ] Fill the real OTIUM WhatsApp number in `guide/troubleshooting.md` (Support section) and commit.
- [ ] Host this repo as a **private** GitHub repo under the OTIUM org.
- [ ] Do one full dry-run of this runbook on a clean macOS user account. Fix friction before selling.

## Pre-session checklist (send to client 2 days before)

- [ ] Laptop (macOS or Windows), charged, admin rights
- [ ] **Claude paid subscription** (Pro minimum) — email them the signup link; verify before the session
- [ ] Google account they actually use for their calendar
- [ ] WhatsApp on their phone (support channel)

## Install session (~3h)

| # | Step | Target |
|---|---|---|
| 1 | Install VSCode → code.visualstudio.com | 10 min |
| 2 | Install Claude Code extension, sign in with their Claude account | 10 min |
| 3 | Get the vault: `git clone <repo-url> ~/Documents/AI-OS` (git missing on their machine → download ZIP from GitHub instead; note it in your client log — updates will be manual) | 10 min |
| 4 | VSCode → Open Folder → `~/Documents/AI-OS` | 5 min |
| 5 | Google Calendar connector: claude.ai → Settings → Connectors → Google Calendar → sign in with THEIR Google account | 10 min |
| 6 | Run `onboard` — you guide, THEY type. Their hands on the keyboard from here on | 25 min |
| 7 | Guided practice: 1 real event via `calendar` (verify it appears in their phone's calendar app — that's the wow moment), 2 real tasks via `tasks`, 1 real fact via `capture` | 45 min |
| 8 | Break something on purpose (e.g. ask about calendar with WiFi off) → have THEM run `doctor` | 10 min |
| 9 | Walk through `guide/command-card.md`, set the morning-coffee habit, save OTIUM WhatsApp in their phone | 15 min |
| 10 | Optional if time allows: install Obsidian, open the vault once (teaser for Module 2's graph view) | 10 min |

## After the session (same day)

- [ ] Record client + install date + machine/OS + git-or-zip in OTIUM's client tracking
- [ ] WhatsApp them the command card as an image + "reply here if anything feels off"

## Updating a client later (paid tune-up visit)

Client data is gitignored by design → `cd ~/Documents/AI-OS && git pull` is always safe. ZIP-installed clients: re-download ZIP, replace ONLY `.claude/`, `guide/`, `CLAUDE.md`, `README.md`, `wiki/SCHEMA.md` — never touch `profile/`, `tasks/`, `wiki/pages/`, `sessions/`.

## Module 2 activation (sold as a second training session)

Run `onboard` → activate modules `wiki` + `session-handoff` → teach the 3-rule schema → store 3 real pieces of knowledge together → open Obsidian graph view as the session climax.

## Optional power-up

Superpowers plugin (official marketplace, don't vendor): teaches Claude disciplined brainstorming/verification. Only for clients who show appetite — skip for everyone else.
