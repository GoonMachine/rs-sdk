# Operator ingest — boards, then strategy

**Audience: the operator.** Not for Cloud A/B. Durable lane rules stay in
[`operator.md`](operator.md). North star and phases:
[`strategy-compete-koth.md`](strategy-compete-koth.md).

You evaluate the world. Agents execute lanes. They unstick themselves most
of the time. Your main job is to ingest live data and decide whether the
current plan is still the fastest path to a kill-and-rejoin, then KOTH
minutes.

## What you optimize

| Lever | Default (until live data says otherwise) | Recheck when |
|---|---|---|
| **Leveling** | [`bootstrap-quest-stack.md`](bootstrap-quest-stack.md) on `qstboot1`. Restless Ghost first. Do not stop after Waterfall. | A finishes a quest; hill is empty of 123s (highest-cb-in-polygon could score earlier) |
| **Equips** | Cheap replaceable. Configured-cost keep in `death.rs2`. Unskulled 3, skulled 0 unless PI. Do not copy goo’s rune 3-piece onto an attacker. | `/hiscores/outfit` or KOTH median loadout changes |
| **Consumables** | Cheap food for the 1 HP corridor. Waterfall: rope + 6 air/water/earth. Bones unnecessary after Restless Ghost (pray 47). | Next `.rs2` item list; mule inventory |
| **Fleet** | Two lanes, then a 4-account canary. Extra low-cb bodies add zero score. | 8-stack or goo leaves the hill; our scorer exists |

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
| `https://rs-sdk-demo.fly.dev/hiscores/player/<name>?profile=main` | Combat / skills for a name on the hill |
| `https://rs-sdk-demo.fly.dev/mapview/` | Eyes only |
| `https://rs-sdk-demo.fly.dev/status/<name>` | Our controller + tile |

Known tiles: polygon IN `(3288,3879)` / OUT `(3288,3878)`. Recurring 1+7:
scorer `(3288,3886)`, stack `(3284,3884)`. Names in
[`koth-swarm-snapshot-2026-08-15.md`](koth-swarm-snapshot-2026-08-15.md)
are a prior, not a hunt list.

## Every 60s tick

1. Positions: who is inside a ruins box (~3270–3310, 3860–3900), who is on
   the two 1+7 tiles, how many `goo*` are on the hill, where **our** three
   names are.
2. KOTH day + all: top 5, last-held, all-time delta vs last tick if you
   cached it. One sample per wall-clock minute — that is why the loop is 60s.
3. A/B run status. **B never idle** (FINISHED → next physical job).
4. One sentence: **hold** the current lanes, or **change** (empty hill,
   swarm returned, quest done, kit gap).

Do not write a new snapshot file every tick. Write only when the formation
or the leader changed in a way that should change our phase.

## Research (operator, not the VM)

When a quest is about to start or just finished, open **that** `.rs2` for
items and `stat_advance`. Then shops / drops in `wiki/` after the source
check. Angle the mule (B) at the next kit before A walks there.

When the outfit or KOTH loadout board moves, recompute keep order. Food and
PI beat a high-config weapon that knocks food off the keep list.

When the hill is empty of maxed bodies, a lower-cb stand becomes legal
(highest combat in the polygon wins). Do not send `qstboot1` there until
that is actually true on `/playerpositions`.

## When to touch an agent

They recover from dialog, doors, and short script writes. You intervene
when the **lane** is wrong or dead:

- B run `FINISHED` with no next job
- Walking a non-goal (cows, hill at low cb while 123s are on it, Taverley
  at pray 1, new bot names)
- Same known jam class **twice** and the tile is still (lookup, merge)
- Idle after a phase gate (quest done, kit in inv, no next POST)

Do not cancel a run that is already on the right physical step. Do not
shove a nearby tile because a script has been quiet for one tick.

## Write-back

| What changed | Write |
|---|---|
| Formation / who scores | short note; live wins over the dated snapshot |
| New keep-kit or food meta | [`strategy-compete-koth.md`](strategy-compete-koth.md) loadout box, or a one-liner on the agent paste |
| Next quest items | [`bootstrap-quest-stack.md`](bootstrap-quest-stack.md) only if the order/XP was wrong |
| Steer principle | [`operator.md`](operator.md) |
