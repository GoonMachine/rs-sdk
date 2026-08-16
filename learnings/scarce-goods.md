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
`runiterock1` / `runiterock2`. Owner review found **five** map placements,
including Wilderness ([`owner-context.md`](owner-context.md)). Player-count
scaling can shorten respawn; it does not remove spawn control.

We cannot mine this on `qstboot1` (mining 1) or a fresh gatherer. Later: one
dedicated miner + hauler, not a hill body.

## Black dragon hides

Always-drop black hide:

- Ordinary black dragon — [`black_dragon.rs2`](../server/content/scripts/drop%20tables/scripts/black_dragon.rs2) `obj_add(..., dragonhide_black, 1, ...)`. Combat 227, 60-tick respawn. Samples `(2829, 9826)`, `(3048, 10266)` ([`owner-context.md`](owner-context.md)). Four ordinary spawns in the reviewed maps.
- KBD — [`king_black_dragon.rs2`](../server/content/scripts/areas/area_wilderness/scripts/king_black_dragon.rs2) also always `dragonhide_black`. Combat 276, `(2716, 9817)`, 150-tick ([`wiki/npcs/king-black-dragon.md`](../wiki/npcs/king-black-dragon.md)). **Saturated** per Max. `Brotha` was live at `(2732,9689)` on 2026-08-16 — this is where the gear elite spends time.

Tan: Al Kharid [`tanner.rs2`](../server/content/scripts/areas/area_alkharid/scripts/tanner.rs2) (`dragonhide_black` → `dragon_leather_black`).
Craft black d'hide: vambraces **79**, chaps **82**, body **84**
([`levelup_unlocks_crafting.rs2`](../server/content/scripts/levelup/scripts/levelup_unlocks_crafting.rs2)).
Wear black d'hide: ranged **70** / body also def **40**
([`tier70.rs2`](../server/content/scripts/levelrequire/scripts/tier70.rs2)).

Generic `wiki/items/dragonhide.md` collapses colors — do not use it.

## Kalphite Queen (still open)

Max still called KQ unsolved 2026-08-02. Loc sample `(3474, 9496)`
([`wiki/npcs/kalphite-queen.md`](../wiki/npcs/kalphite-queen.md)). Drop table
[`kalphite_queen.rs2`](../server/content/scripts/drop%20tables/scripts/kalphite_queen.rs2)
includes rune chain / kite and a dragon-chain roll. Prestige + kit, not a
first gather.

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
| **Scorer (unskulled)** | Three cheap replaceable slots. Food if it still keeps. | Dragon sq / chain / baxe. goo’s rune 3-piece *fits* keep — do not copy it onto a **skulled** attacker. |
| **Pile (skulled)** | Protect Item on; one PI candidate (cheap weapon); food you will lose. | Any dragon piece. Any 139K 3-piece. |
| **Banked scarce (off-hill)** | Dragon longsword (first real upgrade), then med helm, then hide body/vambraces, then glory. Shark / Wydin food stacks. Stein-style rune warehouse if we smith. | Walking this set to wild 46 before 1 HP rejoin is boring. Wearing the 500k sq shield as a scorer (it will eat keep slots). |

First **fight** upgrade after cb ≥ 77: one dragon longsword + rune plate, glory
if Heroes’ is done. That is the 340K band, not brotha’s museum.

## First gather (B’s VM, one extra lite)

Not runite (mining 85). Not black dragons (combat 227). Not cows (A/B loser).
Not the hill.

**`kitprep1` on Cloud B’s existing VM:** buy and bank **Wydin food**
(Port Sarim `(3014,3204)` — cheese / banana / cabbage) plus Betty runes if
`foodprobe1` is on death-watch. Park Draynor. Trade `foodprobe1` or `qstboot1`
when asked. This is the first gather because elite banks are food-backed and
our Waterfall / 1 HP corridor already stalls on empty inv.

`foodprobe1` stays mule + death-watch. `foodkill1` stays idle until a real
1 HP clock. Create `kitprep1` only when the operator POST says so. One name,
same VM, `bun bots/create-bot.ts kitprep1` then lite. Never commit `bot.env`.
