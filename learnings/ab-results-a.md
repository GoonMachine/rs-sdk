# A/B results — Agent A (quest path)

Status: **Phase 1 in progress.** Tick + Cook's Assistant measured on
`qstprobe1`. Polygon and Phase 2 not done. Do not invent the empty cells.

## Phase 1

| Probe | Result | Evidence |
|---|---|---|
| Prod ms/tick | **300.3 ms/tick** | `qstprobe1`: 100 ticks / 30,025 ms wall. Confirms `Koth.ts` prod 300ms, not local 400ms |
| Cook's Assistant cooking XP before → after | **0 → 7,500** (+7,500) | `qstprobe1`, quest completed 2026-08-16. "Congratulations! Quest complete!"; `getSkillXp('Cooking')` 0 then 7,500 |
| Implied quest `xpRate` | **25×** | 7,500 / 300 base = 25. `stat_advance(cooking, 3000)` = 300 XP tenths-decoded, then ×`xpRate`. Quest XP **is** multiplied (matches `WorldConfig.ts` `xpRate: 25`) |
| Polygon inside `(x, z)` / counted? | | |
| Polygon outside `(x, z)` / counted? | | |

Side note (not a substitute for Agent B’s board pass): `/playerpositions` at
that session start had **no players** in x∈[3280,3293], z∈[3879,3892]. Live
wins; the dated 8-stack snapshot may already be stale.

## Phase 2 (Waterfall / quest stack)

- Trainer bot:
- Login wall-clock:
- Waterfall complete? (yes/no + wedge notes):
- Attack XP / Strength XP after Waterfall:
- Minutes to Prayer 25:
- Minutes to combat ≥ 77:
- Combat / skills snapshot at stop:

## Notes

(source citations, live vs checkout drift)
