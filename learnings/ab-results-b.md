# A/B results — Agent B (food path)

Status: **partially live-probed by Codex on 2026-08-15 EDT**. Cloud Agent B
still owns the unchecked death-mark duration/NPC-death work and Phase 2. Do not
repeat the completed fixture unless testing a different branch.

Inherited from Agent A (do not re-measure): prod tick **300.3 ms/tick**, so
expect death mark **~300s** and skull **~600s**. Agent A also saw an empty
KOTH polygon once — fetch live boards anyway; live wins.

## Phase 1

| Probe | Result | Evidence |
|---|---|---|
| `/playerpositions` vs snapshot | Recurring 1+7: `Tqckgxgj08` inside at `(3288,3886)`, seven outside at `(3284,3884)`; brief all-eight-inside + `Goo001`; all 25 goo online, usually none on hill | [`live-probes-2026-08-15.md`](live-probes-2026-08-15.md) |
| `/hiscores/koth` vs snapshot | All-time `Tqckgxgj08` 1,627→1,628 during fixed 1+7; later 1,646. `Goo001` 8,510→8,513 after brief entries. Use `window=`, not `period=` | [`live-probes-2026-08-15.md`](live-probes-2026-08-15.md) |
| PvP death spawn `(x, z)` | `(3219,3219)`, Lumbridge | Self-owned `Cdxkill815` → `Cdxvict815` |
| HP after PvP death | 2/10 on first observer read; exact frame 1 missed, so the source's 1 HP remains unchecked | Do not round this to a confirmed 1 |
| Items kept / lost | Victim wore wooden shield, carried dagger + 25 arrows; kept shield + dagger + 1 arrow in inventory; killer saw 24 arrows + bones | Live proof of worn+carried unit-cost order and one-unit stack keep |
| NPC death while marked — HP on respawn | | expect 1 |
| Mark duration (ticks + wall-clock) | | checkout 1000 ticks |

## Phase 2 (rats / food)

- Trainer bot:
- Login wall-clock:
- Loop (rats / goblins / cows) + proven in 10–30s?
- XP/hour (attack / strength / defence / hp / prayer):
- Minutes to Prayer 25:
- Minutes to combat ≥ 77:
- Combat / skills snapshot at stop:
- NPC deaths:

## Notes

- The first `bot.attackPlayer` attempt falsely reported success while an NPC
  combat state overlapped a server level-difference refusal. Raw
  `sendInteractPlayer` plus target-specific combat-state evidence worked. SDK
  report: `msv4vnr5-71583b41`.
- The fresh victim had Prayer 1, so skulled/Protect Item branches were not
  live-tested. Source results remain authoritative until those probes run.
