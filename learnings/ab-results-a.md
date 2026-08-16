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

## Phase 2 (quest-XP combat/prayer bootstrap on `qstboot1`)

Revised order (source-checked tenths ×25): Restless Ghost (prayer) → Vampire
Slayer (att) → Waterfall (att/str) → Witch's House / Holy Grail (def+prayer)
for the cb≥77 jump. Trainer bot: **`qstboot1`** (started combat 3, att/str 0).

### 1. The Restless Ghost — COMPLETE ✅ (2026-08-16)
- Source: `quest_priest/scripts/quest_priest.rs2` `stat_advance(prayer, 11250)`
  = 1125 base ×25.
- **Prayer XP 0 → 28,125 (+28,125)**; **Prayer level 1 → 47**. "Congratulations!
  Quest complete!". Combat 3 → **9**.
- **Unlocks Protect Item (level 25) AND Protect from Melee (level 43)** in one
  Lumbridge-area quest — exactly as planned.
- Route: Aereck (church, start) → Urhney (**hut at (3235,3153)**, SE swamp — NOT
  the (3146,3176) shack) gives Ghostspeak amulet → equip → open graveyard coffin
  spawns "Restless ghost" (npc lives ~100 ticks) → "Yep, now tell me…" →
  skull is a ground obj in the **Wizard's Tower basement at (3120,9567)** (a
  level-13 skeleton spawns on pickup) → use skull on the open coffin.

Wedges (recorded per instruction):
- **SDK pathfinding vs stale `collision-data.json`.** `bot.walkTo` could not
  approach the tower basement ladder (`reach=true` but every adjacent tile
  returned "no waypoints"), and the swamp legs wedged repeatedly. Workaround:
  low-level `sdk.sendInteractLoc(x,z,locId,op)` + `sdk.sendWalk` use the lite
  client's live collision and succeeded where `walkTo` failed. This is the key
  reusable lesson for cross-map quest navigation on this fleet.
- Dialog double-click footgun: rapid continue-clicks skipped choice menus and
  auto-picked option 1 (started us down the Saradomin branch instead of the
  quest). Fix: dedupe by dialog signature; never click the same page twice.
- Wrong Urhney coord (OSRS memory) cost several dead-end walks; the real spawn
  is in `maps/m50_49.jm2` → (3235,3153).
- `qstboot1` died twice to aggressive NPCs while idle/mispathing (kept 3 items).

Wall-clock: ~58 min first-login→prayer-47, but that is **not** a clean training
metric — it was dominated by the navigation/collision debugging above, not by
XP grind. A clean re-run with the low-level-nav workaround would be far shorter.

### 2. Vampire Slayer — pending
### 3. Waterfall — pending
- (Wall White Wolf note: Lumbridge→Baxtorian walk wedged at the mountain; needs
  the Taverley→Catherby trail, not a south pathfind past z=3400.)

## Notes

(source citations, live vs checkout drift)
