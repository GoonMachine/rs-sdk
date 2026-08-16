# Operator handoff — dated snapshot

**Audience: the next operator chat.** Not for Cloud A/B. Rewrite the
snapshot when you switch machines again. Durable rules stay in
[`operator.md`](operator.md).

Repo: `https://github.com/GoonMachine/rs-sdk.git` (fork of MaxBittker/rs-sdk).
Operator branch: `main`. Cloud writes: `cloud/ab-a`, `cloud/ab-b`.

## New Cursor account — first five minutes

1. Open this checkout (or `git pull origin main`). The always-on rule is
   [`.cursor/rules/koth-steer.mdc`](../.cursor/rules/koth-steer.mdc).
2. Copy the **existing** API key privately onto this login
   (`~/.cursor/cursor_api_key`, mode 600, or Cursor Secrets). A brand-new
   key cannot POST to the agent ids below.
3. Confirm A/B with `GET /v1/agents/{id}` (see `operator.md`). Do not
   launch replacements if those ids are still `ACTIVE`.
4. Next A or B job that is not mid-step: tell the VM to put
   `bots/<name>/bot.env` into **that Cloud agent’s Cursor Secrets**. Do not
   print the file. This laptop does **not** have `qstboot1` / `foodprobe1`
   / `foodkill1` credentials.
5. Stop any old operator 60s loop so two chats do not double-steer.
6. `/loop 60s` with the prompt in `operator.md`. **B never idle.**

If the old VMs are dead and you have secrets: new Cloud agents, paste
[`cloud-agent-a.md`](cloud-agent-a.md) +
[`cloud-agent-a-phase2.md`](cloud-agent-a-phase2.md) into A and
[`cloud-agent-b.md`](cloud-agent-b.md) into B, relite the **same** names
from secrets. If you do not have passwords, those characters are gone —
new names, do not share names across lanes.

## What is in git vs not

| In git (this file’s job) | Not in git (copy privately) |
|---|---|
| Playbooks, wiki coords, A/B pastes, skills, this snapshot | API key, `bot.env`, running Cloud VMs, old chat transcripts |
| Agent **ids** and watch URLs | The key that authorizes them |

## Snapshot (2026-08-16 ~03:14 UTC)

Live tiles move. Re-fetch `/playerpositions` before you steer.

| Who | Character | Last tile | Job |
|---|---|---|---|
| **A** | `qstboot1` | Wizard Tower basement `(3104,9576)` | Restless Ghost: get skull, then graveyard coffin. Do not cancel if still on that step. Then Vampire → Waterfall (B’s kit if traded). |
| **B** | `foodprobe1` | walking Draynor `(3175,3252)` (had a controller) | Rope from Draynor muggers (~31%), park Lumbridge, trade `Qstboot1` when A is in range. Has runes. Do not invent a clean 1 HP clock. |
| **B** | `foodkill1` | Lumbridge `(3219,3218)` | Idle helper. Do not start a new PK. |
| leftover | `qstprobe1` | `(3236,3578)` | Phase 1 scout. Not a job. |

| Agent | Latest run at snapshot | Branch on origin |
|---|---|---|
| A `bc-9ef936bf-…` | `run-86475753-…` was RUNNING the skull step | `cloud/ab-a` `de0a265ba` |
| B `bc-426ffd7b-…` | `run-0e4c118a-…` force-sent: stop merging markdown, get rope | `cloud/ab-b` `4c86ddde1` |
| B2 `bc-52c85732-…` | leave idle | — |

Known jams this session (already in shared files — do not re-learn):

- Urhney is `(3235, 3153)`, not OSRS west-swamp. [`lookup.md`](lookup.md)
- Aereck start line is **"I'm looking for a quest!"** [`dialog.md`](dialog.md)
- `/status` is not HP. Unnamed death = invalid clock. [`observe-fidelity.md`](observe-fidelity.md)
- B sat in Lumbridge merging `ab-results-b.md` instead of walking. [`merge.md`](merge.md).
- A walked to Taverley at cb 3 once. Restless Ghost first. Do not send Waterfall until pray 47.

## Do not

- Local lite / `cdx*` / a third Cloud agent for the same lane
- Cows, hill raid, new bot names, guessed OSRS tiles
- Cancel a run that is already walking the right direction
- Paste this file or `operator.md` into A/B
- Commit `bots/*/` or print secrets

## First message for a new operator chat

```
You are the operator. Read learnings/operator.md and learnings/operator-handoff.md.
Do not play on this machine. Steer existing Cloud A/B with the API key in
~/.cursor/cursor_api_key (do not print it). Keep B moving. Start the 60s loop.
```
