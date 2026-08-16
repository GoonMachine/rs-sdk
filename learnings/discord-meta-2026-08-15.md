# rs-sdk Discord field notes — 2026-08-15

Read this with [`owner-context.md`](owner-context.md). This file preserves the
evidence behind the current economy, swarm, and Wilderness strategy; the owner
context is the shorter decision-ready briefing.

## Scope and reliability

- Source: the current `rs-sdk` Discord, read through the Discord desktop UI on
  2026-08-15. Server id: `1468002571644833846`.
- Sampled channels: `general`, `99str-contest`, `agent-techniques`, and
  `share-progress`, plus server-wide search results for `pk`.
- This was a read-only sample of visible and search-returned messages, not a
  complete Discord export. No messages, reactions, or settings were changed.
- Discord posts are third-party claims. They are evidence of what participants
  report doing, not automatically game truth. Mechanics below were checked
  against the local server source where possible.
- Discussions about evading detection on official OSRS were excluded. This
  project targets the owner-sanctioned agent-only 2004scape demo server.

Confidence labels used below:

- **Owner report** — a statement by `maxbittker`, the rs-sdk owner.
- **Demonstrated** — a participant described a concrete run or implementation,
  sometimes with an image/video, but it was not independently reproduced here.
- **Anecdote** — a single community claim or proposal.
- **Source-checked** — behavior found in this checkout's server code. Production
  may still lag the local checkout, so live behavior should be probed safely.

## What people report doing

| Date / channel | Observation | Confidence | Strategic meaning |
|---|---|---|---|
| 2026-08-15, `general` | An embedded post from `maxbittker` described cheap labor making ordinary commodities abundant, weak demand for cash, goods-for-goods trade, and heavy contention for fixed-respawn resources. | Owner report | Measure wealth in scarce goods and access time, not headline GP. |
| 2026-08-15, `share-progress` | `ijohndoe` reported a four-bot LiteClient pilot using deterministic tick policies, shared reporting/curriculum, low-frequency planning, no model calls in the hot loop, and a 20-stun stop guardrail. | Demonstrated | Start with a small canary; keep the model in the planning/recovery layer and validated loops deterministic. |
| 2026-07-27, `share-progress` | `Nick` said combat farmers banked at three inventory items, or immediately after a noted/rare drop. `maxbittker` said he killed roughly 20 on their return route and obtained little of value. | Two matching reports | Bank routes are attack surfaces. Cap loot exposure and bank instantly on rare/noted drops. |
| 2026-03-31, `share-progress` | `maxbittker` reported that an account killed a bot he had placed to PK rune miners. `1G` described it as a defensive script and confirmed it watched a mapped area. | Demonstrated | A geofenced sentinel at a scarce node is already a known pattern; the likely next contest is approach and return-route control. |
| 2026-02-10 to 2026-04-28, server search for `pk` | Search results showed the first reported PK, opportunistic PK while clueing, a bot positioned against rune miners, and multiple attempts to teach agents PK. One participant warned that evolved modern-OSRS PK intuition does not map cleanly onto 2004scape. | Owner reports + anecdotes | Use this revision's source and short live probes, not modern OSRS guides, as the ruleset. |
| 2026-03 to 2026-04, `share-progress` | Participants described very large fishing and pickpocket fleets, including thousand-account-scale GP/fish claims. The exact rates were not verified. | Anecdotes | The quantities are unreliable, but the direction matches the observed abundance and GP inflation thesis. Do not compete by printing more undifferentiated cash. |
| 2026-03 to 2026-04, `share-progress` | Two agents repeatedly failed to coordinate Shield of Arrav until a human reminded them to listen to their partner. | Demonstrated | Coordination and recovery protocols are a bigger early bottleneck than raw account count. |
| 2026-05 to 2026-06, `agent-techniques` | `1G` described using a full headless browser for routes with ropes, trees, doors, and other transitions, then switching to GoThin for a stable combat loop. Participants described `rsmod-pathfinder` as useful for overland travel but weak on ladders, stairs, boats, and teleports. | Demonstrated | Use a heavy setup/recovery adapter and lightweight workers; cache successful transition routes explicitly. |
| 2026-02 to 2026-07, `99str-contest` | Quest-based combat bootstrapping was faster than pure grinding. A level-3 Waterfall attempt needed an adaptive rat/food fallback; later advice stacked Waterfall, Grand Tree, Tree Gnome Village, Fight Arena, and Vampyre Slayer. | Demonstrated + anecdotes | Build replacement combat accounts through validated quest prerequisites, with fallback states instead of brittle end-to-end scripts. |
| 2026-07-02, `share-progress` | A difficult combat script teleported after repeated large hits; the author estimated at least three bots for one or two kills per trip and discussed poison and recoil damage. | Demonstrated PvM, not PK | Shared retreat thresholds and additive damage sources transfer, but must be revalidated for PvP and deep-Wilderness teleport limits. |
| 2026-07-11, `share-progress` | `Ral` said screenshots were requested only when a control-plane view was active, saving worker performance. | Demonstrated | Prefer state/metrics telemetry continuously and turn expensive visual telemetry on only for diagnosis. |
| 2026-08-08 to 2026-08-12, `share-progress` | Medium clues were fully scripted, while hard clues still needed manual help; the participant reported hundreds of completed clues. | Demonstrated | Automate known-state routines and escalate ambiguous puzzles instead of putting a model in every tick. |

## Mechanics checked against this checkout

These checks supersede the handwritten wiki where they disagree. They still
need a disposable live probe before risking valuable accounts or gear.

### King of the Hill and PvP

- KOTH is implemented as a precise polygon at the Demonic Ruins around
  `(3289, 3886)`, about Wilderness level 46. It awards one capture per
  wall-clock minute. See
  [`Koth.ts`](../server/engine/src/engine/Koth.ts).
- There is no combat-85 floor and no solitude requirement in the checked-in
  implementation. Every visible, non-staff player inside is a contender. The
  highest-combat contender scores; the incumbent wins equal-combat ties, and a
  new equal-max tie is random. An empty hill clears incumbency.
- Demonic Ruins is multiway in
  [`multiway.csv`](../server/content/maps/multiway.csv), so multiple accounts can
  focus one target rather than being blocked by the ordinary single-combat lock.
- Wilderness attack eligibility is
  `abs(combat-level difference) <= min(the two Wilderness levels)`. Weak support
  accounts may therefore be unable to attack a maxed challenger even at the
  level-46 hill. See
  [`pvp_combat.rs2`](../server/content/scripts/skill_combat/scripts/pvp/pvp_combat.rs2).
- First aggression normally skulls the attacker. Unskulled players keep three
  configured-highest-value items, plus one with Protect Item; skulled players
  keep none, or one with Protect Item. Protection uses static configured cost,
  not the live barter value, so the game can preserve the economically wrong
  item. See [`pk_skull.rs2`](../server/content/scripts/skill_combat/scripts/pvp/pk_skull.rs2)
  and [`death.rs2`](../server/content/scripts/player/scripts/death.rs2).
- A PvP death returns the account to Lumbridge at 1 HP and applies a temporary
  death mark. At the deep-Wilderness hill, ordinary spell teleports are already
  blocked; glory and Ring of Life are also above their Wilderness limits.
- Protection prayers reduce PvP damage rather than fully blocking it. Protect
  Magic also shortens freezes. Bind/Snare/Entangle require a controller that
  can keep the caster near the target.
- A KOTH hiscore route exists at `/hiscores/koth`. Logging configuration can
  affect whether capture events persist, so the live board is also a deployment
  health check.

### Scarcity and trade

- The authoritative mining table requires **85 Mining** for runite and gives a
  base respawn of 2,400 ticks. The handwritten
  [`wiki/skills/mining.md`](../wiki/skills/mining.md) says 70 and is stale. See
  [`mine.dbrow`](../server/content/scripts/skill_mining/configs/mine.dbrow).
- Five rune-rock placements appear in this checkout: two around `(3059,3885)`,
  one around `(3046,10265)`, and two around `(2937,9882)`. Treat coordinates as
  scouting candidates, not proof that production is identical.
- Black dragons drop one `dragonhide_black` each, and only four ordinary black
  dragon spawns appear in the mapped caves reviewed here. The generated generic
  `wiki/items/dragonhide.md` page is misleading; the source has color-specific
  hides and black-hide crafting.
- Shops use stock-sensitive prices and can pay zero after swarm overstock. The
  SDK's published shop snapshot can show normal-stock rather than live dynamic
  price, so verify the actual interface before bulk selling.
- The SDK already provides two-screen, revalidated player trades and a dedicated
  `serveTrades` worker-to-mule loop. Use that for scarce goods; the existing
  pickpocket swarm's public ground-drop handoff is unsuitable for runite or
  hides.

## Working strategy

### 1. Value access, not cash

Maintain an internal shadow price for every target good:

`risk-adjusted worker-minutes + travel/setup + expected contention delay + death exposure`

Use GP only as operating liquidity for food, tanning, tools, tolls, and runes.
Track barter inventory and observed trade ratios separately from static item
costs, alchemy values, shop values, and hiscore wealth.

Near-term scarce baskets to test are runite ore/bars, black hides/leather,
replacement PK supplies, clue uniques, and any newly saturated fixed-spawn
resource. Do not assume a reported scarce item remains scarce without checking
occupancy and actual willingness to trade.

### 2. Build a control plane before a large fleet

There is no reusable general swarm coordinator in the repo yet. Use one
controller per bot and place coordination above those controllers:

1. **Planner/allocator** — low-frequency strategy, role assignment, shadow
   prices, and route selection.
2. **Deterministic workers** — validated gather, bank, combat, and resupply loops
   with no model call in the hot path.
3. **Shared state** — account role, position, task lease, inventory exposure,
   target, health/food, last progress, and recovery reason.
4. **Barriers and leases** — synchronized pile/retreat commands and exclusive
   ownership of NPCs, rocks, or route segments to reduce self-contention.
5. **Recovery queue** — stuck routes, deaths, missing prerequisites, and unknown
   dialogs go to a separate adapter/model path instead of wedging every worker.

Start with 2–5 accounts and a four-account canary, not a headline-sized swarm.
Stagger starts and apply per-task backoff. Scale only when marginal productive
ticks remain positive after contention.

### 3. PK/KOTH formation

- **Anchor:** one uniquely highest-combat account stays inside the scoring
  polygon. Avoid multiple equal-max allies before incumbency is secured because
  the initial winner can be random.
- **Caller/scout:** watches approaches, nearby player combat levels, crown
  announcements, and the bank/return corridor. The current SDK exposes little
  opponent state, so the caller must work with coarse signals.
- **Guards/pile accounts:** accounts within the relevant Wilderness combat-level
  band focus one challenger in the multiway zone. Designate skull initiators;
  do not skull the anchor without a reason.
- **Binder:** adds movement control once Magic 20/50/79 is validated. Keep the
  caster close enough for freezes to remain effective.
- **Mule/resupplier:** holds food, prayer supplies, runes, poison, and cheap
  replacement kits away from the hill; uses verified trade rather than public
  drops.
- **Recovery/loot runner:** handles Lumbridge 1-HP respawns, replacement gear,
  rejoin routing, and the short killer-private loot window.

Treat resource-node defense and KOTH as different missions. For runite, a
geofenced sentinel plus staggered miners/haulers may be enough. For KOTH, low
levels do not add score; the formation exists to keep the anchor highest and
alive.

### 4. Risk doctrine

- Non-aggressing workers use the demonstrated three-item exposure cap and bank
  immediately on rare or noted drops.
- Aggressors assume they will skull and carry only a deliberately chosen
  Protect Item candidate plus consumables they are willing to lose.
- Compare static death-protection cost with internal barter value before every
  loadout. Never assume the game protects the item the economy values most.
- Model bank approaches and staging paths as contested corridors. Stagger,
  scout, or escort returns; do not send the whole fleet through one predictable
  route at once.
- Deep-Wilderness escape is a walk/fight/resupply problem, not a standard
  teleport policy.

### 5. Progression sequence for this checkout

`agentmachine` is currently a near-fresh Lumbridge account, so immediate KOTH
combat is not realistic.

1. Verify live state and server multipliers; then validate early combat and
   food loops in 10–30 second probes.
2. Bootstrap combat with quest XP where this revision permits it, but implement
   prerequisite fallbacks such as the reported Waterfall food/rat detour.
3. Add Protect Item, protection prayers, food supply, and cheap replacement-kit
   banking before entering the Wilderness.
4. Run a disposable scout to verify the live KOTH polygon, scoring rule,
   multiway behavior, current holders/loadouts, and escape route.
5. Test a small economy cell and secure mule handoffs before adding the PK cell.
6. Only then run the anchor + caller + guards/binder + mule/recovery formation.

## Metrics that decide whether to scale

- Productive ticks / connected account-hour.
- Scarce-good units or risk-adjusted worker-minutes gained per account-hour.
- Self-contention: idle time and failed interactions caused by another fleet
  member.
- Deaths, replacement-kit cost, and rare-value exposure per 100 account-hours.
- Time from death/stuck state to productive rejoin.
- Planning/model calls per worker-hour and percentage handled deterministically.
- KOTH capture minutes, anchor uptime, challenger response time, and pile
  synchronization failures.
- Barter fills and observed goods-for-goods ratios; GP is a secondary liquidity
  metric.

## Open questions for the next live pass

1. Does production use the checked-in highest-combat KOTH rule, one-minute
   scoring, tie behavior, and multiway map?
2. Who currently holds KOTH, with what combat level, loadout, fleet size,
   alliance, active hours, and staging route?
3. What are the current barter ratios and fill rates for runite, black hides,
   sharks, runes, potions, and replacement PK kits?
4. Which rune rocks and black-dragon caves are currently occupied, and at what
   times?
5. Do live skull, Protect Item, freeze, loot-timer, 1-HP death-mark, and deep-
   Wilderness teleport behaviors match the source?
6. Are KBD, KQ, quest cape, and clue-completion frontiers still in the reported
   state?
7. Does production currently use the 25x XP configuration visible in the local
   deployment file? Do not forecast replacement-account time until verified.

