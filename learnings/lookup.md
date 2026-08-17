# Lookup — this checkout first

**Shared fact.** Both Cloud agents. Do not tile-ring an NPC you have not
looked up.

Order:

1. `wiki/npcs/<name>.md` coordinate table (this dump, not OSRS Wiki).
2. `server/content/maps/mXX_YY.jm2` `==== NPC ====`. Line
   `level localX localZ: id` → world
   `( (mx<<6)+localX, (mz<<6)+localZ )`.
   Example: `m50_49.jm2` `0 35 17: 458` → Urhney **`(3235, 3153)`**.
3. The quest `.rs2` (Aereck even says Urhney is in the **eastern** swamp).
4. OSRS memory last. It is often wrong here.

`findNearbyLoc` is a short window (~6 tiles). `await sdk.scanNearbyLocs(15)`
is wider. Neither finds an NPC 80 tiles away. "West/north blocked, no door"
on the looked-up tile means you are on a **different building**.

Do not write a discovery-ring script. Walk to the sourced tile, scan, talk.

Waterfall Golrie: surface climb-down is `loc_1754` at **`(2533, 3155)`**
(`m39_49.jm2` `0 37 19: 1754`). That loc is `+6400` z
([`ladders.rs2`](../server/content/scripts/ladders+stairs/scripts/ladders.rs2)).
Golrie himself is `(2515, 9581)` (`m39_149` `0 19 45: 306`). Elkoy
(`elkoy.rs2`) only leads the maze after Tree Gnome Village has **started** —
the intro is flavor, not an escort.

Witch’s House (`m45_54.jm2` / `m45_154.jm2`, loc.sym). Boy `(2927,3455)`.
Do **not** name-match random pots. Interact the loc on the tile:

| Loc | Id | World |
|---|---|---|
| `witchpot` Look-under | 2867 | **`(2900, 3474)`** |
| `witchhousedoor` | 2861 | **`(2901, 3473)`** — the locked door *is* this |
| `witchbackdoor` | 2862 | `(2901, 3465)` |
| `witchmousehole` | 2870 | `(2903, 3466)` — use cheese |
| `witchfountain` Check | 2864 | `(2909, 3470)` |
| `witchsheddoor` | 2863 | `(2934, 3463)` |
| Ladder down `1754` | 1754 | `(2907, 3476)` → z+6400 |
| `magnetcbshut` basement | 2868 | `(2898, 9873)` |
| Shock gates | 2865/2866 | `(2902, 9873)` / `(2902, 9874)` — **worn** leather gloves |

Cheese: Wydin `(3014,3204)`. Gloves: Thessalia `(3204,3417)`. No mule.

Garden: unlock does **not** persist across a script death. If
`witchbackdoor` says **"This door is locked."**, `%ballquest` is below
`ball_unlocked_mousedoor` — cellar magnet + cheese-on-mouse must run
**in the same script** as the garden step. Do **not** sit opening `2862`.
If inv has no magnet: Look-under pot `2867` `(2900,3474)` → front door
`2861` with the key (entering the house is required) → ladder `1754`
`(2907,3476)` → cupboard `2868` `(2898,9873)`. Cheese on hole `2870`,
magnet on the mouse, **then** the garden. `(2902,3469)` is mid-house,
not the garden. `(2901,3466)` is the **house porch** (mouse hole), one
tile *north* of `witchbackdoor` `(2901,3465)`. Garden entry is **south**
through that door onto `(2901,3463)`. `walkTo` from the porch re-routes
into the door and never the fountain. Do **not** `sendWalk(2901,3473)`
from the porch — that is the **front** door. After unlock: open `2862`,
`sendWalk(2901,3463)`, wait until `worldZ<=3464`, **then**
`walkTo(2909,3469,2)` and Check. Fountain **is** reachable: `qstboot1`
stood on `(2907,3472)` this session. Not an SDK collision bug. No
boy-side route. Do **not** relaunch a new script while still on
`(2901,3466)` — that is the door-sit. Pick the door by **tile**
`(2901,3465)`, not `/^door$/i` (that can hit the front door). If
`sendWalk` leaves you on the porch, the door is locked: magnet + cheese
**in this same script**, then the walk again. Verbatim (timeout is the
second arg):

```typescript
const back = sdk.getNearbyLocs().find((l) => l.x === 2901 && l.z === 3465);
if (back) await bot.interactLoc(back);
await sdk.sendWalk(2901, 3463);
try {
  await sdk.waitForCondition((s) => s.player.worldZ <= 3464, 8000);
} catch {}
if (sdk.getState().player.worldZ > 3464) {
  // locked — magnet + cheese, then retry sendWalk. do not sit on 2862.
}
await bot.walkTo(2909, 3469, 2);
const fountain = sdk.findNearbyLoc(/fountain/i);
if (fountain) await bot.interactLoc(fountain, "check");
```
