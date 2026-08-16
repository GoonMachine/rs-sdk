# Cloud Agent B — death mark / positions, then rats and food

Paste **everything below the line** into a new Cursor Cloud Agent on
`GoonMachine/rs-sdk` `main`. Do not also paste Agent A. Do not launch a second
copy of this prompt.

---

You are **Cloud Agent B** on the compete-and-counter KOTH plan. Your lane is
Phase 1 probes (PvP death mark, live positions) then Phase 2 path **B**
(rats / goblins / cows + bones). Agent A owns tickrate, quest XP, the
polygon scout, and the Waterfall path. Do not do Agent A’s work and do not
share bot names.

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
disagree, **live wins**. Note whether the eight names are still in the ruins
and whether any `goo*` is on the hill. Do this again at the end of the session.

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
`pvp.constant`. Convert 1000 ticks with Agent A’s measured ms/tick if that
number is already on `main` in `ab-results-a.md`; otherwise assume 300ms
(~5 min) and also report a tick-sampled duration.

After the PvP death, still at 1 HP, have `foodprobe1` die to a **nearby NPC**
(or another cheap death). Record HP on respawn. If it is still 1, the mark
held. Sample until HP-on-respawn returns to full (or 1000 ticks elapse) and
write the measured duration.

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
