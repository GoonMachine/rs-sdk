# A/B results — Agent A (quest path)

Status: **Phase 1 complete** (all three Agent-A probes measured on `qstprobe1`,
2026-08-16). Phase 2 (Waterfall) not started. Do not invent Phase 2 cells.

## Phase 1

| Probe | Result | Evidence |
|---|---|---|
| Prod ms/tick | **300.3 ms/tick** | `qstprobe1`: 100 ticks / 30,025 ms wall. Confirms `Koth.ts` prod 300ms, not local 400ms |
| Cook's Assistant cooking XP before → after | **0 → 7,500** (+7,500) | `qstprobe1`, quest completed 2026-08-16. "Congratulations! Quest complete!"; `getSkillXp('Cooking')` 0 then 7,500 |
| Implied quest `xpRate` | **25×** | 7,500 / 300 base = 25. `stat_advance(cooking, 3000)` = 300 XP tenths-decoded, then ×`xpRate`. Quest XP **is** multiplied (matches `WorldConfig.ts` `xpRate: 25`) |
| Polygon inside `(x, z)` / counted? | **(3288, 3879) = IN** (also (3288,3882) interior) | Stood on the tile; `Koth.ts` `polygonContains(x+0.5,z+0.5)` at level 0 = true. Held 40s, `inZone` stayed true |
| Polygon outside `(x, z)` / counted? | **(3288, 3878) = OUT** | One tile south of the z=3879 wall; same test = false. So a single tile across the south wall flips in/out, matching `Koth.ts` |

Polygon method: re-implemented `Koth.ts` `KOTH_POLYGON` + `polygonContains`
verbatim and evaluated it at the bot's **actual** standing coords (walked with
tolerance 0). Same-edge pair on the south wall (z=3879, interior-side counts):
`(3288,3879)` true vs `(3288,3878)` false.

Live context (2026-08-16, `/playerpositions`): the **8-stack is still on the
hill** — 7 bodies on `(3284,3884)` + 1 on `(3288,3886)`:
`D0c8tdgypo, M17ikgb5kn, U642zcnw4e, Cdbova8vse, Ohkyzlosps, Tfloe12pjl,
Y8xdp1e99k, Tqckgxgj08`. No crown change fired in a 40s in-zone stand (stable
incumbency). Could **not** get an independent live capture-confirmation because
they are combat-maxed and I am combat 3 — the polygon test above is the
authoritative in-zone check (it is the exact server function).

Hazards for a low-cb scout: **Greater demons in the ruins are aggressive** and
killed `qstprobe1` once (respawned Lumbridge, kept 3 items). The 8-stack could
**not** attack me and I could not attack them: at wild ~46, `abs(123−3)=120 >
46` (`pvp_combat.rs2`). NPCs, not PKers, are the threat to a combat-3 body here.

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
