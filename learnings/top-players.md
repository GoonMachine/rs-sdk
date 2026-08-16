# Top players — living elite index

**Shared fact.** Operator writes this when the **top outfit kit** or a watched
name’s **activity class** changes (hill / KBD / bank / mine / offline). Do not
rewrite every 60s tick. Cloud A/B may read it; they do not own it.

Gear elite ≠ hill elite. KOTH minutes are occupancy. Outfit is last **logout**
worn kit (configured-cost, no ammo/rings in the keep sense — rings *are* on
this board; ammo is skipped at write time in
[`LoginServer.ts`](../server/engine/src/server/login/LoginServer.ts)). Bank
gold is a trap ([`owner-context.md`](owner-context.md)). Live tile is
`/playerpositions`.

Parse outfits from raw HTML (`data-item-id` + `title`). WebFetch strips
canvases.

## Watch list

`brotha`, `hoplite`, `goo001`, plus anyone in outfit-top-20 who is online.

## Snapshot 2026-08-16 ~05:25 UTC

### Outfit top 10 (parsed)

| # | Name | Value | Worn (logout) |
|---|---|---|---|
| 1 | brotha | 1,135,613 | Dragon med helm, Cape of legends, Glory(4), Dragon battleaxe, Dragon chainbody, Dragon sq shield, Saradomin legs, Cooking gauntlets, Climbing boots, Ring of life |
| 2 | hoplite | 474,713 | Berserker helm, Cape of legends, Glory(2), Dragon halberd, Rune platebody, Rune plateskirt, Goldsmith gauntlet, Climbing boots, Ring of wealth |
| 3 | bjzx62 | 421,500 | Dragon med helm, Fremennik cloak, Glory, Dragon longsword, Rune plate, Granite shield, Rune legs, gloves, boots, Ring of wealth |
| 4 | vends | 391,253 | Dragon med helm, cape, Power ammy, Dragon longsword, Rune plate + kite + legs, Dragon vambraces |
| 5–16 | celebrity / clone band | ~336–343K | **Rune full + glory + dragon longsword + rune plate/kite/legs.** Same shape, cheap gloves/boots. Seeded or copied. |
| 45–50 | goo001 / 006 / 008 / 014 / 019 / 022 | 139,600 | **Rune scim + rune chain + rune legs only.** Hill kit, not elite. |

`brotha` is a full dragon tank (sq shield 500k + chain 250k + baxe 200k).
`hoplite` is the first “real PKer we might meet”: berserker + dragon halberd +
rune plate, 474K — more than 3× goo, less than brotha’s museum set.
The 340K band is **one dragon longsword on a rune 3-piece**, not 8-stack junk.
id `1540` on peterthiel is **anti-dragon shield**, not a DFS.

### Bank top (flag gold-only)

| # | Name | Value | Top stacks | Gold-only? |
|---|---|---|---|---|
| 1–4 | nickwins / banker / banker2 / bigstacks | 1.4–2.4B | Coins 1–2B | **yes — ignore** |
| 5 | nickai3 | 731M | Raw shark / swordfish / coins / anchovies | food warehouse |
| 7 | brotha | 142M | 60M coins, 78k shark, rune plate stacks | gold-led, but has rune stock |
| 8–14 | mrmammal / framed / settled / torvesta / faux | ~123–128M | 120M coins + sharks | **mostly gold** |
| 15 | stein | 113M | hundreds of rune legs/kite/chain/helm | **rune warehouse** (real) |
| 22 | hoplite | 45M | coins, 10k rune arrows, 1k magic shortbows, cannonballs, shield right half | ranged stock |

Stein / goo007 / bjzx62 banks are the “can re-kit after death” pattern. Coin
leaders are not wealth.

### Live tiles (same pull)

| Name | Tile | Class |
|---|---|---|
| Brotha | `(2734,9692)` | **KBD pocket** |
| Tqckgxgj08 | `(3289,3887)` | hill, one tile off the old scorer |
| Goo001 | `(3288,3886)` | **scorer** — back on the hill |
| Goo006 | `(3508,9497)` plane 2 | dungeon (not hill) |
| Torvesta | `(3267,3430)` | Varrock |
| Mrmammal / Bjzx62 | `(3213,3464)` | Varrock east |
| Skillspecs / Settled | `(2963,3379)` | Falador |
| Faux | `(2652,3109)` | Camelot / Seers |
| hoplite | offline | — |

Brotha is spending time on the scarce-goods node Max named, not on the 8-stack.

### Skill-rank outliers (player pages)

| Name | Overall | Time | Rushed (low rank #) | Ignored (high rank #) |
|---|---|---|---|---|
| hoplite | 8 / 1881 | 976h | herblore 9, agility 13, magic 14, crafting 16 | fishing 3038, thieving 5601 |
| brotha | 9 / 1881 | 1452h | herblore 10, agility 14, crafting 19, fletching 19 | fishing 3057, thieving 5610 |

They maxed the same skills; they did **not** fish or pickpocket for the kit.
Herblore / agility / crafting / magic are the rush. Do not copy a cow or
fishing grind.

### What a geared incoming PKer looks like

Not `Tqckgxgj08` in empty slots. A real kit is **dragon weapon + rune (or
dragon) body + glory + a helm that is not bronze**, 340K–475K logout value,
often with a food/arrow bank. `brotha` at KBD is the endgame tank. Goo’s 139K
3-piece loses that fight and still wins **minutes** because KOTH scores combat
level inside the polygon, not worn value.

## When to rewrite

- Outfit #1–3 items change
- A watched name’s class changes (hill ↔ KBD ↔ offline)
- A new name enters outfit-top-5
