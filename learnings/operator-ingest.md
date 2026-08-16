# Operator ingest — boards, then strategy

**Audience: the operator.** Not for Cloud A/B. Durable lane rules stay in
[`operator.md`](operator.md). North star and phases:
[`strategy-compete-koth.md`](strategy-compete-koth.md).

You evaluate the world. Agents execute lanes. They unstick themselves most
of the time. Your main job is to ingest live data and decide whether the
current plan is still the fastest path to a kill-and-rejoin, a contestable
KOTH minute, **and** a scarce-goods kit that survives a real PKer.

## What you optimize

| Lever | Default (until live data says otherwise) | Recheck when |
|---|---|---|
| **Leveling** | Witch’s House **now** (10 HP death spiral), then Waterfall (str dump), then TGV/Arena/Grail. Never Accurate. Do not grind def. Scorer has no stay-under cap. Pile is 80–(scorer−1). See stack file. | A finishes a quest; hill is empty of 123s |
| **Equips** | Hill trip: cheap replaceable. Configured-cost keep in `death.rs2`. Unskulled 3, skulled 0 unless PI. Do not copy goo’s rune 3-piece onto an attacker. Bank the elite kit off-hill ([`scarce-goods.md`](scarce-goods.md)). | `/hiscores/outfit` top kit or KOTH median loadout changes |
| **Consumables** | Cheap food for the 1 HP corridor. Waterfall: rope + 6 air/water/earth. Bones unnecessary after Restless Ghost (pray 47). | Next `.rs2` item list; mule inventory |
| **Scarce goods** | Runite + black d'hide are the trade goods ([`owner-context.md`](owner-context.md)). Bank-hiscore gold is a trap; a **6k mule treasury** is shop lubricant ([`scarce-goods.md`](scarce-goods.md)). Follow a new thread if A/B or the board surfaces one. | Outfit elite moves; a watched name farms a contested spawn; an agent reports a new skill, shop, or “can’t” |
| **Fleet** | Two VMs. **No standing mule.** A self-banks (Draynor/Falador). B = `kitprep1` warehouse + scheduled Draynor trade. Extra low-cb bodies add zero KOTH score. | kitprep finishes iron; 8-stack leaves |

Live boards beat a dated snapshot. This checkout beats OSRS memory.

## Visible vs not

`/playerpositions` is `{name, x, z, level}` — `level` is **plane**, not combat.
`/status/:name` is name + tile + controller, not HP. Outfit hiscores are last
non-empty logout kit (stale, no inventory/ammo/rings). KOTH sprites are a
per-slot median across captures, not one worn set. Boards do not show skull,
prayer, food, or who started the fight.

## Endpoints (no auth)

| URL | Use |
|---|---|
| `https://rs-sdk-demo.fly.dev/playerpositions` | JSON. Hill + our names + named swarms |
| `https://rs-sdk-demo.fly.dev/playercount` | World pop |
| `https://rs-sdk-demo.fly.dev/hiscores/koth?window=day\|week\|all&profile=main` | HTML. Minutes + last held. Day/week roll; use **all** for short deltas |
| `https://rs-sdk-demo.fly.dev/hiscores/outfit?profile=main` | HTML. Configured-cost kits |
| `https://rs-sdk-demo.fly.dev/hiscores/bank?profile=main` | HTML. Wealth prior |
| `https://rs-sdk-demo.fly.dev/hiscores/player/<name>?profile=main` | Skills + playtime + **rank outliers** (what they rushed). Not current activity. |
| `https://rs-sdk-demo.fly.dev/mapview/` | Eyes only |
| `https://rs-sdk-demo.fly.dev/status/<name>` | Our controller + tile |

Known tiles: polygon IN `(3288,3879)` / OUT `(3288,3878)`. Recurring 1+7:
scorer `(3288,3886)`, stack `(3284,3884)`. Names in
[`koth-swarm-snapshot-2026-08-15.md`](koth-swarm-snapshot-2026-08-15.md)
are a prior, not a hunt list.

## Every 60s tick

1. Positions: who is inside a ruins box (~3270–3310, 3860–3900), who is on
   the two 1+7 tiles, how many `goo*` are on the hill, where **our** three
   names plus any `goon*` ([`names.md`](names.md)) are, and where the
   [`top-players.md`](top-players.md) watch list is standing (`brotha`,
   `hoplite`, `goo001`, plus outfit-top-20 who are online).
2. KOTH day + all: top 5, last-held, all-time delta vs last tick if you
   cached it. One sample per wall-clock minute — that is why the loop is 60s.
3. Outfit + bank top 10 (HTML `data-item-id` / `title` — WebFetch strips
   canvases; curl the raw page). Flag gold-only banks.
4. A/B run status **and what they just said**. Peek the latest run /
   stream. **B never idle** (FINISHED → next physical job).
5. **Pull a thread if one showed up.** Something the agents (or the
   board) just surfaced — a new tile, skill number, shop price, “can’t”,
   a method they invented, a watch-list name doing a job we have no
   note for. Source-check it. Experiment on paper, or on B if that lane
   is idle. Do **not** re-score the same comparison you already wrote.
   If nothing new bubbled up, skip research this tick.
6. One sentence on the board: **hold** the current lanes, or **change**
   (empty hill, swarm returned, quest done, kit gap, gear elite moved,
   a thread beat the current plan).

Do not write a new snapshot file every tick. Write
[`top-players.md`](top-players.md) only when the top outfit kit or a
watched name’s **activity class** changed (hill / bank / mine / offline).
Formation notes stay short; live wins over the dated 8-stack snapshot.

## Research — follow threads, do not rotate a list

The agents are the sensor. Ideas come from *their* run: a jam, an
unexpected tile, a skill they just hit, a shop they opened, a sentence
they floated and we ignored. Your job is to pull that thread — look up
the `.rs2`, try the adjacent or *different* idea, see if it is faster
for the north star. It does not have to be a variant of the current
task. Fishing can beat mining even if nobody asked about fishing.

Do **not**:

- Re-check mine vs fish vs wc every tick
- Rotate gather / quest / kit on a schedule
- Invent a research slot when the streams are quiet
- Yank A off a correct quest step to “try something”
- Farm GP as a hiscore ([`owner-context.md`](owner-context.md)). Empty mule is a miss — guards → 6k treasury.

Do:

- Peek A/B streams and `ab-results-*.md` for something new
- Source-check before you believe the agent *or* the old table
- Write only if the thread changed a verdict
  ([`scarce-goods.md`](scarce-goods.md) is a prior, not a ritual)
- Run a small experiment on B when that lane is idle or already wrong;
  leave A alone if they are on the right physical step

When a quest is about to start or just finished, open **that** `.rs2`
for items and `stat_advance`. When the outfit or KOTH loadout board
moves, recompute keep order. When the hill is empty of maxed bodies, a
lower-cb stand becomes legal — only if `/playerpositions` actually
shows that.

## When to touch an agent

They recover from dialog, doors, and short script writes. You intervene
when the **lane** is wrong or dead:

- B run `FINISHED` with no next **progress** job (a 5-min sit on
  `foodprobe1` / `kitprep1` counts as idle)
- Walking a non-goal (cows, hill at low cb while 123s are on it, Taverley
  at pray 1, new bot names)
- Same known jam class **twice** and the tile is still (lookup, merge)
- Idle after a phase gate (quest done, kit in inv, no next POST)
- `qstboot1` about to melee (shed / Golrie) with no food in *his* inv

Do not cancel a run that is already on the right physical step. Do not
shove a nearby tile because a script has been quiet for one tick.

## Write-back

| What changed | Write |
|---|---|
| Formation / who scores | short note; live wins over the dated snapshot |
| Gear elite / watched tile class | [`top-players.md`](top-players.md) |
| New keep-kit, food meta, or a gather thread that paid off | [`strategy-compete-koth.md`](strategy-compete-koth.md) loadout box, or [`scarce-goods.md`](scarce-goods.md) |
| Next quest items | [`bootstrap-quest-stack.md`](bootstrap-quest-stack.md) only if the order/XP was wrong |
| Steer principle | [`operator.md`](operator.md) |
