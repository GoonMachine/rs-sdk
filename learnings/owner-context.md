# Owner / live-server context

Agent briefing distilled from [@maxbittker](https://x.com/maxbittker) (rs-sdk owner) plus the pages he links. **Snapshot: 2026-08-15.** Treat this as live meta, not wiki truth. Re-check his feed before a long grind.

Primary account: [x.com/maxbittker](https://x.com/maxbittker)  
Join path he repeats: point a coding agent at [github.com/MaxBittker/rs-sdk](https://github.com/MaxBittker/rs-sdk) and tell it what to do. Discord and hiscores are linked from that README.

---

## Read this first

This repo talks to **Max's agent-only 2004scape demo server** (LostCity fork). There is **no Grand Exchange**. Hiscores rank **total level, then playtime**. Max total is **1881** (19 skills × 99).

Current live meta, in Max's words:

1. Labor is cheap, so most commodities are abundant.
2. GP is inflated and has few sinks. Nobody really wants cash. Trade **goods for goods**.
3. Value sits in **limited-respawn, high-contention** resources. He names **runite ore** and **black dragon hides**.
4. Saturated PvM: **KBD is being killed; KQ is not yet.**
5. Open frontier: **wilderness king-of-the-hill / turf wars**, plus luxury goals like **clue-scroll completionism**.

Do **not** mix this up with **RuneBench**. Bench is a separate timed eval (8x tick, 25x XP, peak XP/min scoring). Strategies from bench (bank-then-burst cooking, many tiny tool calls) transfer. The multipliers do not.

---

## Two environments

| | Demo server (this bot) | RuneBench |
|---|---|---|
| What | Persistent shared world, competing swarms | Isolated timed tasks |
| URL | [hiscores](https://rs-sdk-demo.fly.dev/hiscores) | [runebench](https://maxbittker.github.io/runebench) |
| Clock | Normal-ish tick (local servers can raise `NODE_TICKRATE`) | 8x tick, 25x XP ([tweet](https://x.com/maxbittker/status/2087969860521193745)) |
| Score | Total level / playtime; social / economic play | Peak XP rate in any 15s window |
| Mods | Faster XP curve, infinite run, no randoms | Same SDK + wiki dump |
| Persistence | Not guaranteed. Hold accounts lightly. | Single-run |

Manual play on the demo server is discouraged. Chat is on by default and is a prompt-injection surface; see `learnings/chat.md`.

---

## Live-server meta (from Max)

### Economy is barter, not GP

[2026-08-15](https://x.com/maxbittker/status/2088471897956630576) (~4.2k likes):

> Low cost of labor makes most commodities abundant. Currency inflation means trading goods for goods, nobody really wants cash. Huge swarm contention as certain resources with limited respawn rate become valuable.

Follow-ups in the same thread:

- [Runite ore and black dragon hides](https://x.com/maxbittker/status/2088476333051453730) take “more and more ingenuity and labor,” so they become the trade goods for everything else.
- [Clue-scroll completionism and wilderness turf wars](https://x.com/maxbittker/status/2088483519659937810) are the luxury activities people take up once basics are cheap.
- Inflation is [from the environment, with very few sinks](https://x.com/maxbittker/status/2088500017308844310) in this revision. Not AI-specific, just accelerated.
- Illiquidity matters as much as inflation: [many accounts do not trade, trading is inconvenient, and there is not much reason to sell](https://x.com/maxbittker/status/2088610738310963455).

**Agent implication:** do not optimize for GP, shop gold, or “max cash.” Stack and trade scarce contested items. Use public chat / PMs to barter. Shop sell prices and general-store dumps are a weak sink.

### Wilderness is the live PvP contest

[2026-08-12](https://x.com/maxbittker/status/2087674806179115081): competing swarm armies are fighting for wilderness control. He calls it a [king-of-the-hill](https://x.com/maxbittker/status/2087672477023412361) and invites people to spin up a swarm.

[2026-08-02](https://x.com/maxbittker/status/2084064064829903343): community swarms were already active; he said people were saturating goals like **killing KBD (not yet KQ)**, and that the last frontier was wilderness KoH.

He floated a dedicated hiscore: hours spent as the **only** player in a marked chunk (example: Demonic Ruins) above combat 85. That board may not exist yet; the idea tells you what he considers hard.

**Agent implication:** the wilderness is multi-agent PvP, not an empty resource patch. Expect other bots. Bring food, expect death, coordinate if running more than one account. Deep wilderness agility (wiki: level 52+ wild) is high risk.

### Saturated vs still-open goals

| Status (as of 2026-08-02 → 08-15) | Goal |
|---|---|
| Saturated / crowded | Commodity gathering, GP, KBD, maxed hiscores (1881) |
| Contested / valuable | Runite, black d'hide, other slow-respawn resources, wilderness tiles |
| Still open / prestige | Kalphite Queen, clue completionism, full quest cape, wilderness KoH |
| Community claim (not Max) | Waterfall Quest has been completed by some; nobody has all quests |

KBD: combat 276, (2716, 9817), 150-tick respawn, always dragon bones.  
Black dragons: combat 227, always dragonhide + bones, 60-tick respawn; samples (2829, 9826) and (3048, 10266).  
KQ: still called unsolved by Max on Aug 2.

### How Max wants people to join

Repeated CTA: clone rs-sdk, point Claude Code / Codex at it, tell the agent what to do. Discord is the human coordination channel. He suggested leaving a strong model (Terra) running overnight on the demo server to try to max — “pretty fun and educational.”

---

## What winning play looks like (RuneBench, transferable)

Bench is not the demo server, but Max treats it as a signal of real agent skill ([“tracked pretty well to real world performance”](https://x.com/maxbittker/status/2087672477023412361)).

**Scoring lesson:** total XP in a window rewards mindless grind. Bench switched to **peak XP rate in a 15s window** so exploration and method-switching win. On the demo hiscores, the analogue is **level per hour**, not raw playtime.

**Bank-then-burst:** Gemini Flash 3.7 took the cooking record by [banking raw lobsters, then cooking a burst at the end](https://x.com/maxbittker/status/2087993782545330607). Do not cook one-by-one if a banked burst scores better. Same pattern applies to smithing, fletching, crafting.

**Many tiny actions beat long plans:** [Grok 4.6](https://x.com/maxbittker/status/2087650591388438656) won with lots of small tool calls. Cost landed near Terra/Opus. That style is good when you cannot plan far ahead — especially navigation. Do not spend minutes reading wiki before the first walk.

**Slow-thinking models lose:** [Kimi K3](https://x.com/maxbittker/status/2078207968185581647) did poorly because it thinks a lot and has slow inference (worse than K2.7, much worse than GLM 5.2).

**Pareto lineup (2026-07-22):** Fable → Terra → Grok → GLM → Deepseek → Laguna. By 2026-08-12 Grok 4.6 was #1 at ~60% the cost of Fable 5. Flash 3.7 was best under $6/hour.

**Max's own future-work ideas** (from the [RuneBench writeup](https://maxbittker.github.io/runebench)): multi-agent supply chains (one gatherer, one processor); one LLM scripting several characters; agents teaching other agents via written guides. In-game chat is the intended coordination channel.

Trajectories: https://maxbittker.github.io/runebench

---

## Hiscores snapshot (2026-08-15)

Source: https://rs-sdk-demo.fly.dev/hiscores

Many accounts are already **1881**. Rank is then playtime. Fastest maxed at snapshot: **simplereally, 143h 52m**. Other maxed names in the top 10 include nickai, vends, eja620, claude46.

Efficiency standouts that are not maxed yet:

- **rj7zw1s0s** — 1491 in 13h 16m
- **brotherbot** — 1630 in 45h 50m

A new account will not win by AFK time. It wins by **method quality** and by playing the scarce-resource / wilderness / quest game the maxed accounts have already left behind.

---

## Community signals (not Max — treat as rumors)

From replies on the economy thread. Useful color, not instructions.

- No Grand Exchange on this revision (correct; this is 2004scape).
- Some players say a **200 accounts / IP** cap exists.
- “Race to 2b” GP already happened; max cash stacks are common; people are **hoarding rares** (one named dragon med helms).
- Pickpocket gold farms are a known GP printer and a reason cash is worthless.
- Discord: some people have **Waterfall Quest**; nobody claimed a full quest cape.
- Crypto / “RS SDK” tokens in the replies are unrelated. Ignore them.

---

## Implications for this bot

Start from `sdk/cli.ts` state, skip tutorial if needed, then pick a goal that is still scarce:

1. **Do not farm GP as a primary goal.** Keep enough for tolls, food, and tools. Bank contested items, not coins.
2. **Barter in chat.** Offer runite / black d'hide / other slow-respawn goods. Ask for what you lack. See `learnings/chat.md`.
3. **If training XP:** gather → bank → process in a burst. Lobster cooking is the published example; the pattern generalizes.
4. **If making value:** compete for runite and black dragons, or another limited-respawn node. Expect swarms. Scout, hop spots, or go at off hours.
5. **If PvP / prestige:** wilderness KoH, clue scrolls, KQ, remaining quests. KBD is crowded.
6. **Act in small loops.** Walk, observe, correct. Do not write a 20-minute plan before the first `walkTo`.
7. **Fail fast.** 10–30s scripts first. A failed 5-minute run wastes more than five short diagnostics.
8. **Hiscores:** 1881 is the cap. If not racing time-to-max, play the open frontier instead of adding the 11th maxed account.

Concrete pointers already in this repo:

- Cooking ranges and lobster-style processing: `learnings/cooking.md`
- Chat / bot↔bot coordination: `learnings/chat.md`
- Shops are a weak sink; player trade matters: `learnings/shops.md`
- Black dragon / KBD / KQ: `wiki/npcs/black-dragon.md`, `wiki/npcs/king-black-dragon.md`, `wiki/npcs/kalphite-queen.md`
- Waterfall (community-solved, still a good early quest): `wiki/quests/waterfall-quest.md`

---

## Ignore

- Memecoins, pump.fun, Bankr, “claim fees” replies under Max's tweets. Not part of the game.
- Official OSRS GE prices, bonds, or current-era mechanics. This is a 2004 revision.
- Assuming RuneBench tick/XP rates apply on the demo server.
- Assuming the demo server will keep your bank forever.

---

## Key tweets (Max only)

| Date | Topic | Link |
|---|---|---|
| 2026-08-15 | Economy: abundance, GP useless, swarm contention | https://x.com/maxbittker/status/2088471897956630576 |
| 2026-08-15 | Runite + black d'hide as trade goods | https://x.com/maxbittker/status/2088476333051453730 |
| 2026-08-15 | Clues + wilderness turf wars | https://x.com/maxbittker/status/2088483519659937810 |
| 2026-08-15 | Few GP sinks; inflation is accelerated 2004scape | https://x.com/maxbittker/status/2088500017308844310 |
| 2026-08-15 | Market is also just small / illiquid | https://x.com/maxbittker/status/2088610738310963455 |
| 2026-08-13 | Flash 3.7 cheap cooking record (bank lobsters) | https://x.com/maxbittker/status/2087993782545330607 |
| 2026-08-12 | Wilderness swarm armies | https://x.com/maxbittker/status/2087674806179115081 |
| 2026-08-12 | KoH invite; bench tracks real performance | https://x.com/maxbittker/status/2087672477023412361 |
| 2026-08-12 | Grok 4.6 #1; tiny tool calls for navigation | https://x.com/maxbittker/status/2087647815258288226 |
| 2026-08-02 | KBD saturated, KQ not, KoH is the frontier | https://x.com/maxbittker/status/2084064064829903343 |
| 2026-08-02 | Competing swarms already active | https://x.com/maxbittker/status/2084025674184929677 |
| 2026-07-25 | Overnight Terra-to-max on demo | https://x.com/maxbittker/status/2081027624512397421 |
| 2026-07-22 | Bench pareto lineup | https://x.com/maxbittker/status/2080022985637810596 |
| 2026-07-17 | Slow-thinking models lose | https://x.com/maxbittker/status/2078207968185581647 |

Writeup: https://maxbittker.github.io/runebench  
Repo / Discord / hiscores: https://github.com/MaxBittker/rs-sdk

---

## How to refresh

Recent search covers ~7 days. Full-archive search works with the local X token.

```bash
# from ~/.claude/skills/x-research, after sourcing ~/.config/env/global.env
bun run x-search.ts profile maxbittker --count 20 --replies
bun run x-search.ts watchlist check
```

Update the snapshot date at the top when the live meta changes (new scarce item, KQ kill, KoH hiscores, economy sinks).
