# Server diffs — this world is not current OSRS

Read this before applying Discord, X, the in-repo wiki, or any modern OSRS PK
guide. This checkout talks to Max's **agent-only 2004scape demo** (LostCity
fork) at `rs-sdk-demo.fly.dev`. Advice written for 2024–2026 Old School
RuneScape (GE prices, bounty hunter, wildy ditch, gear switches) is usually
wrong here.

Compete-and-counter plan: [`strategy-compete-koth.md`](strategy-compete-koth.md).
Live meta: [`owner-context.md`](owner-context.md).

If Discord and this checkout disagree, **source wins** until a live probe says
production drifted.

## Verify on production

Checkout values below are from this tree. `Koth.ts` notes prod ticks at 300ms
while local default is 400ms. Do not forecast from unchecked boxes.

- [x] Prod tick length (watch `sdk.getState().tick` vs wall clock) — **agent A**
      **300.3 ms/tick** on `qstprobe1` (100 ticks / 30,025 ms), 2026-08-15.
      Use 300ms for duration forecasts.
- [ ] Quest XP is multiplied by `xpRate` — **agent A**, measure Cook's Assistant
      first (`stat_advance(cooking, 3000)` tenths in `quest_cook.rs2`; 300 XP ×
      rate). Do not wait for Waterfall to tick this box. Waterfall lists 13,750
      att/str in [`wiki/quests/waterfall-quest.md`](../wiki/quests/waterfall-quest.md)
      (`137500` tenths in `quest_waterfall.rs2`).
- [x] PvP death respawns Lumbridge at 1 HP — **agent B**, 2026-08-16: two junk
      accounts, Edgeville wild 4. Victim respawned (3222,3218) at 1 HP (`death.rs2`
      `stat_sub(hitpoints,…,1)`); kept 3 highest-cost items, dropped 15. Clean 1-HP
      frame captured via the marked NPC-death respawn 1/18 (Codex's earlier read saw
      `(3219,3219)` 2/10 mid-regen and missed frame 1). See `ab-results-b.md`.
- [x] Death mark still blocks NPC-suicide full-heal — **agent B**, 2026-08-16:
      marked victim died to a Dark wizard at PvP+174s → respawn **1/18 HP**; the
      same NPC death at PvP+510s (mark expired) → **18/18 HP**. Duration
      `^pvp_death_mark_duration=1000` × 300.3 ms ≈ 300s. See `ab-results-b.md`.
- [ ] KOTH polygon matches [`Koth.ts`](../server/engine/src/engine/Koth.ts)
      vertices (stand just inside / just outside) — **agent A**, disposable junk,
      do not attack
- [x] `/playerpositions` and `/hiscores/koth` still match the dated snapshot —
      **agent B**, 2026-08-16: 8-stack names all present and back on the hill
      (~(3287,3885), `Tqckgxgj08` on crown, `Goo001` adjacent, both capturing);
      15 `goo` parked underground. Consistent with Codex's recurring 1+7 / brief
      all-inside convergence ([`live-probes-2026-08-15.md`](live-probes-2026-08-15.md)).
      See `ab-results-b.md`.
- [x] Production death-keep selector matches source for one disposable fixture:
      worn shield + carried dagger + 25 arrows returned shield + dagger + one
      arrow; 24 arrows dropped to the killer. Skulled/PI branches remain source-only.

## Diffs that change advice

| Topic | This server (source) | What online guides assume | Agent implication |
|---|---|---|---|
| Revision | LostCity 2004scape fork. No Grand Exchange. Barter. | Current OSRS + GE | Ignore GE prices, bonds, BH, and 2024 wildy ditch meta |
| XP | Checkout `xpRate: 25` in [`WorldConfig.ts`](../server/engine/src/util/WorldConfig.ts) | 1x OSRS curve | Faster combat bootstrap; still verify quest XP on prod |
| Tick | Local default 400ms ([`Environment.ts`](../server/engine/src/util/Environment.ts) `NODE_TICKRATE`). [`Koth.ts`](../server/engine/src/engine/Koth.ts) says prod 300ms | 600ms OSRS | Convert tick durations with the **measured** prod tick |
| Run / randoms | Infinite run, no random events ([`README.md`](../README.md)) | Run energy + randoms | Travel is cheap; DPS and food still matter |
| KOTH | Custom. Highest combat in a Demonic Ruins polygon, one capture per wall-clock minute. No combat-85 floor, no solitude. Multiway. Crowns only within 24 tiles of `(3289, 3886)`. [`Koth.ts`](../server/engine/src/engine/Koth.ts) | No such minigame | Gear does not score. Extra low levels add zero minutes |
| Wild attack | `abs(cb diff) <= min(the two players' wild levels)` in [`pvp_combat.rs2`](../server/content/scripts/skill_combat/scripts/pvp/pvp_combat.rs2) | Same era rule, often misquoted | Hill is ~wild 46. Combat 3 cannot touch combat 123 there |
| Teleports | Standard teles fail above wild 20 ([`teleport.rs2`](../server/content/scripts/skill_magic/scripts/spells/teleport.rs2)) | Many later escapes / jewelry | Escape is walk, fight, or resupply. No tele off the hill |
| Death keep | Unskulled: 3 highest **configured** values. Protect Item adds one. Skulled: 0 unless PI. Stacks are not one item. [`death.rs2`](../server/content/scripts/player/scripts/death.rs2) | GE-value keep, later item lists | Static config cost ≠ barter value. Do not assume the economy's favorite item is kept |
| PvP respawn | PvP death → Lumbridge at **1 HP**. [`pvp_death_mark`](../server/content/scripts/skill_combat/scripts/pvp/pvp_death_mark.rs2) keeps every death at 1 HP for `^pvp_death_mark_duration = 1000` ticks (~5 min at 300ms). Comment in [`pvp.constant`](../server/content/scripts/skill_combat/configs/pvp.constant). Cannot NPC-suicide to full HP | Full HP respawn; suicide-to-reset | A kill buys a long weak walk-back. That is the counter to an 8-stack |
| Skull | `^pk_skull_duration = 2000` ticks ([`pvp.constant`](../server/content/scripts/skill_combat/configs/pvp.constant)) | 20 min OSRS skull | ~10 min at 300ms, ~13 min at 400ms. Measure, don't assume |
| Caps | 200 logins/IP ([`World.ts`](../server/engine/src/engine/World.ts) `PLAYER_MAX_PER_IP`). World 2048 | No such IP cap | One Cloud VM can run many lite clients. Do not start 100 Cloud Agents |
| Hiscores | Total level then playtime. Cap 1881 (19×99) | OSRS hiscores / GE | Fastest max is not the open frontier |

## Sources that are safe vs unsafe

**Safe as era mechanics (still probe):** LostCity / 2004scape docs, period videos
cited in the `.rs2` files, this checkout's content scripts.

**Leads only:** Discord, X, [`koth-swarm-snapshot-2026-08-15.md`](koth-swarm-snapshot-2026-08-15.md).
Human opinion. Ownership, prayers, skull, food, and controller logic are not
visible on the public boards.

**Last, and only after a source check:** in-repo `wiki/`. Already stale on
mining 70 vs 85 (`mine.dbrow`) and hide colors (`wiki/items/dragonhide.md`).

**Do not use for loadouts or PK:** current OSRS Wiki PvP, GE, bounty hunter,
BH worlds, multi-way switches, or “protect the most expensive GE item.”
