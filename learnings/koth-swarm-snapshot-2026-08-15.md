# KOTH swarm and loadout snapshot — 2026-08-15

Read this after [`owner-context.md`](owner-context.md). This is a point-in-time
snapshot of public live-server state at approximately **2026-08-15 20:44 EDT**,
combined with the Discord reports preserved in
[`discord-meta-2026-08-15.md`](discord-meta-2026-08-15.md). The live boards move
continuously; account ownership and alliances are not public.

## What is directly visible

### An eight-account group was occupying the hill

Five checks of the public [`playerpositions`](https://rs-sdk-demo.fly.dev/playerpositions)
feed found the same eight accounts inside the KOTH area:

- `Tqckgxgj08`
- `D0c8tdgypo`
- `M17ikgb5kn`
- `U642zcnw4e`
- `Cdbova8vse`
- `Ohkyzlosps`
- `Tfloe12pjl`
- `Y8xdp1e99k`

All eight had 99 Attack, Strength, Defence, and Hitpoints. Their Prayer levels
were 81–89, while Ranged and Magic were below the public hiscore threshold.
Their stable co-location and near-identical builds are strong evidence of a
coordinated **eight-body hill formation**, but do not prove a common owner.

Visible kits were cheap and varied rather than best-in-slot:

| Account | Visible KOTH / outfit kit |
|---|---|
| `Tqckgxgj08` | Iron med helm, shortbow, leather vambraces |
| `D0c8tdgypo` | Steel med helm, bronze longsword, black square shield |
| `M17ikgb5kn` | Mithril med helm, iron scimitar, wooden shield |
| `Tfloe12pjl` | Iron med helm only |
| `U642zcnw4e`, `Cdbova8vse`, `Ohkyzlosps`, `Y8xdp1e99k` | Mostly default clothing / no visible combat kit |

At the snapshot, `Tqckgxgj08` led the daily board with about **1,053 capture
minutes**, versus about **336** for `Goo001`. On the weekly board, the order was
reversed: `Goo001` had about **6,671** and `Tqckgxgj08` about **1,605**. This
looks like a recent control change, not proof of a fight outcome.

### The `goo` fleet is larger, but was not all on the hill

`Goo001` through `Goo025` were all online in the sampled public positions feed.
Twenty distinct `gooNNN` accounts appear on the all-time KOTH board, with about
**8,874 combined capture minutes**. Among those twenty scorers, one was combat
126, eight were 124, and eleven were 123; every one had 99 Attack, Strength,
Defence, Hitpoints, and Ranged.

This supports a **25-account combat-ready fleet** and a **20-account historical
KOTH cohort**, not 25 simultaneous hill occupants. During the sample, fifteen
`goo` accounts were stacked on two adjacent underground tiles elsewhere and
`Goo001` had left the hill.

The recurring `goo` defensive kit was:

- rune scimitar — configured value 25,600 GP
- rune chainbody — configured value 50,000 GP
- rune platelegs — configured value 64,000 GP

Six `goo` accounts appeared on the equipment board with exactly this **139,600
GP three-item kit**. In the checked-in death rules, an unskulled defender keeps
its three highest configured-value worn-or-carried items. That makes this a
plausible standardized death-protected defender kit. It is not safe for the
account that initiates aggression: a skulled attacker normally keeps nothing,
or one item with Protect Item.

`Goo001` was the all-time leader at about **8,503 capture minutes**. Its older
representative capture outfit was only a mithril scimitar, while the daily
snapshot showed the standardized rune scimitar, chainbody, and platelegs.

## What the formations suggest

The strongest current strategy signals are:

1. **Stats and bodies matter more than expensive gear.** The active eight-body
   group uses maxed melee/HP accounts with disposable or absent visible gear.
2. **Standardize replacements around the death rules.** The `goo` three-piece
   rune kit is cheap, uniform, and protected only while the wearer remains
   unskulled and carries no higher-configured-cost item that changes the keep
   order.
3. **Use an anchor plus replaceable guards.** `Goo001` at combat 126 surrounded
   by combat 123–124 accounts is consistent with a highest-combat scoring
   anchor and interchangeable bodies. That role assignment is an inference,
   not something the public data proves.
4. **Occupancy can beat visible offensive power.** A combat-123 account wearing
   an iron helm and shortbow can lead the daily board because KOTH scores the
   highest-combat eligible contender present at the minute sample; it does not
   score equipment value.
5. **Keep control logic deterministic.** Discord's clearest four-bot pilot used
   deterministic tick policies, shared reporting, low-frequency planning, and
   no model calls in the hot loop.

## Other reported swarm sizes

These are Discord participant reports, not independently reproduced live data:

| Mission | Reported size | Reported method / result |
|---|---:|---|
| Thieving pilot | 4 | Ran 20m30s; reported 96.8k XP and 1,452 GP; bots reached Thieving 52/54/53/51 and all stopped on a 20-stun guardrail. Deterministic workers shared telemetry/curriculum while planning stayed low-frequency. |
| Fishing | 3, considering 10 | Reported 7.6k of a 30k-shark target, with an expressed plan to scale from three workers to ten. |
| Difficult PvM farm | At least 3 | Estimated one or two kills per trip using dragon battleaxes, teleporting after repeated large hits; poison and recoil damage were discussed. This was PvM, not a confirmed PK loadout. |
| Rune-node defense | 1 sentinel | A geofenced defensive account watched a mapped rune-mining area and engaged a miner. |
| Shield of Arrav | 2 | Coordination repeatedly failed until a human told the agents to listen to each other, showing that protocol quality can dominate fleet size. |
| Fishing / pickpocket farms | Claimed thousand-scale | Treat as unverified anecdotes. They support the abundance/inflation direction, not a reliable fleet count or rate. |

## What remains invisible

- Public position data shows names, coordinates, level, and reachability—not
  ownership, alliance, HP, prayers, skull state, food, inventory, policy, or
  controller architecture.
- Equipment hiscores are the last non-empty logout outfit, can be stale, and
  omit inventory and ammo. Rings are not visible.
- KOTH's representative outfit is aggregated per equipment slot across capture
  records, so it can synthesize a combination that was never worn at one time.
- The boards cannot tell us who initiated, what was focused, whether binds,
  poison, recoil, protection prayers, special attacks, or resupply were used,
  or how many accounts were controlled by one person.

## Public live sources

- [Current player positions](https://rs-sdk-demo.fly.dev/playerpositions)
- [KOTH — all time](https://rs-sdk-demo.fly.dev/hiscores/koth?profile=main)
- [KOTH — this week](https://rs-sdk-demo.fly.dev/hiscores/koth?profile=main&period=week)
- [KOTH — today](https://rs-sdk-demo.fly.dev/hiscores/koth?profile=main&period=today)
- [Equipment hiscores](https://rs-sdk-demo.fly.dev/hiscores/outfit?profile=main)
- [`Goo001` profile](https://rs-sdk-demo.fly.dev/hiscores/player/goo001?profile=main)
- [`Tqckgxgj08` profile](https://rs-sdk-demo.fly.dev/hiscores/player/tqckgxgj08?profile=main)
