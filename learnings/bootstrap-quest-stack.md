# Combat bootstrap — 25× quest stack

**Shared fact.** Agent A executes this on `qstboot1`. The operator uses it to
steer. Do not put API ids or force-send tactics in this file.

Use this instead of cows. `stat_advance` values are **tenths**; production multiplies by **25** (Cook's Assistant live: 3000 → 7,500 cooking). Combat formula is [`Player.ts`](../server/engine/src/engine/entity/Player.ts) `getCombatLevel`. Wiki XP numbers are often 1× and stale.

Need **Prayer 25** (Protect Item) and **combat ≥ 77** (`abs(cb − 123) ≤ 46` at wild 46). Protect from Melee is Prayer **43**.

## Order on `qstboot1`

| Step | Source | Display XP (25×) | After (from fresh) |
|---|---|---|---|
| 1. Restless Ghost | `quest_priest.rs2` `stat_advance(prayer, 11250)` | +28,125 pray | **pray 47** — PI + prot melee. Lumbridge. Do this first. Father Urhney is **`(3235, 3153)`** (`m50_49.jm2` `0 35 17: 458`), SE swamp toward Al Kharid — **not** OSRS west-swamp `(3147,3174)` / `(3150,3176)`. |
| 2. Vampire Slayer | `quest_vampire.rs2` `48250` att | +120,625 att | att 68. Items: garlic (free, Morgan's house), hammer (gen store), beer (Harlow / bartender). Prot melee for Count Draynor. Do not skip to Waterfall for lack of ~4 gp — sell junk or mule, do not farm men. |
| 3. Waterfall | `quest_waterfall.rs2` `137500` att/str | +343,750 each | att 87 / str 83 / **~cb 65**. Prot melee in the dungeon. White Wolf = Taverley→Catherby **trail**, not south of z=3400. Items: rope (Ned Draynor `(3100, 3258)` / 15gp, `ned.rs2` — not a mugger grind), 6 air, 6 water, 6 earth. |
| 4. Tree Gnome Village | `quest_tree.rs2` `114500` att | +286,250 att | att 94 / ~cb 67. Same pocket as the Waterfall pebble. |
| 5. Fight Arena | `quest_arena.rs2` `121750` att | +304,375 att | att 99 / ~cb 69 |
| 6. Witch's House | `quest_ball.rs2` `hitpoints, 63250` | +158,125 hp | HP dump. Taverley. After you can kill the shapeshifter. |
| 7. Merlin's Crystal → Holy Grail | `quest_grail.rs2` `110000` pray + `153000` def | +275,000 pray / +382,500 def | **cb 90+**, pray 80+. This is the 77 jump, not cows. |
| 8. Grand Tree later | `quest_grandtree.rs2` — **Agility 25** hard gate | +460,000 att | Optional. 4,863 agility XP at 25× is small. |
| 9. Dragon Slayer later | `quest_dragon.rs2` — **32 QP** | +466,250 str/def | Rune plate unlock + more cb. Not the first 77. |

Waterfall **alone** is att/str 83 / **cb 56**, prayer 1. Stopping there was the under-strategized plan.

Priest in Peril also gives prayer (`14060` tenths) but needs 50 essence — skip if Restless Ghost already landed 47.

## Do not

- Lumbridge rats/cows as the combat path (Agent B A/B lost).
- Enter Waterfall dungeon at combat 3 / prayer 1.
- Survey every `stat_advance` in `server/content/scripts/quests/`.
- Walk the hill on the trainer.

## B's parallel job

Mule the Waterfall kit and time the 1 HP rejoin. Agent B reads
[`cloud-agent-b.md`](cloud-agent-b.md), not the operator file.
