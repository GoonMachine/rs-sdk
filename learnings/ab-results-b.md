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

### Step 2j — chicken kill-and-pickup: 0 gp (chickens are not a coin source)
foodprobe1 withdrew 2 food, killed chickens at `(3237,3295)` (Farmer/Cow/Chicken pen) and
picked up **8 raw chicken + 50 feathers**. But **0 coins**: chickens drop **no coins**
(only bones/raw chicken/feathers), and **Wydin buys raw chicken for 0 gp** ("Sold Raw
chicken x8 … coins=0"). Feathers are the only sellable loot and need the **Gerrant fishing
shop `(3013,3225)`** — banked the 50 feathers as a tradeable good instead (marginal ~few gp
if sold). No coins cleared → no cheese bought. **Lesson: for non-pickpocket kill-and-pickup
GP, kill MEN (drop 3–15 coins), not chickens** — chicken loot is feathers/bones, not gp.
foodprobe1 at Draynor, 0 gp; bank now holds its food stack + 50 feathers + raw chicken.

### Step 2k — feathers → gp → cheese (the chicken loot did convert)
Withdrew the 50 feathers, opened **Gerrant's Fishy Business `(3014,3224)`**: live feather
**sellPrice = 1 gp** (>0, despite the shop holding ~1000 — the floor here is 1, not 0).
Sold **50 feathers → 29 gp** (price decays as stock rises, ~0.58 gp/feather avg), then bought
**3 cheese at Wydin `(3014,3204)`** (~12 gp) and **banked them**, leaving **17 gp**. So the
full chicken→feather→Gerrant→cheese chain does yield a little gp + food — but it's marginal
(~a whole chicken-kill session for ~29 gp). Men-coin drops remain the better non-pickpocket
GP if speed matters. foodprobe1 now **17 gp**, bank food stack expanded (+3 cheese);
`kitprep1` unchanged (iron kit banked); `foodkill1` idle.

### Step 2l — spent 17 gp on Wydin food, banked
foodprobe1 opened Wydin `(3014,3204)` and spent its 17 gp on **~5 food (3 cheese @4 + cheap
cabbage/banana)** — cheese stock caps at 3/visit — then **banked the stack**. foodprobe1 now
**0 gp**, bank food stack larger. `kitprep1` still holds the iron kit on the bank tile;
`foodkill1` idle. (Warehouse-food loop is stable; gp is fully converted to banked food.)

### Step 2m — Betty's rune prices recorded (Waterfall re-supply reference)
Recon at Betty's Magic Emporium `(3012,3259)`, 2026-08-16: **Air / Water / Earth runes each
4 gp, stock 1000** (fully stocked; also Fire 4, Mind 3, Body 3, Chaos 15, Death 30). So a
full **Waterfall kit re-buy = 6+6+6 runes × 4 gp = 72 gp**, always in stock — the re-supply
cost never spikes on stock. Rope is separate (Ned 15 gp / mugger drop). foodprobe1 has 0 gp
so this was record-only; it walked back to the Draynor bank tile. Warehouse is sufficient:
`kitprep1` holds the iron kit, `foodprobe1` holds the food stack, both parked at Draynor.

### Step 2n — Ned rope price confirmed live: 15 coins, unlimited
Talked to `Ned` at Draynor `(3100,3258)`. Rope is a **fixed 15-coin dialog sale**, **not** a
stocked shop — so there is **no stock cap and no price decay**: unlimited rope at 15 gp each
(`ned.rs2` hardcodes `inv_del(coins,15)`; the live dialog reached the "Okay, please sell me
some rope / That's a little more than I want to pay / I will go and get some wool" choice that
follows Ned's "sell you some rope for 15 coins" line). Alt: 4 balls of wool → 1 rope (grind).
foodprobe1 had 0 gp so it declined and returned to the Draynor bank tile. **Waterfall
re-supply reference is now complete: runes 72 gp @ Betty (stock 1000), rope 15 gp @ Ned
(unlimited).**

### Step 2o — Lumbridge→Draynor corridor rehearsal (safe, food): ~1 min, 0 deaths
Timed the **safe** resupply/regroup route with 8 food carried: **Lumbridge respawn
`(3222,3218)` → Draynor bank `(3092,3243)` = 58.6 s (0.98 min)**, `reached=true`,
**died=false** (verified by unchanged `lifeId=1 / respawnCount=0`, per observe-fidelity — not
walkTo success). This is the friendly-side rejoin: a body that respawns at Lumbridge reaches
the Draynor warehouse/bank in ~1 min with no hazard. (Contrast: the deep-wild ruins-approach
corridor to `(3303,3878)` was ~2.6 min movement but lethal to a lone cb-low body.) foodprobe1
banked its leftover food and stays on the Draynor bank tile.

### Step 2p — safer northern path (north of z=3248): slower, hazards still at the bank
Re-ran Lumbridge `(3222,3218)` → Draynor bank `(3092,3243)` routed **north of z=3248**
(above the willow/wizard strip): **75.5 s (1.26 min)** — **slower** than the direct 58.6 s
(the northern detour costs ~17 s). died=false (food carried, cb ~20), but foodprobe1 still
took ~2 dmg (HP 22→20) because the **Draynor bank approach itself is guarded**:
- **Jail guard cb26 @(3101,3238)**
- **Dark wizard cb7 @(3084,3237)**
Both sit at z~3237-3238 right beside the bank `(3092,3243)`, so a **1-HP rejoiner cannot
fully avoid them** — the jail guard (cb26) in particular can two-shot a 1-HP body. Takeaway:
the friendly rejoin is ~1 min direct, but the last ~10 tiles into Draynor bank pass the
jail/wizard; a 1-HP body wants food up before that approach (or use a different bank).
foodprobe1 banked food, stays on the bank tile.

### Step 2q — Falador IS the safe 1HP bank (Draynor is not)
Tested **Falador bank `(2945,3366)`**: Draynor→Falador = **92.3 s (1.54 min)** (farther), but
the **approach is hazard-free** — **0 combat NPCs within 12 tiles of the bank**, foodprobe1
took **0 damage** (HP 25→25, no death), bank **opens** fine. Only combat NPCs seen were the
Draynor Dark wizard (at the start) and a harmless **Imp cb2 @(2931,3372)** near Falador.
**Verdict: use FALADOR as the 1HP rejoin/resupply bank** — its last tiles are safe, whereas
Draynor's last ~10 tiles have a **Jail guard cb26** (two-shots 1 HP) + Dark wizard. Cost is
~90 s extra travel, cheap insurance for a 1-HP body. Kit warehouse stays at Draynor;
route 1-HP banking to Falador. foodprobe1 returned to the Draynor bank tile.

### Step 2r — Falador bank now stocked with food (1HP rejoin ready)
Moved the food stack to the safe bank: withdrew **22 food** at Draynor, walked to **Falador
bank `(2945,3366)`**, deposited it, and confirmed the bank (opened at the Falador booth) now
holds **22 food** (`falBankFood=22`, invFoodLeft=0), then returned to Draynor. (RS bank is
shared across booths, so this stocks the one bank reachable from the hazard-free Falador
approach.) **A 1-HP body respawning at Lumbridge can now walk to Falador (safe last tiles)
and withdraw ~22 food** without passing Draynor's jail guard. kitprep1 unmoved (iron kit at
Draynor); foodprobe1 back on the Draynor bank tile; foodkill1 idle.

### Step 2s — the real 1HP rejoin corridor: Lumbridge → Falador bank = 2.1 min, safe
Timed **Lumbridge respawn `(3222,3218)` → Falador bank `(2945,3366)` = 126.0 s (2.10 min)**,
`reached=true`, **died=false**, and foodprobe1 took **0 damage** (HP 21→25, i.e. it *regened*
+4 en route — no hostile hit). Combat NPCs seen: Man/Woman/Rat at the Lumbridge start
(harmless), and only a **non-aggressive Guard cb21 @(2949,3380)** at the Falador bank (no
damage). **This is the safe 1HP rejoin: ~2.1 min Lumbridge→Falador vs ~1 min to Draynor, but
Draynor's last tiles two-shot a 1HP body (jail guard cb26) and Falador does not.** With
30 food now banked at Falador, a rejoiner: respawn Lumbridge → 2.1 min walk (safe) → withdraw
food at Falador. foodprobe1 banked its 8 food at Falador (now ~30 there) and returned to
Draynor. kitprep1 unmoved.

### Step 2t — warehouse split set: iron @ Draynor, food @ Falador (1HP receive point)
Final standing layout:
- **Draynor bank — `kitprep1`** (stationary): holds the **iron quester kit (chainbody +
  platelegs + sword)** for `qstboot1`. Do not walk kitprep1.
- **Falador bank `(2945,3366)` — `foodprobe1`** (now parked here, HP 25): holds **4 food in
  inventory** + **~30 food banked** at the safe Falador booth — the **1HP receive point**. A
  body respawning at Lumbridge walks the safe 2.1-min corridor to Falador and tops up food
  from foodprobe1 / the Falador bank (no Draynor jail-guard hazard).
- `foodkill1` idle.
This splits the two warehouses across the map's two nearest banks so a 1HP rejoin has safe
food at Falador while the combat kit stays banked at Draynor.

### Step 2u — no Falador prayer altar (scouted); use Lumbridge church (3243,3210)
For A's prayer restore after Witch's House: scouted **10 Falador-area spots** for an `Altar`
loc — both banks `(2945,3366)`/`(3018,3356)`, the church/statue area `(2995-3010,3383-3387,
Statue of Saradomin)`, the **White Knights' Castle** `(2976,3346)/(2993,3340)`, and the
**Edgeville Monastery ground floor** `(3059,3484)/(3053,3495)`. **No altar found** anywhere at
ground level — Falador has **no accessible prayer altar** in this checkout (the monastery has
a ladder, so its altar is likely upstairs, unverified). **Nearest confirmed F2P prayer altar
= Lumbridge church `~(3243,3210)`**, which is right on the 1HP Lumbridge-respawn corridor —
recommend A restore prayer there (or Varrock church), not Falador. foodprobe1 returned to the
Falador bank `(2945,3366)` and stays as the 1HP receive point.

### Step 2v — Lumbridge church altar LIVE-CONFIRMED at (3243,3205)
Walked to the Lumbridge church and found the **`Altar` at exactly `(3243,3205)`** (option
**"Pray-at"**). Demonstrated a real restore: drained prayer to 0 (message *"You have run out
of prayer points, you must recharge at an altar."*), then **Pray-at the altar → *"You recharge
your prayer points."*** — points back to max. So **A's prayer-restore point after Witch's House
= Lumbridge church altar `(3243,3205)`**, F2P, no requirement, right by the Lumbridge respawn/
1HP corridor. foodprobe1 returned to the Falador bank `(2945,3366)` and stays as the 1HP
receive point.

### Step 2w — monastery upstairs: ladder gated, no accessible altar (thread closed)
Resolved the leftover thread. Edgeville Monastery ladder at **`(3057,3483)`** (option
**"Climb-up"**): the climb **failed** — foodprobe1 stayed level 0 via both `bot.interactLoc`
and raw `sendInteractLoc(opIndex 1)`. This matches the classic Edgeville Monastery: the 1st
floor requires **Prayer 31** and foodprobe1 has **Prayer 1**, so the upstairs is gated (no
error captured, silent refusal). No altar was found on the monastery ground scans either.
**Verdict: the monastery is not a usable prayer-restore for a low-prayer body — use the
Lumbridge church altar `(3243,3205)`.** foodprobe1 returned to the Falador bank.

### Step 2x — Falador food stock confirmed; monastery = INACCESSIBLE
**Monastery = inaccessible** (1st-floor ladder gated at Prayer 31; foodprobe1 Prayer 1).
Prayer-restore card final: **Lumbridge church `(3243,3205)`** only (no Falador/monastery
altar for a low-prayer body). Falador food stock confirmed by opening the bank:
**18 food banked** (Cheese x9, Banana x3, Cabbage x5, Chocolate bar x1) **+ 4 in foodprobe1's
inventory = 22 food** staged at the safe Falador 1HP bank. foodprobe1 stays on the Falador
bank tile `(2945,3366)`, HP 25, as the 1HP receive point; `kitprep1` unchanged at Draynor;
`foodkill1` idle.

### Step 2y — held the 1HP receive point
foodprobe1 held the Falador bank `(2945,3366)` 1HP receive point for ~5 min with 4 food in inv — no leave, no death (`lifeId` unchanged).
Held again ~5 min at `(2945,3368)` with 4 food in inv while A was in the Witch's House basement — no leave, no death (`lifeId=1` steady, HP 25 throughout).
Held a third ~5 min at `(2945,3368)` with 4 food in inv while A was at the Witch's House boy — no leave, no death (`lifeId=1` steady, HP 25/25).
Held a fourth ~5 min at `(2945,3368)` with 4 food in inv while A was on the Witch's House door — no leave, no death (`lifeId=1` steady, HP 25/25).
Held a fifth ~5 min at `(2945,3368)` with 4 food in inv while A tried the boy-side garden entry — no leave, no death (`lifeId=1` steady, HP 25/25).
Held a sixth ~5 min at `(2945,3368)` with 4 food in inv while A was in the Witch's House garden — no leave, no death (`lifeId=1` steady, HP 25/25).
Held a seventh ~5 min at `(2945,3368)` with 4 food in inv while A was in the garden at the fountain — no leave, no death (`lifeId=1` steady, HP 25/25).
Held an eighth ~5 min at `(2945,3368)` with 4 food in inv — no leave, no death (`lifeId=1` steady, HP 25/25). No 1 HP Lumbridge respawn appeared during the hold (would have shown a teleport to `(3222,3218)`); nothing to clock.
The three lite clients (foodprobe1/foodkill1/kitprep1) had died (state went ~9 min stale, all tmux `lite-*` sessions gone); restarted `lite-foodprobe1` — it logged back in on the same tile `(2945,3368)` with 4 food, unchanged. Held a ninth ~5 min on a live connection at `(2945,3368)` with 4 food in inv — no leave, no death (`lifeId=1` steady, HP 25/25 every 30s sample).
Held a tenth ~5 min (live) at `(2945,3368)` with 4 food in inv — no leave, no death (`lifeId=1` steady, HP 25/25 at all 10 samples).
Held an eleventh ~5 min (live) at `(2945,3368)` with 4 food in inv — no leave, no death (`lifeId=1` steady, HP 25/25 at all 10 samples).
`lite-foodprobe1` had logged off again (state ~3.6h stale, tmux session gone); relaunched it (same character, no new account) — logged back in already on `(2945,3368)` with 4 food. Held a twelfth ~5 min (live) at `(2945,3368)` — no leave, no death (`lifeId=1` steady, HP 25/25 at all 10 samples).

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

### Step 4 — Warehouse transfer to A + live shop price card (2026-08-16)
**Iron kit delivered to A.** `kitprep1` withdrew the banked iron kit (Iron chainbody +
Iron platelegs + Iron sword) and gifted it to `goonmule1` at Draynor bank
(`bot.trade`/`serveTrades`, "Accepted trade."). `goonmule1` hauled it to the boy
`(2927,3455)` and traded it to `qstboot1` (attempt #0 "busy", #1 succeeded: *gave Iron
chainbody x1, Iron platelegs x1, Iron sword x1, received nothing*), then walked to Falador
bank `(2945,3368)` and holds a small food reserve (Shrimps + Bread). **There is only ONE
iron kit and it is now on A — `goonmule1` has no spare iron.**

**Food not landed on A (yet):** `foodprobe1` carried 22 food (13 Cheese, 3 Banana, 5
Cabbage, 1 Chocolate) to the boy but `qstboot1` declined / was busy / no-response across 8
attempts (A was cycling the Witch's House). Bug found + fixed: offering a `give` list that
includes an item not in inventory (`/bread/i`) fails the *whole* offer with "Item not found
in inventory" — now only items actually held are offered. Food is still on `foodprobe1`.

**Live smith/armour + food shop prices (buy / sell / stock), read from the open shop
interface (`shop.shopItems`):**

**Horvik — Varrock `(3229,3438)`:**
- Bronze chainbody 60/36 (5), Iron chainbody 210/126 (3), **Steel chainbody 750/450 (3)**,
  Mithril chainbody 1950/1170 (1)
- Bronze platebody 160/96 (3), Iron platebody 560/336 (1), **Steel platebody 2000/1200 (1)**,
  Mithril platebody 5200/3120 (1), Black platebody 3840/2304 (1)
- Iron platelegs 280/168 (1), Studded body 850/510 (1), Studded chaps 750/450 (1)

**Wayne's chains — Falador `(2973,3312)`:**
- Bronze chainbody 60/39 (3), Iron chainbody 210/136 (2), **Steel chainbody 750/487 (1)**,
  Black chainbody 1440/936 (1), Mithril chainbody 1950/1267 (1), Adamant chainbody 4800/3120 (1)

**Wydin's food — Port Sarim `(3014,3204)`:**
- Cheese 4/2 (3), Cabbage 1/0 (3), Banana 2/1 (3), Tomato 4/2 (3), Potato 1/0 (1),
  Redberries 3/2 (1), Chocolate bar 10/7 (1), Pot of flour 10/7 (3), Bread 12/8 (**0**),
  Raw beef 1/0 (1), Raw chicken 1/0 (1)

**Method-finding takeaways:** none of these are overstock-cheap right now — steel chain is
the cheapest useful next-tier piece at **750gp** (Horvik has 3 in stock, Wayne 1). Steel
platelegs/sword are **not stocked** at Horvik/Wayne (chain + plate only). `kitprep1` has
only **25 coins**, so no steel/mith buy is possible without ~2150gp for a full steel set —
GP, not stock, is the wedge. Cheese is 4gp at Wydin (stock 3); `foodprobe1` has 0 coins in
hand so it read prices only.

**Al Kharid legs/scimitars (Louie + Zeke) — toll-blocked for live capture, config-sourced.**
`kitprep1` reached the Al Kharid toll gate `(3267,3227)` but the crossing is a wedge: the
toll is **10gp**, `kitprep1` had 25→5 (my first toll handler double-paid because it walked
back west through the gate after crossing — fixed to a single pay), and the ground-coin
piles at the gate are **contested** (other bots grab them before pickup), so it could not
reliably re-fund the 10gp. It also could not buy anything across (0–20 coins vs 600+). The
Louie/Zeke stock/prices below are read from
[`alkharid.inv`](../server/content/scripts/areas/area_alkharid/configs/alkharid.inv)
(item, start stock, base cost); live buy price ≈ base at full stock:

Config base costs (start stock): Zeke bronze 100(5)/iron 200(3)/steel 600(2)/mith 4000(1);
Louie bronze 100(5)/iron 400(3)/steel 900(2)/black 1200(1)/mith 2000(1)/adamant 13000(1).

**LIVE capture (2026-08-16, `kitprep1` crossed the toll):** funded via a few Lumbridge-man
pickpockets (Thieving 50, 12gp), paid the 10gp toll **once** by talking to the **Border
Guard** NPC (the fixed single-pay handler works; the gate `loc` `interactLoc` fails with
`cant_reach` — talk the guard instead), crossed to `(3313,3175)`, and read both shops LIVE.
Live prices differ from config base (stock-based `price_mod`):

**Louie's Legs — Al Kharid `(3316,3175)` LIVE:** Bronze 80 (5), Iron 280 (3),
**Steel 1000 (2)**, Black 1920 (1), Mithril 2600 (1), Adamant 6400 (1).

**Zeke's scimitars — Al Kharid `(3288,3190)` LIVE:** Bronze 32 (5), Iron 112 (3),
**Steel 400 (2)**, Mithril 1040 (1).

**Full steel set LIVE:** chain 750 (Horvik) + legs 1000 (Louie) + scim 400 (Zeke) =
**~2,150gp** (matches the estimate). Zeke's steel scim (400) is the cheapest steel weapon —
cheaper than Varrock's iron-sword tier once you count the chain/legs. `kitprep1` had 2 coins
after the toll so it read-only; it is now in Al Kharid (task done). GP (~2,150) is the wedge
to actually buy a set, not stock.

### Step 5 — goonmine1 runite-miner pipeline (foundation) ✓
Created `goonmine1` (new gatherer). `bot.skipTutorial()` drops it at Lumbridge with the
**tutorial starter kit including a bronze pickaxe** — no Bob trip / coin pickup needed.
Walked to the **SE Varrock mine `(3285,3365)`** and ran a mine loop:
- Rocks are locs named `Rocks tin ore` / `Rocks copper ore` / `Rocks iron ore` with a
  `Mine` option — so ore type is selectable by loc name. Loop mines copper/tin until
  Mining 15, then **prefers iron rocks** (level 15 req), and banks ore at **Varrock east
  bank `(3253,3420)`** when the inventory fills.
- Per-loop safety: `if (player.hp < player.maxHp) bot.eatFood(...)` (implemented as
  instructed; SE Varrock has no aggro so it does not trigger — this mine is safe, unlike
  Al Kharid scorpions).
- **Live result: Mining 1 → 50 in ~10 min** (33,687 xp), banking iron at Varrock east the
  whole time (multiple full-inventory bank runs). Still climbing on the current grind.
- **Runite is Mining 85 and all five rocks are wilderness** ([`scarce-goods.md`](scarce-goods.md)) —
  so this body keeps mining iron at safe SE Varrock to build toward 85; **no wild runite,
  no Al Kharid (scorpions) until cb 27, no cow grind.** Iron ore in `goonmine1`'s Varrock
  east bank is itself a barter good / smith feedstock in the meantime.

### Step 6 — food to A + Wydin cheese restock onto goonmule1
- **A fed at the Falador sit:** when `qstboot1` sat on `(2945,3368)` at 1 HP, `foodprobe1`
  withdrew its 22-food stack and traded it to `qstboot1` in place (*gave Cheese x13, Banana
  x3, Cabbage x5, Chocolate x1, received nothing* — confirmed by `foodprobe1` inventory
  emptying). A then left Falador for the Witch's House `(2900,3477)` — not followed.
- **Wydin cheese restock onto the mule:** `foodprobe1` withdrew 25 coins, walked to Wydin
  `(3014,3204)`, bought the **entire cheese stock (3 @ 4gp = 12gp)**, returned to Falador,
  and traded the 3 cheese to `goonmule1` (`serveTrades` accept). Wydin cheese stock is only
  **3** at a time, so a bigger reserve needs repeated trips as it restocks. `goonmule1` now
  holds Shrimps + Bread + Cheese x3; `foodprobe1` has 13 coins left.
- Note: agent-to-agent gifts fail with *"doesn't have enough inventory space"* if the
  receiver's inventory is full — `goonmule1` still carries its ~18-item tutorial starter
  kit, so it can only hold ~10 more items; bank its junk first for a larger food reserve.

### Warehouse state after this pass
- `goonmine1`: SE Varrock mine, **Mining ~59 and rising** (persistent miner in tmux
  `mine-goonmine1`, auto-restarts), banking iron at Varrock east. Toward 85 for wild runite.
- `goonmule1`: Falador bank `(2945,3368)`, food reserve (Shrimps + Bread + **Cheese x6**
  after two Wydin restocks of 3 each).
- `foodprobe1`: ~1 coin after the Wydin buys; shuttles Wydin cheese (stock 3/trip) to the
  mule. Wydin cheese restocks ~3 at a time, so the reserve grows a few per round trip.
- `kitprep1`: **crossed the toll and captured LIVE Louie/Zeke prices** (above), now in Al
  Kharid with 2 coins. Funding the toll: Lumbridge-man pickpockets (Thieving 50) → talk
  Border Guard → single 10gp pay. To actually buy a steel set it still needs ~2,150gp.

### Step 7 — second A resupply + ongoing mule food shuttle
- **A fed again at the Falador sit:** `goonmule1` traded its whole food reserve to
  `qstboot1` in place (*gave Shrimps x1, Bread x1, Cheese x6 = 8 food, received nothing*).
  A left afterward — not followed.
- **Food shuttle economics (the recurring wedge):** `foodprobe1` rebuilds the mule reserve
  by (a) a sanctioned coin pickup from a ground pile near the Falador sit (~7–12gp, often
  contested) and (b) buying Wydin cheese — but **Wydin only stocks 3 cheese per restock**,
  so each round trip only nets 2–3 cheese onto `goonmule1`. After this pass `goonmule1`
  holds 2 cheese again, `foodprobe1` at 0 coins. Sustained resupply needs a real GP source
  or a bigger food shop; cheese-by-cheese from Wydin is the slow lane.
- `goonmine1`: **Mining ~68** and climbing (SE Varrock eat-loop, banking iron), toward 85.

### Step 10 — GUARD PRINTER unlocked; gloves delivered; fishing lane bootstrapped
The GP wall broke with **Falador guards** (`~(2950,3379)`, next to the mule sit): **30gp per
pickpocket**, thieve 40, stun only 2 dmg. `kitprep1` (thieve 50) hit **61gp in ~2
pickpockets**; `foodprobe1` (thieve 42) got 30gp in **1**. Bootstrapped eat-on-stun food by
`goonmule1` handing its 5 cheese to `kitprep1`.

- **Leather gloves + coins delivered to A (finally):** `kitprep1` guard-printed 61gp, bought
  **Leather gloves at Thessalia `(3204,3417)` for 6gp**, walked to the boy `(2927,3455)`, and
  traded *Coins x55 + Leather gloves x1* to `qstboot1` (received nothing). Did not enter the
  house.
- **Fishing lane bootstrapped:** `foodprobe1` guard-funded 30gp and bought a **Small fishing
  net (5gp)** + a feather at Gerrant. **Gerrant `(3013,3225)` live price card:** Small net 5,
  Fishing rod 5, Fly rod 5, Harpoon 5, Lobster pot 20, Feather 2 (stock 1000), Fishing bait 3
  (1500); raw-fish buy tiers: shrimp 5, sardine 10, herring/anchovies 15, trout 20, pike 25,
  salmon 50, tuna 100, lobster 150, swordfish 200 (most raw stock 0 — a *sell* outlet, not buy).
- **Lane split now:** `goonmine1` mines SE Varrock → 85 (no detach); `kitprep1` = guard
  printer → `goonmule1` (6k target); `foodprobe1` = food warehouse via net-fish shrimp → cook
  Lumbridge range `(3230,3196)` → dump cooked on the mule (GP-free food).

**All three lanes proven end-to-end (2026-08-16 ~16:35Z):**
- **Guard printer + pool:** `kitprep1` pickpockets Falador guards, eats cheese on stun, and
  dumps coins to `goonmule1` at the sit (`bot.trade` → `serveTrades` pool). Thieving climbed
  **50→55**; first 300gp pooled on `goonmule1`. It is **food-gated**: HP 10 + only the 5 cheese
  means it takes stuns and retreats when cheese runs out — the fix is the shrimp loop below
  feeding the mule, which re-supplies the thief.
- **Shrimp→cook→mule (GP-free food), proven:** `foodprobe1` net-fished **15 raw shrimp** at
  Draynor `(3087,3230)`, cooked at the **Draynor fireplace `(3100,3256)`** (much closer than
  the Lumbridge range), and delivered **7 cooked shrimp** to `goonmule1` (8 burned at low
  level). One cycle took Fishing **1→28** and Cooking **1→25**. `goonmule1` now holds 300
  coins + 7 cooked shrimp. Burn rate drops as Cooking levels, so cooked yield rises over time.
- **Closed loop:** guard GP → `goonmule1` (→ steel/gloves for A); shrimp food → `goonmule1`
  (→ re-supply the guard-thief and A). No shop GP needed for food; guard GP funds the kit.

### Step 13 — mule climbing (780) despite guard contention; Mining 92
Falador guards are **heavily contested** by cb-126 players (Settled, Skillspecs) who kill the
guards faster than kitprep1 can pickpocket — so throughput is a slow trickle, not zero. The
pickpocketable "knights" (50gp) are **Knights of Ardougne** (Ardougne market, far west); the
Falador White Knights are stationary/combat NPCs, **not** pickpocketable. Sending kitprep1 to
Ardougne would strand its GP ~400 tiles from the Falador mule (breaking the co-located dump)
on an already-flaky connection, so kept it on Falador guards. Fix for the trickle: **lowered
the dump threshold to 60** so contended earnings still land on the mule. Result: **mule
300 → 420 → 780** (multiple `ok=true` dumps), climbing toward 6k. `goonmine1` **Mining 92**
(639k xp) on safe SE Varrock rocks — no wilderness. Note: demo-server `BotDisconnectedError`
still flaps control connections; the tmux supervisors auto-recover.

**Flapping root cause found (the repeated "kitprep1 has no controller"):** a **duplicate grind
controller** survived each supervisor restart, so two `bun grind.ts` processes fought over
`kitprep1` and kicked each other (`BotDisconnectedError`). Fix = kill all but the youngest
grind PID after any restart (verify `ps` shows exactly 1). With a single controller + a fresh
`lite-kitprep1`, kitprep1 is stable: Thieving now **60**, mule **780 → 840** and climbing,
`goonmine1` **Mining 93**. Guard contention (cb-126 farmers) remains the throughput cap.

**Self-heal added (fixes the recurring "no controller"):** the grind loop swallowed
`BotDisconnectedError` and then *spun uselessly on a dead connection* until its 1200s timer —
so kitprep1 looked alive (process running) but did nothing. Fix: each loop checks
`sdk.getStateAge() > 15s` (or null player) and **exits**, so the tmux supervisor restarts with
a fresh connection. In one 5-min window it auto-recovered **6 disconnects** while the mule
climbed **840 → 990** and `goonmine1` held Mining 93 on safe rocks. The demo server drops
kitprep1's control link often, but the loop now recovers on its own without manual reattach.

### Step 12 — pipeline unstuck: mule coins 300 → 420; Mining 90
The mule was stuck at 300 for a long time due to three compounding bugs, now fixed:
1. **Orphaned duplicate controllers** — restarting supervisors without killing the old `bun`
   child left 2–3 grind processes fighting over `kitprep1`, causing `BotDisconnectedError`
   thrash and zero progress. Fix: one controller per lane, verified with `ps`.
2. **Dead pool** — `goonmule1 pool.ts` timed out (~26 min) and wasn't supervised, so dumps
   had no receiver. Fix: `pool-goonmule1` tmux supervisor (auto-restart).
3. **Dump out of range** — `kitprep1` tried to trade `goonmule1` from the guard tile
   (~11 tiles); trades need adjacency. Fix: dump now walks to `goonmule1`'s actual tile
   before trading, and dumps at a low threshold (≥100 / ≥60 on flee) so progress lands.
Result: **first real dump landed (`ok=true`), goonmule1 now holds 420 coins** (+7 shrimp),
climbing toward 6k. `goonmine1` reached **Mining 90** (537k xp) on safe SE Varrock rocks —
no wilderness, no lava maze, no KBD. All four lanes + pool run under single-controller tmux
supervisors. Throughput is still gated by cb-126 players farming the same Falador guards.

### Step 11 — Mining 85 reached; guard printer fixed (flee-on-catch)
- **`goonmine1` hit Mining 85** (378,875 xp) on the SE Varrock iron eat-loop — runite is now
  unlocked (all 5 runite rocks are wilderness per `scarce-goods.md`; needs a dedicated wild
  trip, not this safe loop). Kept mining SE Varrock iron; no wild runite yet.
- **Guard-printer bug + fix:** on a **cb-3 / HP-10** body, a *failed* guard pickpocket makes
  the guard aggro, and auto-retaliate then **locks kitprep1 in a losing melee** (seen taking
  8–11 dmg with no XP gain — it was punching a guard, not thieving). Fix = **flee-on-catch**:
  when hp drops, run to the mule tile `(2945,3368)` (outside guard aggro — the mule sits there
  un-attacked), eat, let combat drop, then return. With that, `kitprep1` pickpockets safely
  (Thieving 55→56, HP stayed 10/10, no lock). Dumps coins to `goonmule1` via a
  `want:[shrimp]` trade so it self-feeds. **Caveat:** cb-126 players farm/kill these guards,
  so target availability is intermittent — throughput is modest on a fragile body.
- All four lanes now run under tmux supervisors (`mine-goonmine1`, `grind-kitprep1`,
  `shrimp-foodprobe1`, plus goonmule1 `pool`) for continuous, self-restarting operation.

### Step 9 — fleet coin count + the GP wall (2026-08-16 ~16:08Z)
Inventory coins, read live from each bot:

| Bot | Inv coins |
|---|---|
| `kitprep1` | **1** |
| `goonmule1` | **0** |
| `foodprobe1` | **0** |
| `goonmine1` | **0** |

**Fleet total = 1 coin.** This is the wall behind every stalled instruction: leather gloves
(Thessalia `(3204,3417)`, 6gp — stock confirmed 10), steel scim (400), steel legs (1000),
steel chain (750), even Wydin cheese (4) are all unbuyable. `kitprep1` reached Falador with
**no gloves** (the Thessalia buy failed — 1 coin) so there is nothing to hand A right now.
Sanctioned GP (single coin pickups, no pickpocket lane) only nets a few gp at a time and the
Falador/gate piles are contested. The **GP-free paths that actually scale** are: (1)
`goonmine1` mining → smithing (iron→steel bars→gear, no shop GP), already at Mining ~73; and
(2) a net-fish-shrimp + cook loop for food. Buying kit at shops is GP-blocked until one of
those produces sellable/smithable goods or a real coin source is opened.

### Step 8 — steady state (mule shuttle + miner)
Re-read confirmed Al Kharid live prices are **stable** (Zeke steel scim 400/stock 2, mith
1040/1; Louie steel legs 1000/2, mith 2600/1, black 1920/1, adamant 6400/1). `foodprobe1`
ran another coin-pickup + Wydin trip (bought 3 cheese, traded to `goonmule1`) — mule reserve
now **5 cheese**. `goonmine1` **Mining 71** (145k xp) and still climbing toward 85. `kitprep1`
parked at Louie `(3316,3175)` with 2 coins (read-only until funded ~2,150gp).
