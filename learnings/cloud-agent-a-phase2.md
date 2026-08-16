# Agent A — quest stack on `qstboot1`

**Audience: Cloud Agent A only.** Do not read `operator.md`.

Paste into the **existing** Agent A thread
(`bc-9ef936bf-19d3-4e78-bc92-189fe6d15015`). Do not start a third Cloud agent.

Full order and XP table: [`bootstrap-quest-stack.md`](bootstrap-quest-stack.md).

---

Phase 1 is done. Do not re-measure tick, Cook's, or the polygon.

`qstboot1` only. Kill leftover `lite-qstprobe1`. Skip tutorial if needed.

**Restless Ghost first** (you spawn in Lumbridge). `quest_priest.rs2`
`stat_advance(prayer, 11250)` × 25 = **+28,125 prayer → level 47**. That is
Protect Item and Protect from Melee. Do not walk to Baxtorian at combat 3 /
prayer 1.

Urhney is **`(3235, 3153)`** ([`lookup.md`](lookup.md)), not OSRS `(3147,3174)`.
Aereck: **"I'm looking for a quest!"** ([`dialog.md`](dialog.md)).

Then, one quest at a time, source `.rs2` first, wiki last, fail fast 10–30s,
write `ab-results-a.md` + commit `cloud/ab-a` after each complete
(conflict → [`merge.md`](merge.md); quest step first):

1. Vampire Slayer (`quest_vampire.rs2`) — **done** (att 68, str still ~1)
2. **Witch’s House now** if Waterfall caves are killing 10 HP
   (`quest_ball.rs2`). Taverley. Worn leather gloves + cheese. Prot melee.
   Combat style **Aggressive / Strength** — do not train Attack. Then
   Waterfall (`quest_waterfall.rs2`) — that is the strength dump. Prot melee.
   White Wolf = Taverley → Catherby **trail**. Rope, 6 air, 6 water, 6 earth.
   Shop and bank **yourself** (Falador `(2945,3366)`, Draynor `(3092,3243)`).
   **Food in your inventory** before the shapeshifter / Golrie. 10 HP:
   eat on first damage. You are **not** underleveled for the shed —
   all four forms are melee (`witches_house.npc`); prot melee zeros
   them. Two deaths were prot-off / prayer-expire / no food / 1 HP
   shed, not “need more combat.” Shed gate: (1) food count in inv,
   (2) Taverley altar, prot **off** until the shed door, (3)
   `bot.activatePrayer('protect_from_melee')` then enter, (4) eat if
   `hp < maxHp`, (5) if `prayerPoints === 0` **leave** — wolf str 40
   one-shots 10 HP. Str 1 makes 144 HP slow; 47 pray can expire.
   Worn gloves (shock is `(hp/10)+1` — kills a 1 HP body). In any combat loop:

   ```
   const hp = sdk.getState().player.hp;
   if (hp < sdk.getState().player.maxHp) {
     const food = sdk.findInventoryItem(/cheese|shrimp|anchov|bread|lobster/i);
     if (food) await bot.eatFood(food);
   }
   ```

   `/status` is not HP. Prot melee stays on. Never Accurate.
3. Tree Gnome Village → Fight Arena (more attack, free; never Accurate)
4. Merlin’s Crystal → Holy Grail (defence dump, cb 77+)

Do **not** grind defence/strength on cows. Do **not** stop after Waterfall.
No hill, no extra bots, no quest-script survey, no foodprobe1 control.
