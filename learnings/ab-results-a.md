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
Current: **Prayer 47, Attack 68, combat 30** after quests 1–2.

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

### 2. Vampire Slayer — COMPLETE ✅ (2026-08-16)
- Source: `quest_vampire.rs2` `stat_advance(attack, 48250)` = 4825 base ×25.
- **Attack XP 0 → 120,895 (+120,625 quest + ~270 combat)**; **Attack level 1 → 68**.
  Combat 9 → **30**. Matches the planned att 68.
- Kit (all self-acquired): sold Shortbow+Wooden shield at the Lumbridge general
  store → coins → **Hammer** (1gp); **Garlic** from a cupboard **upstairs** in
  Morgan's house, Draynor (`Search` after `Open`); **Stake** from Dr Harlow at
  the **Jolly Boar Inn (~3277,3492)** after buying him a **beer** (bartender in
  the same inn). Morgan `(3098,3268)` → "Ok, I'm up for an adventure.".
- Fight: opened the coffin in the **Draynor Manor basement** (Stairs at
  `(3115,3357)` down; coffin/Count spawn `(3078,9774)`). **Protect from Melee**
  (Prayer 43, from quest 1) held me at 10/10 HP; **garlic** dropped Count
  Draynor's defence to ~0 so attack level 1 could still land hits; on 0 HP the
  server auto-drove stake+hammer → complete. Once engaged, combat auto-continues
  even without controller input, so the kill finished on its own.

Wedge (recorded): an **idle-death between scripts** — a **Black Knight (lvl 33)**
in the Jolly Boar Inn killed the parked bot, dropping Hammer + Garlic + coins
(kept 3 items). Recovery: sold a ground-spawn Bronze med helm for coins, re-bought
the hammer, re-grabbed garlic (cupboard respawns). Lesson: **do not leave the
trainer parked next to an aggressive high-level NPC between scripts.** Dropping
the spare Ghostspeak amulet kept stake+hammer inside the 3-item death-keep.

### 3. Waterfall — IN PROGRESS (state: `opened_book_on_baxtorian`)
- Almera (start) → Hudon (raft-landing forced dialogue → `spoken_to_hudon`) →
  rope crossing (Rock#1996 low-level `sendUseItemOnLoc` + walk; dead tree #2020
  → ledge; Ledge#2010 flood-door → washed to `(2527,3413)`) → **read Book on
  Baxtorian** (Hadley's upstairs bookcase #380). No att/str yet (reward is on
  completion).
- **White Wolf solved**: hold **Protect from Melee** across the wolf segment
  (Taverley→Catherby trail, z>3400). Runes re-muled from `foodprobe1` at Betty
  `(3012,3259)`.
- **Blocker — 10-HP fragility.** `qstboot1` HP is level 10. It died in the
  **Golrie cave** (goblins) and to **White Wolf** wolves; each death respawns at
  Lumbridge and dropped the muled runes (lost twice). The **Fire Giant dungeon**
  ahead is worse. Golrie cave-down = loc_1754 `(2533,3155)` → underground Golrie
  `(2515,9581)`.

### 3b. Witch's House (`quest_ball.rs2`) — cut in for the HP dump
- Rationale: `stat_advance(hitpoints, 63250)` = **+158,125 HP** (→ ~HP 65) makes
  the caves / Fire Giants survivable, breaking the death cycle before finishing
  Waterfall. Nora T. Hagg's house ~`(2930,3463)`, boy at `(2927,3455)`.
- Needs: door key (under flower pot, free), magnet (cupboard, free), **cheese**
  (Wydin, Port Sarim `3014,3204`), **worn leather gloves** (Thessalia, Varrock
  `3204,3417`) for the iron gate, then mouse+magnet → shed key (fountain) → shed
  door → kill shapeshifter (Aggressive/Strength style, Prot Melee) → grab ball.
- Blocked pinning the exact witch-house interior loc tiles (pot/door/cupboard);
  generic name-matching grabbed wrong far-away locs.

### 4. Tree Gnome Village quest — after Waterfall
- (Wall White Wolf note: Lumbridge→Baxtorian walk wedged at the mountain; needs
  the Taverley→Catherby trail, not a south pathfind past z=3400.)

## Notes

(source citations, live vs checkout drift)
