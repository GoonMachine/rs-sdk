# Cloud Agent B — death mark, then mule + 1 HP rejoin

**Audience: Cloud Agent B only.** Paste below the line into that agent. Do not
read `operator.md`. Agent A has a different file.

Paste **everything below the line** into a new Cursor Cloud Agent on
`GoonMachine/rs-sdk` `main`. Do not also paste Agent A. Do not launch a second
copy of this prompt.

If this is a **first launch or a resume**, prefer the short block in
“Launch / resume (use this)” over re-pasting the whole essay.

---

You are **Cloud Agent B** on the compete-and-counter KOTH plan. Phase 1
(death mark, boards) is done. You are **not** the cow trainer. Agent A owns
the 25× quest stack. You mule and time the 1 HP rejoin. Do not share bot names.

## Efficiency (read this before researching)

You are **not** leveling a fleet. Stay on `foodprobe1` / `foodkill1` until
[`scarce-goods.md`](scarce-goods.md) names a first gather **and** the
operator POST says to create **one** extra lite on this same VM. Do not
invent a third Cloud environment. Do not walk the hill.

- After **each** probe: write the number into `learnings/ab-results-b.md`,
  tick your box in `learnings/server-diffs.md`, commit on `cloud/ab-b`.
  Conflict → [`merge.md`](merge.md). Do not sit still resolving.
- Prod tick is **300.3 ms/tick** (Agent A, on `main` in `ab-results-a.md`).
  Death-mark 1000 ticks ≈ **300 seconds**. Skull 2000 ticks ≈ **600 seconds**.
  Do not re-measure tickrate.
- Agent A’s `/playerpositions` pass saw **no one in the ruins polygon**.
  The 2026-08-15 eight-stack snapshot may be stale. Fetch live boards
  yourself; do not hunt those eight names. Live wins.
- Do not survey quest scripts. Do not walk the Demonic Ruins. Do not run
  Cook's Assistant or Waterfall.
- Phase 1 is **done** on `origin/cloud/ab-b` (`74cdf02f9`). Do not re-PK.
- Do **not** start `foodboot1` or cows. The quest path won the A/B.
- Next lane: mule Waterfall kit on `foodprobe1` (Betty `(3012,3259)` + rope +
  food), trade `qstboot1`, then time the 1 HP rejoin to `(3303,3878)` eastern
  corridor. Stop before greater demons. Never `(3284,3799)` spiders.
- Do not re-read the full briefing if you already did this session. Open
  the result file and execute the next empty cell.
- Chat reports do not count. Only written cells survive a stop.

## Launch / resume (use this)

```
You are Cloud Agent B on GoonMachine/rs-sdk. git pull origin main.
Phase 1 is on origin/cloud/ab-b. Do not re-PK. Do not cows / foodboot1 / hill.
Read learnings/scarce-goods.md. Banks are per-account; no Lumbridge bank.

We own every account. Trade useful items. Sit is goonmule1 only.

Treasury: all coins on goonmule1 (Falador bank). Target 6k.

kitprep1 (thieve 50) and foodprobe1 (thieve 42): pickpocket **guards**
(30 gp, thieve 40) in Falador/Varrock. Eat on stun. Dump coins to the
mule. First 50 gp: Thessalia gloves (6) + coins onto qstboot1 at the
boy (2927,3455). Then refill mule and shop Horvik/Wayne/Louie/Zeke
when price <= coins. Knights at thieve 55. Write coin counts to
ab-results-b.md.

foodprobe1 also Wydin cheese onto the mule. Do not follow A into the house.

goonmine1: skipTutorial if gated. Bob pick 1gp. Food in inv (Wydin
cheese or a trade from goonmule1). SE Varrock (3285,3365) copper/tin
then iron. Every mine loop: if player.hp < maxHp then bot.eatFood.
No cow combat grind. Al Kharid scorpions later (cb 27+). No wild
runite before Mining 85.

goonmule1 hauls then sits Falador with a food reserve.
foodkill1 idle until a real 1 HP clock. Never commit bots/ or print bot.env.
```

## Environment

This VM talks to the **demo server** `rs-sdk-demo.fly.dev`, not RuneBench.

1. `export PATH="$HOME/.bun/bin:$PATH"` at the top of every new shell.
2. `git pull origin main`. Read, in order:
   `learnings/strategy-compete-koth.md`,
   `learnings/server-diffs.md`,
   `learnings/observe-fidelity.md`,
   then this file if you need to re-check the contract.
3. Do **not** launch Chromium. Lite client only:
   `cd server/webclient && bun src/lite/runner.ts <botname>`
4. Create bots with `bun bots/create-bot.ts <name>` (max 12 alphanumeric).
   You are a **Goonmachine**. Keep `foodprobe1` / `foodkill1` / `kitprep1`.
   New names: [`names.md`](names.md) — `goon` + role + index (`goonkit1`,
   `goonmule1`, …). Do not use `foodboot1`, `goo*`, `agentmachine`, or
   A’s `qstboot1`.
5. `bots/*/` is gitignored. Never commit `bot.env` or print a password.
6. Start lite first, then `bun bots/<name>/script.ts`.
7. One controller per bot. Fail fast: 10–30s scripts until a loop is proven.

## Non-goals (stop if you catch yourself doing these)

- No hill raid, no Demonic Ruins walk, no 8-account fleet, no goo rune kit.
- Do not PK the live 8-stack or goo. Your PvP death is **two of your own
  fresh accounts** in **low wilderness**, junk only.
- Do not clone the 8-stack or goo. Do not start 100 Cloud Agents.
- Do not put a model on every game tick.
- Do not treat Discord / the next Codex dump as production truth until you
  source-check and live-probe.
- Do not complete Waterfall. That is Agent A’s A/B path.

## Phase 1 — your checkboxes (do these first)

Tick the matching boxes in `learnings/server-diffs.md` only after a live
measurement. Write numbers in `learnings/ab-results-b.md`.

### 1. Live boards

Fetch and summarize:

- `https://rs-sdk-demo.fly.dev/playerpositions`
- `https://rs-sdk-demo.fly.dev/hiscores/koth`

Compare to `learnings/koth-swarm-snapshot-2026-08-15.md`. If snapshot and live
disagree, **live wins**. Agent A already saw an empty polygon once — treat
that as a lead, not a skip. Note who (if anyone) is in the ruins and whether
any `goo*` is on the hill. Do this again at the end of the session.

### 2. PvP death → Lumbridge at 1 HP

Checkout: `server/content/scripts/player/scripts/death.rs2` (PvP death sets
the mark and subtracts HP to 1). Wild attack rule:
`abs(cb diff) <= min(the two players' wild levels)` in
`pvp_combat.rs2`. Two combat-3 accounts can fight each other at wild ≥ 1.

1. Create `foodprobe1` and `foodkill1`. Skip tutorial on both. Empty / junk
   inventories. No rune, no food you care about.
2. Walk both to a **low wild** tile (not the hill; Edgeville / Varrock north
   ditch is enough). Confirm wild level ≥ 1.
3. `foodkill1` attacks `foodprobe1` until `foodprobe1` dies.
4. Record: death spawn `(x, z)`, hitpoints, skull, inventory kept/lost,
   wall-clock of death.

### 3. Death mark blocks NPC-suicide full-heal

Checkout: `pvp_death_mark.rs2` and `^pvp_death_mark_duration = 1000` in
`pvp.constant`. At the measured **300.3 ms/tick** that is **~300s**, not
the local 400ms (~6.7 min). Forecast 300s; still measure live.

After the PvP death, still at 1 HP, have `foodprobe1` die to a **nearby NPC**
(or another cheap death). Record HP on respawn. If it is still 1, the mark
held. Sample once around +30s and once around +300s. Do not AFK the full
mark waiting for a single data point. Write the measured duration.

Do **not** use this death to “reset” for the hill. There is no hill in your
lane.

## After Phase 1 — mule + rejoin (not cows)

The quest path won. Do not start `foodboot1`.

1. Relite `foodprobe1` only. Buy Waterfall kit: 6 air/water/earth at Betty
   `(3012,3259)`, rope, cheap food. Trade `qstboot1` if both are in range.
2. Time junk-only 1 HP walk Lumbridge → `(3335,3528) → (3334,3650) →
   (3334,3769) → (3335,3870)` and **stop at `(3303,3878)`**. No demons, no
   spider square `(3284,3799)`, no hill stand.
3. Write minutes + attributed deaths in `ab-results-b.md`
   ([`observe-fidelity.md`](observe-fidelity.md)). Commit `cloud/ab-b`.

**Win metric:** minutes for a 1 HP body to reach `(3303,3878)` alive. Unnamed
death = invalid clock.

## Write-up

Update `learnings/ab-results-b.md` with the template sections already in that
file. Tick **only your** boxes in `learnings/server-diffs.md`.

Commit on a branch named `cloud/ab-b` (not `main`). Never commit `bots/*/`.
Do not print passwords. Hold the account lightly; demo persistence is not
guaranteed.
