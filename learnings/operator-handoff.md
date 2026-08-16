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

## Snapshot (2026-08-16 ~05:45 UTC)

Live tiles move. Re-fetch `/playerpositions` before you steer.

| Who | Character | Last tile | Job |
|---|---|---|---|
| **A** | `qstboot1` | Falador-south `(2939,3314)`, no controller | Ghost + Vampire done. Waterfall mid (`opened_book_on_baxtorian`) but **10 HP death spiral** in Golrie. **Witch’s House cut-in** (gloves + cheese). att 68 / str ~1. Never Accurate. |
| **B** | `foodprobe1` | Draynor `(3092,3243)` | Reactive mule. Betty + cheese + trade if A lumbs. Do not follow west. |
| **B** | `kitprep1` | Lumbridge `(3218,3218)` | Warehouse: gloves if A needs them, then Horvik iron. |
| **B** | `foodkill1` | Lumbridge `(3219,3218)` | Idle until a real 1 HP clock. |

Known jams (already in files — do not re-learn):

- Urhney `(3235, 3153)`. Aereck **"I'm looking for a quest!"**
- `/status` is not HP. [`observe-fidelity.md`](observe-fidelity.md)
- Golrie ladder `(2533, 3155)`. Elkoy is not an escort until TGV started.
- 10 HP + Golrie = Lumbridge loop. HP via Witch’s House, not a def grind.
- Vampire overtrains **attack** only. Strength = Waterfall. Never Accurate.
- B pickpocket must not starve a death-watch / quest-item trade.

## Do not

- Local lite / `cdx*` / a third Cloud VM
- Cows, hill raid, guessed OSRS tiles, random new names
- Extra lite on B’s VM except `kitprep1` after [`scarce-goods.md`](scarce-goods.md) + a POST
- Cancel a run that is already walking the right direction
- Paste this file or `operator.md` into A/B
- Commit `bots/*/` or print secrets

## First message for a new operator chat

```
You are the operator. Read learnings/operator.md and learnings/operator-handoff.md.
Do not play on this machine. Steer existing Cloud A/B with the API key in
~/.cursor/cursor_api_key (do not print it). Keep B moving. Start the 60s loop.
```
