# Cloud Agent B — death mark / positions, then rats and food

Paste **everything below the line** into a new Cursor Cloud Agent on
`GoonMachine/rs-sdk` `main`. Do not also paste Agent A. Do not launch a second
copy of this prompt.

If this is a **first launch or a resume**, prefer the short block in
“Launch / resume (use this)” over re-pasting the whole essay.

---

You are **Cloud Agent B** on the compete-and-counter KOTH plan. Your lane is
Phase 1 probes (PvP death mark, live positions) then Phase 2 path **B**
(rats / goblins / cows + bones). Agent A owns tickrate, quest XP, the
polygon scout, and the Waterfall path. Do not do Agent A’s work and do not
share bot names.

## Efficiency (read this before researching)

You are **not** leveling a fleet. Phase 1 is boards + one junk PvP death.
Phase 2 is **one** trainer (`foodboot1`). Do not create extra accounts
beyond `foodprobe1` / `foodkill1` / `foodboot1`.

- After **each** probe: write the number into `learnings/ab-results-b.md`,
  tick your box in `learnings/server-diffs.md`, commit on `cloud/ab-b`.
- Prod tick is **300.3 ms/tick** (Agent A, on `main` in `ab-results-a.md`).
  Death-mark 1000 ticks ≈ **300 seconds**. Skull 2000 ticks ≈ **600 seconds**.
  Do not re-measure tickrate.
- Agent A’s `/playerpositions` pass saw **no one in the ruins polygon**.
  The 2026-08-15 eight-stack snapshot may be stale. Fetch live boards
  yourself; do not hunt those eight names. Live wins.
- Do not survey quest scripts. Do not walk the Demonic Ruins. Do not run
  Cook's Assistant or Waterfall.
- First paid session: **Phase 1 only**. Write the death-mark numbers, commit,
  stop. Do not start the cow grind in the same run.
- Do not re-read the full briefing if you already did this session. Open
  the result file and execute the next empty cell.
- Chat reports do not count. Only written cells survive a stop.

## Launch / resume (use this)

```
You are Cloud Agent B on GoonMachine/rs-sdk. git pull origin main.
Tick is 300.3 ms — do not re-measure. Death mark should be ~300s. Do not
create qstprobe1/qstboot1/agentmachine. Do not walk the Demonic Ruins.

1. Fetch /playerpositions and /hiscores/koth. The hill may be empty; live
   wins over the 2026-08-15 snapshot. Write ab-results-b.md, commit cloud/ab-b.
2. Create foodprobe1 + foodkill1. Skip tutorial. Junk only. Low wild (not
   the hill). foodkill1 PKs foodprobe1. Record spawn, HP (expect 1), keep/lost.
   Write + commit.
3. While marked, NPC-death foodprobe1. Expect 1 HP again. Sample once soon
   (~30s) and once near 300s — do not AFK the whole mark. Write duration.
   Tick your server-diffs boxes. Commit. Stop.

Do not start foodboot1 or the cow loop. Do not survey quests.
Never commit bots/ or print bot.env.
```

## Environment

This VM talks to the **demo server** `rs-sdk-demo.fly.dev`, not RuneBench.

1. `export PATH="$HOME/.bun/bin:$PATH"` at the top of every new shell.
2. `git pull origin main`. Read, in order:
   `learnings/strategy-compete-koth.md`,
   `learnings/server-diffs.md`,
   `learnings/owner-context.md`,
   then this file if you need to re-check the contract.
3. Do **not** launch Chromium. Lite client only:
   `cd server/webclient && bun src/lite/runner.ts <botname>`
4. Create bots with `bun bots/create-bot.ts <name>` (max 12 alphanumeric).
   Reserved names: **`foodprobe1`** (victim), **`foodkill1`** (killer for the
   death test), **`foodboot1`** (Phase 2 trainer; may reuse `foodprobe1` after
   the mark expires if that is simpler).
   Do not use `agentmachine`, `qstprobe1`, or `qstboot1`.
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

## Phase 2 — A/B path B (only after Phase 1 boxes you own are ticked)

Bootstrap combat with **cheap food loops**, not quests.

- `foodboot1` (or `foodprobe1` after the mark expires and HP is normal).
- Skip tutorial. Train at Lumbridge rats / goblins / cows. Open gates.
  Patterns and coords: `learnings/combat.md`.
- Bury bones for prayer. Eat cheap food. Infinite run helps travel, not DPS.
- Prove the attack loop in 10–30s (XP gained, not just `sendInteractNpc`
  success). Then extend. A failed 5-minute run wastes more than five 30s
  diagnostics.
- Unlock Protect Item (Prayer 25, `prayers.dbrow`) before any planned wild
  trip. You still should not walk the hill on this job.

**Win metric (report both, with wall-clock from first trainer login):**

- Minutes to Prayer 25 (Protect Item)
- Minutes to combat ≥ 77 (needed so `abs(cb − 123) ≤ 46` at wild 46)

Also report XP/hour and deaths-to-NPC. Stop and write results when you have
a clean rate, or if the loop is wedged. Do not silently start Waterfall.

## Write-up

Update `learnings/ab-results-b.md` with the template sections already in that
file. Tick **only your** boxes in `learnings/server-diffs.md`.

Commit on a branch named `cloud/ab-b` (not `main`). Never commit `bots/*/`.
Do not print passwords. Hold the account lightly; demo persistence is not
guaranteed.
