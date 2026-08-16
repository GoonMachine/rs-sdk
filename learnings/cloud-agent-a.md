# Cloud Agent A — tick / quest XP / polygon, then Waterfall

**Audience: Cloud Agent A only.** Paste below the line into that agent. Do not
read `operator.md`. Agent B has a different file.

Paste **everything below the line** into a new Cursor Cloud Agent on
`GoonMachine/rs-sdk` `main`. Do not also paste Agent B. Do not launch a second
copy of this prompt.

---

You are **Cloud Agent A** on the compete-and-counter KOTH plan. Your lane is
Phase 1 probes (tick, quest XP, polygon) then Phase 2 path **A** (quest XP
bootstrap, Waterfall first). Agent B owns death-mark, hiscores, and the
rats/food path. Do not do Agent B’s work and do not share bot names.

## Efficiency (read this before researching)

You are **not** leveling a fleet. Phase 1 is three short probes. Phase 2 is
**one** trainer (`qstboot1`). Do not create extra accounts.

- After **each** probe: write the number into `learnings/ab-results-a.md`,
  tick your box in `learnings/server-diffs.md`, commit on `cloud/ab-a`. Do
  not batch writes at the end of the session.
- Prod tick is **already measured: 300.3 ms/tick**. Do not re-measure it.
  `git pull` and skip to Cook's Assistant.
- Cook's Assistant is the quest-XP test. Run it. Do **not** survey every
  `stat_advance` in `server/content/scripts/quests/`.
- Do not re-read the full strategy/owner briefing if you already did this
  session. Open the two result files and execute the next empty cell.
- Kill leftover lite sessions that are not `qstprobe1` / `qstboot1`
  (`envtestbot` is not your job; it burns a login slot).
- If you stop mid-probe, the last written number is the only thing that
  survived. Chat reports do not count.

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
   Reserved names: **`qstprobe1`** (Phase 1), **`qstboot1`** (Phase 2).
   Do not use `agentmachine`, `foodprobe1`, `foodkill1`, or `foodboot1`.
5. `bots/*/` is gitignored. Never commit `bot.env` or print a password.
6. Start lite first, then `bun bots/<name>/script.ts`.
7. One controller per bot. Fail fast: 10–30s scripts until a loop is proven.

## Non-goals (stop if you catch yourself doing these)

- No hill raid, no PK, no 8-account fleet, no goo rune kit.
- Do not attack anyone in the Demonic Ruins. Polygon scout is stand-in /
  stand-out only, junk gear, disposable body.
- Do not clone the 8-stack or goo. Do not start 100 Cloud Agents.
- Do not put a model on every game tick.
- Do not treat Discord / the next Codex dump as production truth until you
  source-check and live-probe.

## Phase 1 — your checkboxes (do these first)

Tick the matching boxes in `learnings/server-diffs.md` only after a live
measurement. Write numbers in `learnings/ab-results-a.md`.

### 1. Prod tick length

On `qstprobe1`, after skip-tutorial if needed: sample `sdk.getState().tick`
against wall clock for ~20–30s. Report ms/tick. Checkout comment says prod
300ms; local default is 400ms. Use the **measured** value for every later
duration.

### 2. Quest XP × `xpRate`

Do **not** wait for Waterfall to tick this box.

- Source: `server/content/scripts/quests/quest_cook/scripts/quest_cook.rs2`
  `stat_advance(cooking, 3000)` = 300 XP tenths-decoded, then `addXp` multiplies
  by `Environment.node.xpRate` when `allowMulti` is true (default).
- Checkout `xpRate` is 25 in `server/engine/src/util/WorldConfig.ts`.
- Record cooking XP **before** and **after** Cook's Assistant.
- Expected if 25× applies: +7,500 cooking XP. Expected if quests are 1×: +300.
- If Cook's Assistant wedges, pick another **short** quest whose
  `stat_advance` you have read in this checkout. Do not invent wiki XP.

### 3. KOTH polygon

Read vertices in `server/engine/src/engine/Koth.ts`. Walk `qstprobe1` (junk
only, combat 3 is fine) to the Demonic Ruins. Stand just **inside** one edge
and just **outside** the same edge. Record `(x, z)` and whether `/hiscores/koth`
or local crown chat treats you as in-zone. Leave. Do not attack. If you die,
that is data — note it — then stop the hill walk.

Re-read `https://rs-sdk-demo.fly.dev/playerpositions` before you walk up so
you know whether the 8-stack is still there. You are a scout, not a scorer.

## Phase 2 — A/B path A (only after Phase 1 boxes you own are ticked)

Bootstrap combat with **this revision’s quest XP**, Waterfall first.

- Source-check `server/content/scripts/quests/quest_waterfall/scripts/quest_waterfall.rs2`
  before you walk. Reward line is `stat_advance(attack, 137500)` and
  `stat_advance(strength, 137500)` (13,750 XP each × measured quest rate).
- Wiki walkthrough is `wiki/quests/waterfall-quest.md` — last, after the
  script. The wiki is already stale on other skills.
- Use `qstboot1` (fresh). Skip tutorial. 10–30s steps. Record every wedge
  (dialog, “can’t reach”, missing rope/runes, fire giants).
- Later quests only after you open **that** quest’s `.rs2` and read the
  `stat_advance` lines.
- Unlock Protect Item (Prayer 25, `prayers.dbrow`) before any planned wild
  trip. Bones from a short safe loop are allowed if a quest does not give
  prayer.

**Win metric (report both, with wall-clock from first `qstboot1` login):**

- Minutes to Prayer 25 (Protect Item)
- Minutes to combat ≥ 77 (needed so `abs(cb − 123) ≤ 46` at wild 46)

Stop and write results if Waterfall is enough, if you are clearly stuck, or
if you have a clean hours-to-77 / hours-to-PI estimate. Do not silently start
a 5-minute grind on a wedged dialog.

## Write-up

Update `learnings/ab-results-a.md` with the template sections already in that
file. Tick **only your** boxes in `learnings/server-diffs.md`.

Commit on a branch named `cloud/ab-a` (not `main`). Never commit `bots/*/`.
Do not print passwords. Hold the account lightly; demo persistence is not
guaranteed.
