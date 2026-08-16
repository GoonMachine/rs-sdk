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

## Mule + rejoin lane (2026-08-16, Cloud Agent B)

foodprobe1 reused as mule/rejoin runner (cb 13, HP 18, junk only: Shortbow, Bronze
sword, Wooden shield). foodkill1 offline. Agent A owns quest combat.

### Step 1 — Waterfall kit: **COMPLETE (6 air + 6 water + 6 earth + rope)**
Started at 0 gp. Funded by **pickpocketing Lumbridge men** (`Man` cb2 at the castle):
thieving **1 → 29**, ~**100 gp in ~2.5 min** (~3 gp/success; stuns frequent at low level,
fall off fast). Walked to **Betty's Magic Emporium, Port Sarim `(3012,3259)`** and bought
**6 air + 6 water + 6 earth** runes @ 4 gp = **72 gp** (30 gp left). Kit inv now:
`Air rune x6, Water rune x6, Earth rune x6` (+ junk Shortbow/Bronze sword/Wooden shield).
- **Rope = ACQUIRED.** Looted from a **Barbarian Village Mugger** (cb6, rope 40/128).
  Muggers *flee* hard: `bot.attack` returns on engagement, not kill, so a naive loop got
  0 ropes in 55 "engagements". The working loop kill-confirms (npc index disappears) and
  sweeps ground drops — 1 rope after ~140 attacks / 2 confirmed kills.
- **Cleaner rope source for next time:** `Ned` at Draynor `(3100,3258)` **sells rope for
  15 gp** ([`ned.rs2`](../server/content/scripts/areas/area_draynor/scripts/ned.rs2)) — buy
  it, don't grind muggers. (The distant general stores in `wiki/items/rope.md` are all
  Kandarin/Karamja; the `(3018,3185)` mugger spawn is unpathable from Draynor.)

### Step 2 — Trade to Qstboot1: **COMPLETED (kit delivered)**
**Hand-off done 2026-08-16 ~04:09 UTC.** foodprobe1 served the trade at Lumbridge and
gave `Qstboot1` the full Waterfall kit — **Rope x1, Air rune x6, Water rune x6, Earth
rune x6** (pure gift, received nothing). `bot.trade` result: `success=true`.
- Timeline: Qstboot1 finished Vampire Slayer (was in Draynor Manor crypt `(3077,9774)`),
  walked to Lumbridge, entered range at `(3230,3227)` d=14; foodprobe1 (parked at
  `(3235,3213)`) auto-initiated and the trade completed at `(3232,3223)`.
- Earlier attempts held: two prior serve windows found Qstboot1 out of range (Restless
  Ghost / crypt). Controller **never** touched — foodprobe1 initiated from its own side.
- Mule pipeline proven end-to-end: 0 gp → pickpocket men → Betty runes → mugger rope →
  serve trade to the quest bot. Ned (`(3100,3258)`, 15 gp) is the faster rope source next.
- **Post-handoff:** foodprobe1 stays mule, ready to ferry food / replacement kits.

### Step 2b — RESUPPLY after Agent A's White Wolf death (2026-08-16 ~04:26 UTC)
Agent A (`Qstboot1`) died on **White Wolf Mountain** and **lost the Waterfall runes** (kept
the rope). Re-supplied: re-funded via Lumbridge men pickpocket (thieving 29, +102 gp/90s →
132 gp), re-bought **6 air + 6 water + 6 earth** at Betty's (72 gp), then **traded runes to
Qstboot1 at Port Sarim `(3013,3260)`** (it walked to meet foodprobe1; `bot.trade`
success=true, gave 6/6/6, Qstboot1 kept its rope). Full kit restored. Mule pipeline
survives a loss: pickpocket → Betty → trade, ~few min turnaround. Controller not touched.
- Note: **Al Kharid kebab run wedged** at the toll gate — the border-gate toll needs a
  3-step dialog ending in `p_choice3` "Yes, ok." (`border_gate.rs2`); `interactLoc(gate)` +
  `navigateDialog` did not advance it, foodprobe1 stuck at `(3267,3227)`. No cheap-food
  shop exists in Lumbridge/Draynor (source-checked); kebabs (1 gp, `kebab_seller.rs2`
  Al Kharid 3272,3182) need working gate-toll dialog handling.

### Step 2c — Mule food stocked + parked for re-kit (2026-08-16 ~04:29 UTC)
Cheap-food source that works (no Al Kharid): **Wydin's Food Store, Port Sarim `(3014,3204)`**
(`wydin.rs2` → `[foodshop]` inv). Bought **3 cheese, 3 banana, 3 cabbage, 1 chocolate bar**
(10 items; per-item stock is only 1–3 so a single visit is small, restocks over time),
~31 gp. foodprobe1 now **parked at Draynor `(3092,3245)`** with 29 gp + 10 food — central
for re-kit (Betty runes at Port Sarim, Ned rope at Draynor `(3100,3258)`, short hop to the
quest corridor). Ready to re-supply Qstboot1 again if A dies. HP full → no 1-HP clock.

### Step 2d — kitprep1 warehouse: iron chainbody banked (2026-08-16 ~05:5x UTC)
Second mule `kitprep1` (one extra name, this VM). Funded by men pickpocket **with eating/
resting** (fresh account dies from stun damage otherwise — a death drops the low-`oc_cost`
coin stack; kitprep1 lost its starter food/net that way once). No-food rest-to-regen loop
is self-sufficient but slow (~90 gp/260s early, accelerates as thieving climbs; hit ~41→48).
Live **Horvik `(3229,3438)`** prices (read before buying, per scarce-goods): Iron chainbody
**210 gp** (stock 3), Iron platelegs **280 gp** (stock 1), Iron platebody 560, Steel chain 750.
Bought **1 iron chainbody (210 gp)** → **banked at Draynor `(3092,3245)`**.

### Step 2e — iron platelegs bought + banked (kit complete)
Dwarven Mine descent works via the surface **Trapdoor `(3016,3441)` → underground
`(~3037,9846)`** (Drogo's Mining Emporium: bronze pickaxe 1 gp stock 4, buys iron ore 11 gp
depleted / coal overstocked 1026). Skipped the mine-to-15 grind per operator: with **298 gp**
on hand, climbed back out the Trapdoor and **bought Iron platelegs at Horvik `(3229,3438)`
for 280 gp** (stock 1), then **banked at Draynor**. kitprep1 now holds **iron chainbody +
iron platelegs banked at Draynor**, 18 gp left. **Iron quester kit (chain+legs) complete** —
trade to `qstboot1` when both are at Draynor. Iron sword `(3203,3397)` 91 / Zeke scim
`(3288,3190)` 112 still pending funds. `foodkill1` idle; `foodprobe1` death-watch mule at
Draynor (104 gp + food). Note: `buyFromShop`/`sellToShop` need a `waitForTicks(2)` after
`openShop` or `shopItems` is empty and the buy false-fails "out of stock".

### Step 2f — iron sword + food warehouse: both wedged (kitprep1 too fragile)
- **Iron sword:** Varrock sword shop `(3203,3397)` opens by **dialog** ("Yes, please!"),
  keepers are generic "Shop keeper"/"Shop assistant" (no direct Trade option). Iron sword
  ~91 gp, stock ~4 (not overstocked). kitprep1 had **18 gp** → unaffordable; deferred (no
  pickpocket lane).
- **Food warehouse (shrimp):** bought a Small fishing net at Gerrant `(3013,3225)`, but
  kitprep1 **died en route to the Draynor fishing spot** `(3087,3230)` — respawned Lumbridge
  with 0 coins and no net (a death drops the low-`oc_cost` net + coin stack, same failure as
  the earlier stun-death). No shrimp fished/cooked.
- **Root issue:** `kitprep1` is a fresh ~10-HP / low-cb body with **no food**; every
  multi-hop errand risks a death that wipes its coins/consumables. It cannot self-sustain a
  fish→cook→bank loop or hold GP. Recommend: either give it a small HP/combat buffer + food
  first, or accept its warehouse = the **iron chainbody + platelegs already banked at
  Draynor** (the quester kit is done). `foodprobe1` (104 gp + food, cb 13) is the robust mule.
- kitprep1 now: Draynor bank `(3092,3245)`, **0 gp**, iron chain+legs still banked.

### Step 2g — iron sword DELIVERED via buyer-body (kit complete: chain+legs+sword)
Fixed by using the **sturdy foodprobe1** (cb 13, 104 gp + food) as the buyer instead of the
fragile kitprep1. foodprobe1 walked to the Varrock sword shop `(3203,3397)`, bought **Iron
sword for 91 gp** (13 gp left), walked to the Draynor bank tile, and **traded it to kitprep1**
(`bot.trade` success); kitprep1 **banked it**. Full **iron quester kit — chainbody + platelegs
+ sword — now banked at Draynor** for `qstboot1`. Mechanics notes: the sword-shop keepers
("Shop keeper"/"Shop assistant") DO expose a **Trade** option but only when you are **adjacent
(d≤1)** — earlier opens failed on reach/position, not the dialog; advance chat lines with
`sendClickDialog(0)` if a dialog interstitial appears. Bot→bot handoff pattern that works:
receiver runs `bot.serveTrades({from, want, maxTrades:1})` in the background, buyer runs
`bot.trade(receiver, {give})`; receiver then banks. `foodprobe1` is the errand body,
`kitprep1` the stationary bank holder (never send kitprep1 across the map).

### Step 2h — foodprobe1 food stack banked for the 1 HP corridor
Opened Wydin `(3014,3204)` first: cheese **4 gp** (not the hoped 1 gp overstock; stock 3),
cabbage 1 gp, banana 2 gp. With ~13 gp foodprobe1 bought the cheap edibles it could and
**banked a ~14-item food stack (cheese/banana/cabbage/chocolate) at Draynor** for a future
1 HP rejoin corridor. foodprobe1 now 0 gp, food banked; `kitprep1` stationary on the bank
tile (0 gp, holds iron chain+legs+sword); `foodkill1` idle.

### Step 2i — Falador findsomethingnice search: poor gp source, skip it
Walked foodprobe1 to the SE Falador house `(3035,3335)` (confirmed: Range + Crate +
Drawers on the floor — the documented "range + treasure chests" house). Searched furniture
**44 times → 0 finds, 0 gp**. Source math (`findsomethingnice.rs2` + `_chance=10`):
**1/10** to find anything, and only **case 3 of random(4)** is coins (1–10) → **~1/40 per
search for ~5 gp**, the rest gloves/boots/pot/broken junk. Even when it fires it's a
terrible gp rate, and the drawers-search interaction may not register cleanly via
`interactLoc`. **Recommendation: do not use findsomethingnice for gp** — it is far slower
than any shop-overstock buy or a single kill-and-pickup. foodprobe1 back at Draynor, 0 gp,
food stack still banked.

**Codex parallel probes (2026-08-15, preserved from main):** live-probes recorded PvP
spawn `(3219,3219)` at 2/10 HP (frame 1 missed); a death-keep fixture (worn shield +
dagger + 25 arrows → kept shield+dagger+1 arrow, 24 dropped); `bot.attackPlayer` once
falsely reported success over a level-difference refusal (SDK report `msv4vnr5-71583b41`),
raw `sendInteractPlayer` + combat-state worked; victim had Prayer 1 so skulled/Protect-Item
death branches remain source-only. See [`live-probes-2026-08-15.md`](live-probes-2026-08-15.md).

### Step 3 — Full-HP rejoin clock (junk only) ✓
Route: Lumbridge `(3222,3219)` → `(3335,3528)` → `(3334,3650)` → `(3334,3769)` →
`(3335,3870)` → **STOP `(3303,3878)`** (ruins approach, wild ~45).

- **Total wall-clock: 2.60 min (156.1s). Deaths: 0. HP at stop: 18/18 (full — labelled
  full-HP, not 1 HP).**
- Legs: WP0 80.5s (Lumbridge→corridor entrance incl. the one-time wilderness-warning
  crossing), WP1 +26.1s, WP2 +24.1s, WP3 +18.6s, WP4 +6.8s.
- Did **not** enter greater demons (last ~19 tiles), `(3284,3799)` spiders, or the hill.
- Run energy is infinite here, so walk time is **HP-independent**: the real 1-HP post-PK
  rejoin takes the same **~2.6 min** — only survivability differs. **This is the rejoin
  baseline to beat after a PK.** The maxed 8-stack (cb ~123) cannot attack a cb-13 runner
  in this corridor (cb-diff 110 > wild 45), so the walk itself is low-risk; mid-cb PKers
  are the threat.
- foodprobe1 left parked at `(3303,3878)`.

### Step 3b — 1-HP clock attempt: **NOT cleanly captured (unattributed deaths)**
Retries to get a MARKED (1-HP) respawn failed, verified via the death-tracking state
(`respawnCount`/`lifeId`/`lastDeathTick`), not walkTo "success":
- foodkill1 reached the staging tile to find `victim=NONE` — foodprobe1 had already died
  to a wild NPC/PKer while idle and respawned **unmarked at full HP** (no mark → full HP,
  not 1). The corridor therefore re-ran at **full HP (START 19/19)**, i.e. redundant with
  the 2.60-min baseline (clocked 2.62 min).
- Death-tracking dump (foodprobe1 now back at Lumbridge (3221,3219), tick 4850):
  `respawnCount=3, lifeId=4, lastDeathTick=4481, isDead=false, inCombat=false`; two
  `Oh dear you are dead!` game lines at ticks **3747** and **4481**. The runner **died
  again at/after the (3303,3878) stop** (deep wild ~45) and respawned Lumbridge.
- **Conclusion:** a lone cb-14 junk runner does **not** survive parked at the ruins
  approach — it is killed in deep wilderness. So the realistic 1-HP rejoin is NOT a clean
  2.6-min walk-and-stand; a rejoiner needs escort/timing or a shallower hold tile. Treat
  **~2.6 min as the movement lower bound only**, not a survivable 1-HP rejoin. Correct
  death signal for future clocks = `respawnCount`/`lifeId` deltas (walkTo success is not).
