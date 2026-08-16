# Scarce goods — methods and target keep-kit

**Shared fact.** Barter goods, not GP. Max:
[runite + black hides](https://x.com/maxbittker/status/2088476333051453730).
Gavin: the moat is private scripts that **consistently claim** those nodes
([tweet](https://x.com/gavinbasuel/status/2088746501300441525)). Live elite
kits: [`top-players.md`](top-players.md).

Do not send `qstboot1` here mid-quest. Do not walk a scarce kit onto the hill
until the 1 HP rejoin loop is boring.

## Runite

Source: [`mine.dbrow`](../server/content/scripts/skill_mining/configs/mine.dbrow)
`[runite_rock_table]` — **Mining 85**, 1250 exp, success 1/18,
**2400-tick** base respawn. Wiki `mining.md` “70” is stale.

Locs: [`rocks.loc`](../server/content/scripts/skill_mining/configs/rocks.loc)
`runiterock1` (2106) / `runiterock2` (2107). Five jm2 placements. Wild from
[`wilderness_zones.dbrow`](../server/content/scripts/areas/area_wilderness/configs/wilderness_zones.dbrow)
+ `wilderness_level` = `(z - zoneMinZ)/8 + 1`. PvP:
`abs(cb diff) <= min(wild)` ([`pvp_combat.rs2`](../server/content/scripts/skill_combat/scripts/pvp/pvp_combat.rs2)).

| World | Map | Wild | What it actually is |
|---|---|---|---|
| `(3059, 3885)` / `(3060, 3884)` | `m47_60` | **46** | Lava-maze surface. Next to the hill. **Not** the first rock. |
| `(3046, 10265)` | `m47_160` | **44** | KBD pocket. Saturated (hundreds on `(2717,9817)`). **Never.** |
| `(2937, 9882)` / `(2941, 9884)` | `m45_154` | **0** | Heroes’ Guild mine. Door needs `%heroquest >= complete` ([`heroes_entrance.rs2`](../server/content/scripts/areas/areas_heroes_guild/scripts/heroes_entrance.rs2)). **The farm** once A finishes Heroes’. |

`~scale_by_playercount` can cut the 2400-tick base (~12 min at 300.3 ms; ~6 min
at 2000 online). Success 1/18. Spawn control is the game.

### Runite doctrine (do not get farmed)

`goonmine1` stays **combat-low**. Do not cow-train, do not quest this body.
At wild 46 a cb-3 miner is only attackable by cb **1–49**. The 123 scorer
(diff 120) **cannot** hit them. Training combat into the 80s puts them on
Goo001’s menu.

Never skull. Unskulled keep is **3 items by `oc_cost`**, and a stack is not
one keep ([`death.rs2`](../server/content/scripts/player/scripts/death.rs2)).
Runite ore is 3200 each — a 28-ore inv drops 25. **Bank or mule-trade at 3
ores.** PvP death → Lumbridge 1 HP; the killer sees the loot first.

Do not public-drop. `goonmule1` waits **south of the ditch** (`z < 3520`),
takes the trade, banks Falador. Scout `/playerpositions` on the rock tile
before the walk. Empty rock + no mid-cb names → click. Occupied → leave.

Until Mining 85: iron → coal 30 → mith 55 → addy 70 in **safe** rocks. Do
not walk wild “to look.” After 85, skip lava-maze / KBD until Heroes’ is
open unless a scout shows the surface pair empty **and** no cb 1–49 on it.
Then 3-ore trips only.

Dedicated miner + hauler, not a hill body. Not `qstboot1`.

## Black dragon hides

Always-drop black hide:

- Ordinary black dragon — [`black_dragon.rs2`](../server/content/scripts/drop%20tables/scripts/black_dragon.rs2) always `dragonhide_black`. Combat 227, 60-tick. **One** jm2 spawn: `m44_154` `0 4 27: 54` → `(2820, 9883)` underground wild. Owner-context “four spawns” / `(3048, 10266)` is stale — that tile is a runite rock.
- KBD — [`king_black_dragon.rs2`](../server/content/scripts/areas/area_wilderness/scripts/king_black_dragon.rs2) also always hide. Combat 276, lair `(2717, 9817)`, 150-tick. In lever `(3067, 10253)`. **Saturated** per Max. `Brotha` lives here.

Tan: Al Kharid [`tanner.rs2`](../server/content/scripts/areas/area_alkharid/scripts/tanner.rs2) (`dragonhide_black` → `dragon_leather_black`).
Craft black d'hide: vambraces **79**, chaps **82**, body **84**
([`levelup_unlocks_crafting.rs2`](../server/content/scripts/levelup/scripts/levelup_unlocks_crafting.rs2)).
Wear black d'hide: ranged **70** / body also def **40**
([`tier70.rs2`](../server/content/scripts/levelrequire/scripts/tier70.rs2)).

Generic `wiki/items/dragonhide.md` collapses colors — do not use it.

## Kalphite Queen (still open)

Max still called KQ unsolved 2026-08-02. Surface burrow `(3226, 3108)`,
queen `(3474, 9496)` ([`wiki/npcs/kalphite-queen.md`](../wiki/npcs/kalphite-queen.md)).
Rope on both holes (`kalphite_locs.rs2`); no quest flag. Drop table
[`kalphite_queen.rs2`](../server/content/scripts/drop%20tables/scripts/kalphite_queen.rs2)
includes a dragon-chain roll (~1/128). Prestige + kit, not a first gather.
No hide drop.

## What the 340K–1.1M outfits are made of

| Piece | Configured | How (this checkout) |
|---|---|---|
| Dragon sq shield | 500k | Left + right halves (rare). Do not wear on a first hill trip. |
| Dragon chainbody | 250k | Rare PvM (KQ table comments a D chain). |
| Dragon battleaxe / halberd | 200–250k | Rare PvM. Halberd is hoplite’s weapon. |
| Dragon longsword | 100k | The **340K-band signature**. One of these on rune plate is the celebrity clone kit. |
| Dragon med helm | 100k | Rare PvM. brotha / vends / bjzx62. |
| Rune plate / chain / legs / kite / full helm | 35–65k each | Smith or shop. goo wears chain+legs+scim only (139,600). |
| Amulet of glory | 17,625 | Heroes’ Quest + crafting. On almost every elite logout. |
| Cape of legends | 450 | Legends’ Quest (brotha, hoplite). |
| Berserker helm | 60k | Fremennik (hoplite). |
| Dragon vambraces | 4,320 | Black-hide craft (or drop). vends / wanghaf. |
| Climbing boots / leather gloves | 6–12 | Shops. Ignore. |

KOTH ignores this for **score**. A fight against hoplite/brotha does not.

## Target keep-kit

Configured-cost keep: [`death.rs2`](../server/content/scripts/player/scripts/death.rs2).
Unskulled 3, skulled 0 unless Protect Item.

| Role | Wear / carry | Do not |
|---|---|---|
| **Quester (`qstboot1`)** | Shop **iron** at least: chain `(3229,3438)` Horvik 210gp, sword `(3203,3397)` 91gp, legs 280gp. Att 68 can wear steel/mith weapons; def 1 still wears iron plate. Equip after Vampire. Drop weapons+runes only for Glarial’s tomb. | Naked + bronze after att 68. Rune 3-piece on a skulled trip. |
| **Scorer (unskulled)** | Three cheap replaceable slots. Food if it still keeps. | Dragon sq / chain / baxe. goo’s rune 3-piece *fits* keep — do not copy it onto a **skulled** attacker. |
| **Pile (skulled)** | Protect Item on; one PI candidate (cheap weapon); food you will lose. | Any dragon piece. Any 139K 3-piece. |
| **Banked scarce (off-hill)** | Dragon longsword (first real upgrade), then med helm, then hide body/vambraces, then glory. Shark / Wydin food stacks. Stein-style rune warehouse if we smith. | Walking this set to wild 46 before 1 HP rejoin is boring. Wearing the 500k sq shield as a scorer (it will eat keep slots). |

First **fight** upgrade after cb ≥ 77: one dragon longsword + rune plate, glory
if Heroes’ is done. That is the 340K band, not brotha’s museum.

## Background kit pipeline (B’s VM)

Not runite (mining 85). Not black dragons (combat 227). Not cows. Not the hill.
`qstboot1` does **not** farm this mid-quest. Warehouse first; wear later.

We **own every account.** A bank is per-character (you cannot withdraw
another name’s tab), but anything useful **gets traded**. Do not leave
iron or food stranded because “A shops for himself.” There is **no
Lumbridge bank** ([`banking.md`](banking.md)). Meet at Falador
`(2945,3366)` or Draynor `(3092,3243)`, or walk the mule to wherever A is.

`goonmule1` is the sit / haul body so `kitprep1` and `foodprobe1` keep
progressing. Park extras in *qstboot1’s* bank after the trade. After a
Lumbridge death, A walks Falador (126 s) or meets the mule — not a
naked re-quest.

| Priority | Body | Job |
|---|---|---|
| 1 | `kitprep1` | Warehouse. Once **one iron set is on `goonmule1`**, shop the next tier — do not sit. Horvik → Wayne → Louie → Zeke. Read live prices. Steel, then mith. |
| 2 | `foodprobe1` | Food runner. Withdraw / catch / Wydin, then **trade `qstboot1`** so the quester carries food into combat. Not a 5-min sit. |
| 3 | `goonmule1` | The sit. Falador bank `(2945,3368)`. 1 HP receive. Spawned so progress bodies can move. |
| 4 | `foodkill1` | Idle until a real `sdk.getState()` 1 HP clock. |

### Shop ladder (source-checked)

| Stage | Kit | GP (weapon+chain+legs) | Shop tiles |
|---|---|---|---|
| **0 now** | Iron chain + legs + sword/scim | ~600 | Horvik `(3229,3438)` chain 210 / legs 280; Varrock sword `(3203,3397)` 91; Zeke scim `(3288,3190)` 112 |
| **1** | Steel same | ~2,150 | Horvik / Wayne chain `(2973,3312)`; Louie legs `(3316,3175)`; Zeke |
| **2** | Mith same | ~5,590 | Same shops |
| **3** | Rune chain + legs + **longsword** | ~146k | **Scavvo** Champions Guild `(3191,3351)` L1 — **32 QP** (`champions_guild.rs2`). Stock 1, slow restock. **No rune scim in any shop** (smith 90 / trade). |
| **4** | Dragon longsword | +100k | **Jakut** Zanaris `(3252,9572)` — Lost City complete + att 60. Stock 2. **Quester** opens this, not `kitprep1`. |
| **5** | Glory | craft | No shop. Heroes’ = 55 QP + Lost City + Dragon Slayer + Merlin + Arrav. Not a gatherer quest. |
| **6** | Rune platebody | +84.5k | Oziach after Dragon Slayer, or smith 99. |

Food heals (`consume_normal.dbrow`): cheese **2**, shrimp **3**, trout **7**, salmon **9**, lobster **12**, swordfish **14**, karambwan **18**, shark **20**. Shops are **not** the warehouse — Brimhaven lobster/swordfish/karambwan stock **2–3**, Wydin cheese stock **3**, Gerrant raw lobster/sword stock **0**. `brotha`’s 78k shark and `nickai3`’s raw-shark bank were **caught**. `foodprobe1` fishes; `kitprep1` does not.

### GP — the world is rich, we measured **1 coin**

2026-08-16 B count: `kitprep1` 1 / mule 0 / `foodprobe1` 0 / `goonmine1` 0.
Thessalia gloves (6) and steel (~2.1k) were unbuyable. “GP is worthless so
do not pickpocket” left the fleet broke while the rest of the world ran
the printer. Cash is a bad *hiscore*; it is still the shop key.

**Treasury:** all coins live on `goonmule1` (Falador bank). After every A
death, trade a coin stack + gloves + food onto `qstboot1`. Do not leave
coins on a body that walks the shed.

**Printer** (`pickpocket.dbrow`) — this is the scalable lubricant:

| Target | Thieve | Coins | Stun dmg | Who |
|---|---|---|---|---|
| Man | 1 | 3 | 1 | Emergency 6 gp only |
| Warrior | 25 | 18 | 2 | — |
| **Guard** | **40** | **30** | 2 | **`kitprep1` only** (thieve 50). `foodprobe1` fishes. |
| Knight (Ardougne) | 55 | 50 | 3 | `kitprep1` next |
| Hero | 80 | 200–300 | 4 | Later |

Eat on stun. Bank into the mule. Target purse on mule: **6k** (mith set +
Waterfall 87 + glove restock). Then shop. Do not gen-store the coins.

Inflation still helps **buyers**: `shop.rs2` `price_mod` = stock − base;
overstock cuts the price we pay (floor 1 gp). It kills **sellers**.

| Need | Do this | Do not |
|---|---|---|
| 1–20 gp (pickaxe, gloves, cheese) | Pull from the mule treasury. If the mule is at 0, **guard-pickpocket** until 50, then buy. Bob pickaxe 1 gp. | A walking Varrock broke. Leaving 1 coin on `kitprep1`. |
| Iron / steel / mith kit | Guard printer → mule → **open Horvik and read live price.** Buy if coins cover. Do **not** mine-to-smith iron legs. | Hoping overstock is 1 gp when we have 1 coin. |
| Food | `foodprobe1` fishes (shrimp → trout → lobster → sword → shark). Wydin cheese is the 2 hp quest stack. | Buy Brimhaven lobster as the farm (stock 3). Park `foodprobe1` on guards. |
| 100k+ (Scavvo / Jakut) | Quester QP + **barter** (runite / hides later). High alch is 60% of `oc_cost` (`alchemy.rs2`) — only if we already have natures and alchable loot. | Hero pickpocket. Selling into Lumbridge gen store. |

Rule: `bot.openShop` and read `shopItems` price/count **before** any GP grind.
Sell only to a **depleted** specialty shop (stock below base). Bank the item,
not the coins, when the item is the kit.

## Gather methods (prior — update when a thread warrants it)

Last source-check, not a loop ritual. Do not re-score this table every
tick. Pull a new thread if an agent hits a gate, opens a shop, or
invents a method we have not priced. Sources:
`levelup_unlocks_{mining,fishing,woodcutting,smithing,cooking}.rs2`,
`mine.dbrow`, `trees.dbrow`, `smelting.struct`, `fishing.obj` /
`firemaking.obj` costs. General stores already pay ~0 when dumped —
configured `cost` is **not** cash.

| Method | Gate | Configured cost | Same output as… | Verdict |
|---|---|---|---|---|
| Horvik iron chain / legs | coins | 210 / 280 (less if overstock) | smith iron chain 26 / legs **31** | **Buy.** Done on `kitprep1` (chain + legs + sword banked). |
| Betty air / water / earth | coins | **4 gp** each, stock 1000 (`foodprobe1` 2026-08-16) | Waterfall 6/6/6 | **72 gp** for a full re-supply. Not overstocked. Buy after Witch’s House, not now. |
| Ned rope | coins | **15 gp**, dialog sale, unlimited (`foodprobe1` 2026-08-16) | mugger drop | **Buy.** Not a stocked shop — no decay. Waterfall kit = 72 + 15 = **87 gp**. |
| Copper / tin | mine 1 | ore 3 | “any GP” | Weak. Only if we are training a later runite miner. |
| **Drogo** (Dwarven mine `(3036,9846)`) | walk + ore | wiki sell copper/tin **2**, iron **11**, coal **31** when stock **0** | miner already gathering | **Only lubricant sell** if live `shopItems` count is still 0. One inv of iron ≈ 11×28 if it stays depleted. Read the shop first — same formula dumps the price when we overfill it. Do not gen-store the ore. |
| Iron ore | mine **15** | 17 | Horvik iron | Unlock, do not grind for legs. |
| Iron bar / legs | smith 15 / **31** | 3 bars / legs | Horvik 280 | Shop wins until we are already 31 for another reason. |
| Steel | mine coal 30 + smith 30 | — | Horvik / Wayne / Louie ~2.1k | Shop first. |
| Shrimp (Draynor `(3087,3230)`) | fish 1, cook 1 | 3 hp | Wydin cheese 2 hp | **`foodprobe1` start.** Net at Gerrant `(3013,3225)`. Cook Lumbridge range `(3230,3196)`. Dark wizards — stay south of z≈3232. |
| Trout / salmon | fish 20 / 30, cook 15 / 25 | 7 / 9 hp | shrimp | Barb village lure `(3110,3434)`. Fly rod + feathers. No boat. |
| Lobster | fish 40 + cook 40 | 12 hp | Brimhaven shop stock **3** / 195 gp | **Catch** at Musa `(2923,3179)`. Boat 30 gp (`sailors.rs2`). Pot 20 gp at Gerrant. |
| Swordfish | fish 50 + cook 45 | 14 hp | Brimhaven stock **2** | F2P ceiling. Same Musa cage/harpoon spots. |
| Karambwan (shop) | walk Brimhaven | 18 hp / **1 gp** | shark 20 | **Probe only.** Shrimp-and-Parrot `(2793,3188)` stock **3**. Not a warehouse. |
| Shark | fish **76** + cook **80**, `members=yes` | 20 hp | no cooked-shark shop | End-game food. Try at 76; if the catch is members-blocked, stay on swordfish. Elites already have the stacks. |
| Logs / oak / willow | wc 1 / 15 / 30 | 4 / 20 / 40 | “any GP” | Skip. Dark wizards sit on Draynor willows. Gen stores pay ~0. |
| Flax → bowstring | members flax + spin | flax cost 5 | Lowe / Hickton | **No lane.** Flax is `members=yes`. Lowe already holds 2000 bronze arrows (sell **0**). Hickton mith/addy/rune arrows wiki-stock 0 — only useful if we already fletch those, which we do not. Elites rushed fletching for **kit** (hoplite’s 1k magic shorts), not GP. |
| Fletch short/longbow → Lowe | wc + fletch + string | Lowe sell 27–88 | “flax money” | Skip. Four-stock bows, then overstock → 0. Same dump as hides into Lumbridge gen. |
| Yew / magic | wc 60 / 75 | 160 / 320 | high alch 60% of `oc_cost` | Only with natures already in bank. |
| Runite | mine **85** + smith 85 + 8 coal | ore 3200 | the scarce good | The mining endgame. Do not start this on `qstboot1`. |
| Pickpocket **guards** | thieve 40 | 30 gp | shop lubricant | **`kitprep1` only** → mule. Knights at 55. Not `foodprobe1`. |
| Pickpocket men | thieve 1 | 3 gp | emergency 6 gp | Only if no 40+ thief is up. |
| 1 HP bank after death | food in inv | — | Draynor `(3092,3243)` | **Falador `(2945,3366)`.** Lumbridge→Falador **126 s**, 0 dmg, ~30 food staged (`foodprobe1` 2026-08-16). Draynor last tiles two-shot: jail guard cb26 `(3101,3238)` + dark wizard `(3084,3237)`. Pray first at Lumbridge church altar **`(3243,3205)`** (Pray-at; live restore). No Falador ground altar. Monastery upstairs gated (Pray 31; ladder at 3057,3483 will not climb). |
| Falador chests / drawers | none | 1–10 coins on a hit | “free gp” | **Skip.** `findsomethingnice.rs2` is ~1/10 then 1/4 coins. Live: 44 searches, 0 gp (`foodprobe1`). |
| Chickens | none | feathers / raw chicken | “few gp” | **No coins** on this server. Wydin pays 0 for raw chicken. Feathers → Gerrant if the shop is not dumped. |

**Now (warehouse — do not sit):**

GP is **shop lubricant**, not a hiscore. We still need a **6k mule
treasury** (measured 1 coin). **`kitprep1`** prints (guards → mule).
**`foodprobe1`** fishes. **`goonmine1`** stays on Mining 85. Do not
cross those jobs.

| Body | Loop |
|---|---|
| `kitprep1` | **Printer only.** Falador guards ~`(2950,3379)`. First 50 gp: Thessalia gloves `(3204,3417)` + coins onto `qstboot1` at the boy `(2927,3455)` — not the Falador sit. Then dump to mule until ~6k. Shop Horvik/Wayne/Louie/Zeke when `price <=` coins. Knights at 55. Write coin counts into `ab-results-b.md`. |
| `foodprobe1` | **Food warehouse.** Hiscores fishing/cooking unranked. Gerrant `(3013,3225)` net + fly rod + feathers (~20 gp from mule, or one guard stack then **stop**). Draynor net `(3087,3230)` → cook `(3230,3196)` → mule. Fish 20: Barb lure `(3110,3434)`. Fish 40: Musa pot `(2923,3179)` (boat 30 gp). Fish 50: sword. Fish 76: try shark. Wydin cheese is the 2 hp quest stack. Do not follow A into the house. |
| `goonmine1` | **Runite path.** Do not detach. SE Varrock eat-loop to 85. Coal 30 / mith 55 / addy 70 when they unlock. No wild runite before 85. No pickpocket. No smith-for-GP. |
| `goonmule1` | Treasury. Sit Falador `(2945,3368)` with coins + cooked food. After every A death, trade coins + gloves + food onto `qstboot1`. |

**Now (`qstboot1`):** Witch’s House (HP dump) still beats any gather. Wear
the traded iron. Food in inv before the shed. After HP, Waterfall.

`goonmine1` is the runite body (Mining **85**, all rocks wilderness). Path:
Bob pickaxe 1 gp `(3232,3203)` → SE Varrock `(3285,3365)` copper/tin until 15 →
iron there. **Food in inv + eat if `hp < maxHp`** on every mine loop. Skip
Lumbridge swamp (silent fail). Skip Al Kharid until they can tank scorpions
(cb 27+, later). Do **not** walk the wild runite rocks before 85. Do not
cow-train combat for this body. Coal 30 / mith 55 / addy 70 when those
unlock. Bank ore; do not smith for GP.

`kitprep1` still shops the armour ladder. Never commit `bot.env`.
