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
`runiterock1` (2106) / `runiterock2` (2107). All **five** jm2 placements are
wilderness (world = mapsquare×64 + local):

| World | Map | Zone |
|---|---|---|
| `(3059, 3885)` / `(3060, 3884)` | `m47_60` | Surface wild ~46 (near the hill) |
| `(2937, 9882)` / `(2941, 9884)` | `m45_154` | Underground wild |
| `(3046, 10265)` | `m47_160` | KBD-lair pocket |

`~scale_by_playercount` can cut the 2400-tick base (at 2000 online → 1200).
It does not remove spawn control. No safe-area runite in this checkout.

We cannot mine this on `qstboot1` (mining 1) or a fresh gatherer. Later: one
dedicated miner + hauler, not a hill body.

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

**No standing mule.** Banks are **per-account** — `foodprobe1` cannot
withdraw for `qstboot1`. There is **no Lumbridge bank**
([`banking.md`](banking.md)). Closest after a death: **Draynor `(3092,3243)`**.
A is at Falador-south now; Falador bank is ~`(2945,3366)` (`m46_52`).

`qstboot1` **shops and banks for himself** (Betty, Ned, Wydin, Horvik, Thessalia).
Park extras in *his* bank before Golrie / fire giants. After Lumbridge, walk
Draynor and withdraw. Do not wait for a courier.

B spends the model on **`kitprep1` warehouse** (iron → steel → food stacks in
*kitprep1’s* bank). Move items across accounts only with a **scheduled
Draynor trade**, then each side banks. `foodprobe1` stays parked at that
bank as a spare inventory, not a watcher. `foodkill1` stays idle until a
real 1 HP clock — that body also self-banks food before the wild trip.

| Priority | Body | Job |
|---|---|---|
| 1 | `kitprep1` | Warehouse. GP → shop kit → **his** Draynor bank. Trade A only when both are at Draynor. |
| 2 | `foodprobe1` | Spare. Do not death-watch. Trade only if kitprep1 is busy and A is at Draynor. |
| 3 | `foodkill1` | Idle until a real `sdk.getState()` 1 HP clock. |

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

Food: Wydin cheese `(3014,3204)` (also Witch’s House item) → Brimhaven lobster `(2793,3188)` ~195 gp / 12 HP when GP allows. No monkfish. Shark is fish 76 / cook 80, no cooked-shark shop.

GP that is not cows: flax → string, essence after Rune Mysteries, fish to Gerrant, leather gloves to Thessalia. General stores pay ~0 when overstocked.

`kitprep1` is already created. Next new gatherer is `goonkit1` only after a POST.
Never commit `bot.env`.
