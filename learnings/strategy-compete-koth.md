# Strategy: compete, counter, then hold

**North star:** prove we can kill and rejoin, then convert that into KOTH
capture minutes. Do not clone the 8-body junk stack or the 25-account goo
bench.

If a dated snapshot and live `/playerpositions` disagree, **live wins**.
If Discord and this checkout disagree, **source wins** until a live probe
says production drifted.

Server diffs vs OSRS / wiki: [`server-diffs.md`](server-diffs.md).
Evidence (leads, not doctrine): [`owner-context.md`](owner-context.md),
[`discord-meta-2026-08-15.md`](discord-meta-2026-08-15.md),
[`koth-swarm-snapshot-2026-08-15.md`](koth-swarm-snapshot-2026-08-15.md).

## Source hierarchy

Use this order in every decision. Do not flatten it.

1. **Live probe** on `rs-sdk-demo.fly.dev` — positions, a disposable death, a
   10–30s script.
2. **This checkout** — [`Koth.ts`](../server/engine/src/engine/Koth.ts),
   [`pvp_combat.rs2`](../server/content/scripts/skill_combat/scripts/pvp/pvp_combat.rs2),
   [`death.rs2`](../server/content/scripts/player/scripts/death.rs2),
   [`pvp_death_mark.rs2`](../server/content/scripts/skill_combat/scripts/pvp/pvp_death_mark.rs2),
   [`mine.dbrow`](../server/content/scripts/skill_mining/configs/mine.dbrow).
3. **Max owner posts** in [`owner-context.md`](owner-context.md).
4. **Discord / live snapshots** — human reports. Useful size priors, not rates
   or loadouts.
5. **In-repo wiki** last, and only after a source check.
6. **External 2004 / LostCity sources** for era mechanics. Not current OSRS
   Wiki PK meta (BH, gear switches, 2024 wildy ditch, GE prices).

## Counter thesis

Observed 2026-08-15 (snapshot + a later live re-check): the same eight names
held the ruins; all 25 goo accounts were online and **none** were on the hill.

Their visible pattern is occupancy + replaceable maxed bodies. Our edge is
this server's PvP death rules, which modern guides get wrong: a PvP death
sends them to Lumbridge at **1 HP** and the death mark keeps every death at
1 HP for ~1000 ticks. A kill buys a long weak walk-back. Pile one, occupy the
minute sample, repeat.

| What they do | Weakness | Our response |
|---|---|---|
| 8 maxed melee, junk/empty kits, stacked on `(3284,3884)` + one on `(3288,3886)` | Low visible DPS/tank; stacked = easy multiway pile; no tele off wild 46 | One slightly higher-cb **unskulled scorer**; 2–3 **skulled pile** accounts with food + Protect Item consumables only; the killer loots during private drop |
| goo 25 / 20 historical scorers, rune 3-piece kit, often elsewhere | Kit is death-safe only unskulled; 15 were not defending | Do not fight their bench. Contest the hill while they are away. If they return, pile the scorer, not the kit |
| Daily board led by a shortbow iron-helm account | KOTH ignores gear | Do not buy rune to look like goo. Buy food, prayers, and replacement bodies |
| Deterministic workers, no model on the tick | Coordination is the scarce skill | Same: lite hot loops; model only for plan/recover |

Do **not** start with 8 or 25 accounts. Start with 4 once combat exists. Extra
low levels add **zero** KOTH score.

```
Eight junk kits on hill
        → focus-fire one body
        → PvP death: Lumbridge 1hp + death mark
        → long walk back at 1hp
        → our highest-cb account stands the minute sample
```

## Role contracts (four-account canary)

Names: max 12 alphanumeric. Create with `bun bots/create-bot.ts <name>`.
Never commit `bots/*/`.

| Role | Job | Skull | Combat |
|---|---|---|---|
| **Scorer** (trainer first) | Uniquely highest combat among *our* bodies. Stands inside the polygon for the minute sample. Never initiates. | Never | Highest |
| **Pile A / Pile B** | Same combat band as the target. Focus one challenger in multiway. Expected to skull. | Yes | Compatible with wild 46 |
| **Mule / recovery** | Food, cheap kits, Lumbridge 1 HP pickup, rejoin path. Trades, does not ground-drop scarce goods. | No | Irrelevant |

Later, only after source-checked Magic 20/50/79: a **binder** (10-tile range,
account for Protect Magic and the 5-tick post-freeze immunity). A **caller**
watches `/playerpositions`, crown range (24 tiles of `(3289,3886)`), and the
return corridor. Crowns fire only on holder change; they do not prove each
minute scored.

## Loadouts

Calculate the full worn-plus-carried **configured-cost** keep order before
every trip. See [`death.rs2`](../server/content/scripts/player/scripts/death.rs2).

- **Scorer (unskulled):** three keep slots. Cheap, replaceable. Do not wear a
  high-config item that pushes food or PI off the keep list. goo’s rune
  scim + chain + legs (139,600 configured) *fits* unskulled 3-item keep in
  this checkout — that is an inference, not a kit we copy onto attackers.
- **Pile (skulled):** assume 0 kept unless Protect Item is on, then 1. Carry
  one deliberate PI candidate plus food you will lose. Never a rune 3-piece.
- **Mule:** holds replacements off the hill. Use `bot.trade` /
  `bot.serveTrades`, not the pickpocket swarm’s public drop handoff.

Failure is a lost rune kit or a wedged script. Success is a kill, a 1 HP
rejoin, and later a capture minute.

## Phases

Do not skip ahead. `agentmachine` was last a near-fresh Lumbridge combat-3
account. It cannot attack a combat-123 at wild 46.

### 1. Fact-check before any hill walk

10–30s scripts. Tick the boxes in [`server-diffs.md`](server-diffs.md).
Split the boxes across two Cloud agents (table in Phase 2). Do not both run
every probe.

- Measure prod tickrate and whether quest XP is 25x. **Agent A**
- Scout the live polygon vs `Koth.ts` vertices. **Agent A**
- One junk-only PvP death in low wild to confirm 1 HP + death-mark duration. **Agent B**
- Re-read `https://rs-sdk-demo.fly.dev/playerpositions` and
  `/hiscores/koth` at the start of every session. The 8-stack can vanish. **Agent B**

### 2. Combat bootstrap (not KOTH) — two-agent A/B

Two Cloud agents. Phase 1 is a **split of independent probes**, not an A/B.
Phase 2 is the **only** A/B: how we get combat. Do not A/B the north star,
rune vs junk kits, 4 vs 8 bodies, or two fleets on the same hill.

Reserved bot prefixes (max 12 alphanumeric). Do not touch `agentmachine`.

| Agent | Phase 1 (parallel) | Phase 2 (A/B) | Bots |
|---|---|---|---|
| **A** (quest) | Prod tick clock; quest XP via Cook's Assistant (`stat_advance(cooking, 3000)` in `quest_cook.rs2`); polygon stand-in / stand-out | Waterfall first (`stat_advance(attack/strength, 137500)` tenths in `quest_waterfall.rs2`). Later quests only after a source check of that quest script | `qstprobe1`, `qstboot1` |
| **B** (food) | Junk-only PvP death in **low wild** (two fresh accounts; not the hill); death-mark duration; `/playerpositions` + `/hiscores/koth` | Lumbridge rats / goblins / cows + bury bones. Infinite run helps travel, not DPS | `foodprobe1`, `foodkill1`, `foodboot1` |

Paste-ready launch text: [`cloud-agent-a.md`](cloud-agent-a.md),
[`cloud-agent-b.md`](cloud-agent-b.md). Write results to
[`ab-results-a.md`](ab-results-a.md) / [`ab-results-b.md`](ab-results-b.md).

Winner of Phase 2 is **wall-clock minutes** to both:

- Prayer 25 (Protect Item) — [`prayers.dbrow`](../server/content/scripts/skill_prayer/configs/prayers.dbrow)
- Combat ≥ 77 (so `abs(cb − 123) ≤ 46` at the hill)

Stop the slower path when one is clearly ahead. A third Cloud agent is spare
concurrency, not more information.

- Unlock Protect Item and protection prayers before any wild trip.
- Stand up the mule with food and unskulled keep kits.

### 3. Four-account canary, still off the hill

Lite clients, not Chromium:

```
cd server/webclient && bun src/lite/runner.ts <botname>
```

Reuse the in-process pattern in
[`swarm.ts`](../server/webclient/src/lite/swarm.ts) and `bot.serveTrades`:
deterministic workers, staggered starts, exclusive leases.

Prove: login, skip tutorial, mule trade, death → 1 HP → rejoin timer. No
Demonic Ruins until that loop is boring.

### 4. First PK, then minutes

- Scout watches the 8-stack and goo. Do not send the scorer in first.
- Pile **one** of the stacked junk kits. Killer loots. Scorer steps onto
  `(3288, 3886)` for the next minute sample.
- Success: a kill, a 1 HP rejoin, and at least one capture minute.
- Only then add a binder if Magic is source-checked.

### 5. Scale only on metrics

4 → 6–8 only if all of these hold:

- Productive ticks / account-hour stay up after self-contention.
- Death-to-rejoin is shorter than the opponent’s (death mark ~5–7 min; beat it).
- Scorer is uniquely highest combat among our bodies (equal-max first sample
  is random).
- Model calls per worker-hour stay low.

Do not go to 25. Do not start 100 Cloud Agents.

## Control plane

| Layer | What | Tool |
|---|---|---|
| Hot loop | Walk, pile, stand the sample, mule, rejoin | Lite runner + deterministic scripts on **one** Cloud VM |
| Cold loop | “Are we still on the hill?”, rewrite a script, relaunch a dead bot | Later: one cron Automation that inspects and restarts. Never clicks |

Cursor Pro concurrent Cloud Agents ≈ 8. One VM can run several lite clients
(~14 MB each). Automations spawn billed Cloud Agents; they are a supervisor,
not a woodcutter.

Use **two** Cloud agents for Phase 1+2 as the table above. Keep the hot loop
on **one** VM later. Do not give both agents the same bot names.

## Non-goals

- Cloning goo’s 25 or the 8-stack’s junk kits as a first fleet.
- GP / pickpocket / KBD pile farms as a primary goal.
- Chromium tabs on Cloud.
- Putting a model on every game tick.
- Vendoring the X-research skill or a Discord scraper into the hot loop.
- Treating the next Codex Discord dump as production truth until it is
  source-checked and live-probed.
- Committing `bots/*/` or printing `bot.env`.

## Session start checklist

1. `git pull origin main`
2. Read this file, [`server-diffs.md`](server-diffs.md), then
   [`owner-context.md`](owner-context.md)
3. Fetch `/playerpositions` and `/hiscores/koth`
4. Run the current phase only. Fail fast (10–30s) before a 5-minute grind
