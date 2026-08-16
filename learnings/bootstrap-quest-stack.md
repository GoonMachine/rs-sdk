# Combat bootstrap — 25× quest stack

**Shared fact.** Agent A executes this on `qstboot1`. The operator uses it to
steer. Do not put API ids or force-send tactics in this file.

Use this instead of cows. `stat_advance` values are **tenths**; production multiplies by **25** (Cook's Assistant live: 3000 → 7,500 cooking). Combat formula is [`Player.ts`](../server/engine/src/engine/entity/Player.ts) `getCombatLevel`. Wiki XP numbers are often 1× and stale.

Need **Prayer 25** (Protect Item) and **combat ≥ 77** to hit a 123 at wild 46 (`abs(cb − target) ≤ min(wild)`). **cb ≥ 80** to hit a 126. Protect from Melee is Prayer **43** (last prayer in this checkout — no Smite).

## Order on `qstboot1`

Live `qstboot1` after Vampire: **att 68 / str ~1 / def 1 / hp 10 / pray 47 / cb ~30**.
Waterfall is mid (`opened_book_on_baxtorian`) but **10 HP is a Lumbridge death spiral**
in Golrie. Do **not** grind defence or strength on cows to “prepare.” Equips yes;
stat prep is the next *quest*.

| Step | Source | Display XP (25×) | After (from fresh) |
|---|---|---|---|
| 1. Restless Ghost | `quest_priest.rs2` `stat_advance(prayer, 11250)` | +28,125 pray | **pray 47** — PI + prot melee. Lumbridge. Do this first. Father Urhney is **`(3235, 3153)`** (`m50_49.jm2` `0 35 17: 458`), SE swamp toward Al Kharid — **not** OSRS west-swamp `(3147,3174)` / `(3150,3176)`. |
| 2. Vampire Slayer | `quest_vampire.rs2` `48250` att | +120,625 att | att 68, **str still ~1**. Items: garlic (free, Morgan's house), hammer (gen store), beer (Harlow / bartender). Prot melee for Count Draynor. |
| 3. **Witch's House (cut in)** | `quest_ball.rs2` `hitpoints, 63250` | +158,125 hp | **HP ~54.** Do this **now** if Waterfall caves are killing a 10 HP body. Taverley. Boy (`ballboy` in `boy.rs2`) by the long garden north of town; Nora house ~`(2930, 3463)` (`0_45_54_50_7`). **Worn** `leather_gloves` for the iron gate (`quest_ball.rs2` — shock is `(hp/10)+1` without them). Cheese on the mouse hole. Thessalia gloves `(3204, 3417)`; Wydin cheese `(3014, 3204)`. Shapeshifter 21/31/41/51 hp. Prot melee. **Aggressive / Strength** (att is already 68). |
| 4. Waterfall | `quest_waterfall.rs2` `137500` att/str | +343,750 **each** | **This is the strength dump** — att 87 / str 83 / ~cb 65 after the HP cut-in. Prot melee. White Wolf = Taverley→Catherby **trail**, not south of z=3400. Rope Ned `(3100, 3258)`, 6 air/water/earth. Raft miss stays `started` — talk Hudon (`hudon.rs2`). Golrie ladder `loc_1754` **`(2533, 3155)`**; underground `(2515, 9581)`. Elkoy does **not** escort until TGV is started. |
| 5. Tree Gnome Village | `quest_tree.rs2` `114500` att | +286,250 att | att 94. Free accuracy. Same pocket as the pebble. Do **not** melee-train attack to “catch up.” |
| 6. Fight Arena | `quest_arena.rs2` `121750` att | +304,375 att | att 99. Side effect, not the goal. Skip Grand Tree after this (more att into a 99). |
| 7. Merlin's Crystal → Holy Grail | `quest_grail.rs2` `110000` pray + `153000` def | +275,000 pray / +382,500 def | **cb 90+**. This is the defence dump and the 77 jump. |
| 8. Dragon Slayer later | `quest_dragon.rs2` — **32 QP** | +466,250 **str and def** | Second strength dump + rune plate unlock. Champions Guild / Scavvo also wants 32 QP. |
| 9. Grand Tree | `quest_grandtree.rs2` — Agility 25 | +460,000 att | **Skip** once att is 99. |

Do **not** open a combat grind to raise str/def/hp before these quests. Shop iron
(Horvik) is the prep. 10 HP deaths are an HP problem, not a 1-def problem —
prot melee already zeros melee if it stays on. Witch’s House experiment is
**all melee** (crush/slash/stab). `qstboot1` is not underleveled for the shed.
Deaths happen when prot is off, prayer hits 0 (str 1 vs 144 HP can outlast
47 points), food is on another account, or they enter at 1 HP (shock =
`(hp/10)+1`). Do not cow-train. Do not pivot to Observatory / Nature Spirit
to “get HP first” — Nature Spirit needs Priest in Peril; Observatory is a
goblin dungeon at 10 HP for a much smaller dump.

Waterfall **alone** is att/str 83 / **cb 56**, prayer 1. Stopping there was the under-strategized plan.

Priest in Peril also gives prayer (`14060` tenths) but needs 50 essence — skip if Restless Ghost already landed 47.

## Combat goals — not “99 everything”

`getCombatLevel` in `Player.ts`:

```
floor( 0.25*(def + hp + floor(pray/2)) + 0.325*max(att+str, 1.5*range, 1.5*mage) )
```

Three different numbers. Do not collapse them into “train combat.”

| Goal | Number | How we get it |
|---|---|---|
| **Eligible** — can attack a 123 / 126 at wild 46 | **cb ≥ 77 / ≥ 80** | Finish the stack through Witch’s House / Holy Grail. Quest `stat_advance` ignores attack style. |
| **Score** — win a contested minute | **uniquely highest cb in the polygon** | Hill scorers sit **123–126** with 99 att/str/def/hp. The stack lands ~90–98. After that, train the **missing** melee stats — not more Attack. |
| **Win a fight** vs a real kit | max hit + stay alive | Strength (max hit), Defence/HP (tank), Prayer we already have (prot melee). Attack overflows from Vampire → Arena. |

**99 Attack is a Fight Arena side effect, not the target.** 99 all four melee stats is only the later KOTH-score grind, after the stack, if 123s are still on the hill. Magic/range only if we want binds or a different triangle — they do not help the melee term unless they beat `att+str`.

### Attack vs strength — we will not “train” the gap

Vampire only gives **attack**. Strength stays ~1 until **Waterfall** (equal att+str
dump) and later **Dragon Slayer** (str+def). TGV / Arena add **only attack** —
that is free quest XP, not an accident. The accident is swinging **Accurate**
and stacking more attack onto 68 while str is 1.

After the full stack: att 99 / str 83 / def ~63 / hp ~54. Then train
**Strength** (max hit) or leftover def/hp for 123. Never Attack. Skip Grand Tree.

### Attack style (only when we actually swing)

Quest `stat_advance` ignores style. Shapeshifter, Golrie, fire giants, warlord do not.

Read `sdk.getState().combatStyle.styles` and pick by `trainsSkills`. Do **not**
hardcode an index — a sword’s `2` is Aggressive/Strength; a scim’s `2` is Controlled.

| When | Train | Why |
|---|---|---|
| After Vampire, **before Waterfall** (str ~1) | **Aggressive / Strength** | Att is already 68. Shapeshifter / any leftover melee must not be Accurate. |
| After Waterfall, before Holy Grail | **Defensive / Defence** | Str is now ~83. Def is still ~1 until the Grail dump. |
| After Arena (att 99) and Grail | **Aggressive / Strength**, or Defensive if chasing 123 | Never Accurate. |
| Deliberate cb grind with a scim | Controlled only if we want all three | Slower max hit than Aggressive. |

`await sdk.sendSetCombatStyle(index)` after equip. Re-read the tab when the weapon changes. Hitpoints XP happens on every style.

## Brackets — what to stay under (and what not to)

`pvp_combat.rs2`: `abs(cb diff) ≤ min(the two players’ wild levels)`. Hill scorer tile `(3288,3886)` is **wild 46**. Window is ±46, not ±10.

| Our cb at wild 46 | Can attack |
|---|---|
| 77 | 31–123 — **misses 124–126** |
| 80 | 34–126 — first level that touches a 126 |
| 90 | 44–126 |
| 123 | 77–126 |
| 126 | 80–126 |

Classic RS “stay 50 / 60 / 70 combat” or “1-def so I only fight pures” is an **edge / wild 10–20** meta. At 46 it does not keep us off mains. A 50 cannot hit a 123 here (need 77). Do not build that.

### Skill gates that are real in this checkout

From `server/content/scripts/levelrequire/` (not OSRS memory):

| Stay-under / stop-at | Why | Who |
|---|---|---|
| **Attack 40 / 60** | Rune weapons; dragon dagger/long + Lost City. Halberd also wants Str 30 + Regicide. | Already cleared on `qstboot1` after Vampire / Waterfall. Do not park under 60 att. |
| **Defence 1** | Max hit per combat level. Cannot wear rune armour, d-med, d-chain, black hide *body* (40 def). | Only a future `goonpile` if we ever PK low wild. **Not** `qstboot1`. Holy Grail and Dragon Slayer dump def — skip both or you are not 1-def. |
| **Defence 40** | Rune chain/legs/helm. Platebody = 40 def **and** Dragon Slayer. Black hide body = 70 range + 40 def. | First “looks like goo” kit. Holy Grail (~63 def) overshoots this. |
| **Defence 45** | Viking / berserker helms (`tier45.rs2`). | Optional later pile. Not the first body. |
| **Defence 60** | Dragon med / dragon chain. | Holy Grail naturally lands ~60–65 def. Natural first *stop* if we want dragon armour without 99 def. |
| **Prayer 25 / 31 / 34 / 43** | PI, Ultimate Strength, Incredible Reflexes, Protect Melee. File ends at 43. | Take 43. More pray after that is only combat-level (and prayer points), not a new prayer. |
| **Ranged 70** | Black d'hide vambs/chaps; body also 40 def. | Scarce-goods kit, not the quest stack. |

Dragon Slayer is `186500` str **and** def — another 1-def / 40-def killer, plus the platebody unlock.

### Role split (do not put this on one character)

| Role | Combat cap | Def | Notes |
|---|---|---|---|
| **Scorer** (`qstboot1` → `goonscor`) | **None.** Push 123–126. | Do Grail + later Dragon Slayer. Train leftover def/hp/str. | Highest in the polygon wins. A stay-under cap loses contested minutes. |
| **Hill pile / PKer** | **Floor 80** (touch 126s). **Ceiling = scorer − 1** if they might stand in the polygon. | 40+ so rune wears; 60 if we have d-med. Not 1-def vs 123 mains. | Same wild-46 band as the target. Expected to skull. |
| **Low-wild / corridor** | Then 1-def or a 50–70 bracket can matter. | Skip Grail / DS. | Not the current job. The 1 HP walk is still deep wild (~45). |

`qstboot1` is the scorer path. Do not freeze him at 1 def or under 80 to “stay a PKer.” Make a second body if we want a 1-def.

## Do not

- Lumbridge rats/cows as the combat path (Agent B A/B lost).
- Train defence or strength **before** the quest that dumps that stat.
- Enter Waterfall dungeon at combat 3 / prayer 1, or Golrie at 10 HP once Witch's House is available.
- Survey every `stat_advance` in `server/content/scripts/quests/`.
- Walk the hill on the trainer.
- Accurate / Attack after Vampire.

## B's parallel job

Mule the Waterfall kit and time the 1 HP rejoin. Agent B reads
[`cloud-agent-b.md`](cloud-agent-b.md), not the operator file.
