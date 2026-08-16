# Operator only — do not paste into Cloud A/B

Audience: **you (the laptop)**. Cloud agents get a short lane paste plus
shared files they open when stuck. They must not read this file.

North star: PK kill-and-rejoin, then KOTH minutes. Do not clone goo or the
8-stack. **Keep steering.** Write-back is extra, not a substitute.

## Need to know

Always-on context stays small. Details live in files you open when the
class shows up — same idea as a skill description vs its body.

| When | Open |
|---|---|
| Who reads what | [`README.md`](README.md) |
| New Cursor account / where we left off | [`operator-handoff.md`](operator-handoff.md) |
| NPC / loc / “coords are wrong” | [`lookup.md`](lookup.md), then `wiki/npcs/`, then `.jm2` |
| Quest / shop dialog ate the wrong option | [`dialog.md`](dialog.md) |
| Walk, death, HP, killer | [`observe-fidelity.md`](observe-fidelity.md) |
| Merge conflict / sitting on results files | [`merge.md`](merge.md) |
| Quest order / XP | [`bootstrap-quest-stack.md`](bootstrap-quest-stack.md) |
| A’s current job | [`cloud-agent-a-phase2.md`](cloud-agent-a-phase2.md) |
| B’s current job | [`cloud-agent-b.md`](cloud-agent-b.md) (launch block) |

Do not paste operator notes into A/B. Do not put A’s jobs in B’s file.
Agent pastes stay **one line + a link**. New lesson → a short shared file,
not another paragraph on the paste.

A lesson that is **not pushed** does not exist on the VM. Same turn as the
write: commit + push `learnings/`, `wiki/`, `.cursor/rules/`,
`.cursor/skills/` to `GoonMachine/rs-sdk` `main`, then POST
`git pull origin main` (do not cancel a run that is already on the right
physical step — pull on the next job).

## Roles

| | Operator | Cloud A | Cloud B |
|---|---|---|---|
| Job | Tiles, streams, force-send, playbook write-back | 25× quest stack on `qstboot1` | Mule + 1 HP rejoin on `foodprobe1` |
| Do not | Lite / `cdx*` here. Third VM for the same lane. | Cows, hill, B’s bots | Cows, `foodboot1`, new names, hill, A’s quests |

## Live facts (do not re-measure)

- Tick **300.3 ms**. Quest XP **25×**. Polygon `(3288,3879)` IN / `(3288,3878)` OUT.
- PvP death → Lumbridge ~`(3222,3218)` at **1 HP**. Mark ~300s. Regen is not blocked.
- Wild: `abs(cb diff) <= min(wild)`; `wild = (z-3520)/8+1`.
- Need **cb ≥ 77** vs 123 at wild 46. Restless Ghost first. Do not stop after Waterfall.
- Phase 1 is on `origin/cloud/ab-a` and `origin/cloud/ab-b`.

## Secrets (never git, never chat, never A/B paste)

| What | Where | If you lose it |
|---|---|---|
| Cursor API key that owns Cloud A/B | `~/.cursor/cursor_api_key` (mode 600) on the operator machine. Cursor Secrets on a new login. | You cannot POST to the existing agent ids. New key = new Cloud agents. |
| `bots/<name>/bot.env` for `qstboot1`, `foodprobe1`, `foodkill1` | Cloud VM only (gitignored). Not on this laptop. | Same name + new password does **not** log into the existing character. Save into Cursor Secrets on the live VMs before they die. |

Do not commit, print, or paste passwords or the API key. The key was typed in
an old operator thread — rotate it after you copy it privately. Local
`bots/cdx*` / `agentmachine` are leftover laptop play; do not use them.

## Cursor Cloud API

Base `https://api.cursor.com`. Auth: `Authorization: Bearer $CURSOR_API_KEY`
or basic `-u "$CURSOR_API_KEY:"`. Load the key from the file; never echo it.

| Call | Use |
|---|---|
| `GET /v1/agents/{id}` | status, `latestRunId`, watch URL |
| `GET /v1/agents/{id}/runs?limit=2` | latest run statuses |
| `GET /v1/agents/{id}/runs/{runId}` | `result` text |
| `GET /v1/agents/{id}/runs/{runId}/stream` | SSE; peek last ~1k chars if the tile is stuck |
| `POST /v1/agents/{id}/runs` body `{"prompt":{"text":"..."}}` | force-send / next job |
| `POST /v1/agents/{id}/runs/{runId}/cancel` body `{}` | stop a wander |

`CREATING` ghosts often return `run_not_cancellable` — POST cancel anyway.
`409 agent_busy` → wait ~2s and retry. Do not launch a third Cloud agent for
the same lane.

These agent ids belong to the **Cursor account that created them**. A new
Cursor login cannot steer them unless it uses **that same API key**.

| | Id | Watch |
|---|---|---|
| **A** | `bc-9ef936bf-19d3-4e78-bc92-189fe6d15015` | https://cursor.com/agents/bc-9ef936bf-19d3-4e78-bc92-189fe6d15015 |
| **B** | `bc-426ffd7b-4368-4f0c-9d16-8e1a6e158d1b` | https://cursor.com/agents/bc-426ffd7b-4368-4f0c-9d16-8e1a6e158d1b |
| **B2** | `bc-52c85732-6cea-49bc-87d7-7b8ac9a25742` | dead end — leave idle |

Public demo (no auth): `https://rs-sdk-demo.fly.dev/playerpositions`,
`/status/:name` (name + tile + controller only), `/hiscores/koth`.

## Steer loop

- **60s**. Tiles + controllers + latest run. Peek the stream if the tile has
  not moved. Public `/status/:name` is name + tile + controller — not HP.
- Force-send if jammed. `CREATING` often will not cancel — POST anyway.
  Cancel a `RUNNING` wander; wait ~2s on `409 agent_busy`.
- **B never idle.** The tick a B run `FINISHED`, POST the next job.
- One controller per bot. Never commit `bots/*/` or print `bot.env`.
- One operator loop. Stop the previous chat’s 60s tick before starting another.

## Diagnose before you shove

If an agent says you are wrong, **look it up** (`lookup.md`) before sending
another nearby tile. Peek the stream for the class (wrong building, dialog
ate option 1, writing `check.ts`, merging markdown, unattributed death).
Do not cancel a run that is already walking the right direction.

## Write-back (class → file)

| Class | Write to |
|---|---|
| **lookup** | [`lookup.md`](lookup.md), `wiki/npcs/` |
| **dialog** | [`dialog.md`](dialog.md) |
| **observe** | [`observe-fidelity.md`](observe-fidelity.md) |
| **merge** | [`merge.md`](merge.md) |
| **sdk** | one line on the agent paste, or a snippet |
| **steer / comms** | this file (principle, not the tile) |

First time: force-send with live tile + source path. Second time, same
class: write the file before the next POST, then push.

## Force-send

Every POST: live `/playerpositions` tile, one source path, next physical
step, what not to research. Lite `sdk.getState()` is the HP/combat
endpoint. `/status` is not.

## 60s loop prompt

`/loop 60s` then:

```
Steer Cloud A/B. Read learnings/operator.md and learnings/operator-handoff.md.
Fetch /playerpositions + /status/qstboot1 + /status/foodprobe1 + latest A/B runs.
Peek a stream only if a tile has not moved. Force-send if jammed or B FINISHED idle.
Diagnose from lookup.md before shoving a tile. Do not cancel a run walking the right way.
If you wrote a playbook lesson, commit and push the same turn.
Do not run lite or cdx* on this machine. Do not print secrets.
```
