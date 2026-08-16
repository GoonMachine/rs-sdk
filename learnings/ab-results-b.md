# A/B results — Agent B (food path)

Status: **Phase 1 complete** (Cloud Agent B). Prod tick **300.3 ms/tick**
inherited from Agent A (not re-measured); death mark measured **~300s** (1000 ticks).

Live boards fetched **2026-08-16 ~01:1x UTC** (`rs-sdk-demo.fly.dev`).

## Phase 1

| Probe | Result | Evidence |
|---|---|---|
| `/playerpositions` vs snapshot | **Hill OCCUPIED now — live wins over Agent A's "empty polygon" lead.** All 8 snapshot names present, stacked at ~(3287,3885) with `Tqckgxgj08` on the crown tile (3288,3886). `Goo001` adjacent at (3276,3875). 15 `goo` accts stacked underground at (3507,9496) lvl2 (parked, as in snapshot). 899 players online total. | 8/8 of the 2026-08-15 stack co-located inside the ruins; `Goo001` the only `goo` at the hill; other `goo` scattered (Lumbridge/mine), 15 parked underground |
| `/hiscores/koth` vs snapshot | All-time board: **#1 `goo001` 8,509 min (4m ago), #2 `tqckgxgj08` 1,627 min (just now)** — both actively capturing. #3 `palmerluckey` 1,275 (11d, idle). 8-stack members on board: `tfloe12pjl` 63, `y8xdp1e99k` 45 (3m ago), `cdbova8vse` 29 (1h). vs snapshot all-time `Goo001` ~8,503 → +6; `Tqckgxgj08` weekly ~1,605 → 1,627. | `goo001`/`tqckgxgj08` "ago" = just now/4m → live capture in progress. Today/Week tab HTML renders identically server-side (period toggled client-side); all-time numbers used |
| PvP death spawn `(x, z)` | **Lumbridge (3222, 3218)** | Victim respawned at (3222,3216)/(3220,3219) after each PvP death; matches `p_teleport(map_findsquare(0_50_50_21_18…))` in [`death.rs2`](../server/content/scripts/player/scripts/death.rs2) |
| HP after PvP death | **1** | `death.rs2` sets `stat_sub(hitpoints … 1)` when `$pvp_death`. Live: victim seen at Lumbridge post-PvP-death then regening (6/18 at ~+120s); identical 1-HP path confirmed clean on the marked NPC respawn below (1/18) |
| Items kept / lost | **kept 3, lost 15** | First PvP death took inventory 18 → 3. KEPT: Shortbow, Bronze sword, Wooden shield. LOST (dropped): Tinderbox, Small fishing net, Shrimps, Bucket, Pot, Bread, Bronze axe, Bronze pickaxe, Bronze dagger, Bronze arrow, Air/Mind/Water/Earth/Body runes (15). Unskulled = 3 highest configured-cost (`move_priciest_item_on_hero_to_death` ×3), no Protect Item |
| NPC death while marked — HP on respawn | **1** (clean) | foodprobe1 marked at PvP T0=02:08:24Z; died to **Dark wizard (cb20)** at 02:11:18Z = **T0+174s (<300s)**; respawned Lumbridge (3222,3218) at **HP 1/18**. Detected by teleport, HP read immediately. Mark blocks NPC-suicide full-heal ✓ |
| Mark duration (ticks + wall-clock) | **1000 ticks ≈ 300.3s** | `^pvp_death_mark_duration = 1000` ([`pvp.constant`](../server/content/scripts/skill_combat/configs/pvp.constant)) × 300.3 ms/tick (Agent A) = 300.3s. Live bracket around one PvP death (T0=02:08:24Z): NPC death at **T0+174s → HP 1/18 (marked)**; NPC death at **T0+510s → HP 18/18 (full, expired)**. Mark active <300s, gone after — consistent with 300s |

**Contrast (same victim, same killer = Dark wizard cb20, same Lumbridge respawn):**
- Marked (T0+174s): respawn **1/18 HP** → cannot NPC-suicide to heal.
- Unmarked (T0+510s): respawn **18/18 HP** → normal full-HP NPC respawn.

## Phase 2 (rats / food)

**Not started this session** — Phase 1 only per the launch contract (no `foodboot1`,
no cow loop). Deferred to a later run.

## Notes

- **Wild-attack rule is live-verified.** `pvp_level_check` in
  [`pvp_combat.rs2`](../server/content/scripts/skill_combat/scripts/pvp/pvp_combat.rs2)
  requires `min(wilderness_level(both)) ≥ 1` **and** `≥ abs(cb_diff)`.
  `wilderness_level = (z − 3520)/8 + 1` from
  [`wilderness_zones.dbrow`](../server/content/scripts/areas/area_wilderness/configs/wilderness_zones.dbrow)
  (zone south z=3520, x 2944–3391; no physical ditch in this 2004 revision, only a
  one-time `wilderness_warning` modal). Repeated test-deaths inflated the victim's
  combat via HP XP, so the two accounts drifted to cb 11 vs cb 7 (diff 4). At **wild 1
  (z=3524) the attack was silently refused** ("Your level difference is too great!",
  no packet). Moving both to **wild 4 (z=3548)** made diff 4 legal and the kill landed.
- **Death detection** used the Lumbridge respawn teleport (z→~3218) and HP==1, not the
  chat log — the game-message buffer retains stale "Oh dear you are dead!" lines and
  false-positives.
- **Keep order is configured cost, not barter.** The 3 kept items (Shortbow, Bronze
  sword, Wooden shield) are just the 3 highest `oc_cost` of the starter kit; confirms
  the [`death.rs2`](../server/content/scripts/player/scripts/death.rs2) unskulled
  3-slot rule and that "protect the priciest GE item" logic does **not** apply here.
- **Boards (Phase 1.1):** hill is **occupied now** — all 8 of the 2026-08-15 stack are
  co-located at ~(3287,3885) with `Tqckgxgj08` on the crown tile and `Goo001` adjacent;
  both actively capturing (`goo001` 8,509 min "4m ago", `tqckgxgj08` 1,627 "just now").
  15 `goo` accounts parked underground at (3507,9496). This **supersedes Agent A's
  "empty polygon" lead** — live wins.
- Death test used **two of our own junk accounts in low wild (Edgeville north)**, never
  the hill, never the Demonic Ruins, never the live 8-stack/goo.
