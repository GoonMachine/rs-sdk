# Live KOTH and death-rule probes — 2026-08-15 EDT

Read this with [`strategy-compete-koth.md`](strategy-compete-koth.md) and the
dated [`KOTH snapshot`](koth-swarm-snapshot-2026-08-15.md). This note records
source checks and disposable live probes performed against
`rs-sdk-demo.fly.dev`. It supersedes the snapshot wherever exact coordinates
or live results disagree.

## Results at a glance

- The recurring formation is **one eligible scorer plus seven bodies parked
  immediately outside the checked-in polygon**, not eight simultaneous
  scorers: `Tqckgxgj08` at `(3288,3886)` is inside; `(3284,3884)`, where the
  other seven stack, is outside.
- The formation is dynamic. All eight briefly converged onto inside tiles and
  `Goo001` joined, then the original 1+7 arrangement returned. Do not hard-code
  either 8 or 9 as a required fleet size.
- A self-owned PvP death reproduced the source's three-item selector exactly,
  including one-unit stack protection and equipment returning to inventory.
- A gearless combat-10 scout reached direct sight of the scorer from
  `(3303,3878)`, but greater demons killed it before the final outside tile.
  A direct central route also crossed dense poison spiders. The approach is a
  supply/pathing problem even before player combat.

## Exact KOTH boundary and scoring

[`Koth.ts`](../server/engine/src/engine/Koth.ts) tests plane-0 tile centers
against its polygon. The two important observed tiles evaluate as:

| Tile | Checked-in result | Repeated live use |
|---|---|---|
| `(3288,3886)` | Inside | `Tqckgxgj08`, the recurring scorer position |
| `(3284,3884)` | Outside | Seven-account support stack |

Other mechanics confirmed in source:

- The server takes at most one sample every 60,000 wall-clock milliseconds;
  samples are not aligned to UTC minute boundaries and missed intervals are
  not backfilled.
- Eligible means inside the polygon, plane 0, ordinary visibility, and
  `staffModLevel <= 1`.
- Highest combat wins. The incumbent wins an equal-highest tie; without an
  eligible incumbent, an equal-highest winner is random.
- An empty sampled hill clears incumbency. Leaving and returning between
  samples is invisible.
- Holder-change chat goes to everyone within 24 Chebyshev tiles of
  `(3289,3886)`, whether inside or outside. It cannot validate boundary
  membership or recurring capture minutes.

The public KOTH page exposes aggregate minutes and coarse `last held`, but not
the recorded combat level or contender count. The correct query parameter is
`window=day|week|all`; older `period=today|week` links are ignored. Day and week
are rolling windows and can decrease, so use all-time totals for short passive
delta checks.

### Live formation sequence

During the pass:

1. The same 1+7 formation appeared repeatedly: `Tqckgxgj08` inside and the
   other seven at the outside tile; all 25 `goo` accounts were online and none
   was initially on the hill.
2. While the positions stayed 1+7, `Tqckgxgj08`'s monotonic all-time total
   advanced from 1,627 to 1,628 while `Goo001` remained at 8,510. This strongly
   corroborates the source interpretation, though public data still does not
   expose contender count.
3. The original eight then briefly moved onto checked-in inside tiles and
   `Goo001` joined them. Within roughly a minute, `Goo001` left and the 1+7
   arrangement returned. Purpose and common control remain unknown.
4. At 21:36 EDT, world population was 900, the 1+7 formation was again present,
   and no `goo` account was in the ruins box. All-time totals were then 1,646
   for `Tqckgxgj08` and 8,513 for `Goo001`, consistent with `Goo001` capturing
   during later brief entries.

This is better described as a **scorer plus a seven-body response stack** than
as eight bodies continuously scoring. The exact jobs of the seven—pile,
blocking, reserve, or something else—are still inference.

## Live death-keep probe

Two fresh self-owned accounts were used away from the hill:

- victim: `Cdxvict815`, unskulled and combat 3 at setup
- killer: `Cdxkill815`, the sole initiator

The victim carried exactly a bronze dagger and a 25-arrow stack and wore a
wooden shield. Source-configured unit costs predict the three keep operations
will choose shield, dagger, then **one** arrow.

Observed result after the killer won:

- Victim respawned in Lumbridge at `(3219,3219)`.
- First observer read found 2/10 HP. Source sets 1 HP, but the exact respawn
  frame was not captured and a regeneration boundary may have passed; live
  frame-1 HP remains unverified.
- Victim inventory contained wooden shield ×1, bronze dagger ×1, and bronze
  arrow ×1. The shield was returned to inventory rather than re-equipped.
- Killer saw bronze arrow ×24 and bones at the death tile `(3265,3588)`.

This live death proves that production selects across worn and carried items
and protects one unit of a stack per keep operation. It also matches the
checked-in configured-cost order for this fixture.

The source matrix remains:

| State at fatal hit | Kept |
|---|---:|
| Unskulled, Protect Item off | 3 |
| Unskulled, Protect Item on | 4 |
| Skulled, Protect Item off | 0 |
| Skulled, Protect Item on | 1 |

Protect Item and skulled variants were **not** live-tested in this pass because
the fresh victim had Prayer 1. The `goo` rune scimitar + rune chainbody + rune
platelegs set therefore remains a strong source-backed fit for an unskulled
three-item kit, not proof of the fleet's intent. A skulled wearer would keep
none; Protect Item would keep only the 64,000-cost platelegs.

## Disposable scout and approach route

The first direct scout route crossed poison spiders around `(3284,3799)`.
`Cdxkill815` arrived there at 1/15 HP and poisoned, then died during the final
sprint. After respawn it was empty, unskulled, and retried by the eastern route:

`(3335,3528) → (3334,3650) → (3334,3769) → (3335,3870) → (3303,3878)`

The eastern corridor itself was safe for the empty combat-10 body. At
`(3303,3878)` it directly observed `Tqckgxgj08` 15 tiles away, with two greater
demons only three tiles away. The demons killed it during the final 19-tile
attempt to `(3284,3884)`.

Operational consequence: do not route a fresh combat-3 scout straight through
the spider square, and do not treat the final east approach as safe merely
because it avoids poison. A useful next scout needs a source-checked demon-safe
line or enough food/poison mitigation and combat durability to cross the last
screen. No valuable items should be carried.

## Safe watch links

- [Live visual map](https://rs-sdk-demo.fly.dev/mapview/) — public, no
  credentials, player labels at sufficient zoom.
- [`Cdxkill815` status](https://rs-sdk-demo.fly.dev/status/cdxkill815) — exact
  state age and session counts while it is connected.
- [Exact current positions](https://rs-sdk-demo.fly.dev/playerpositions)
- [KOTH all time](https://rs-sdk-demo.fly.dev/hiscores/koth?window=all&profile=main)
- [KOTH today](https://rs-sdk-demo.fly.dev/hiscores/koth?window=day&profile=main)

There is no safe passive per-bot 3D spectator URL. `/bot?...&password=...`
contains credentials and logs into the account, which can replace the active
client. Do not publish or open it alongside a lite runner.

## Still unverified live

- Exact HP on the first PvP respawn frame.
- NPC death while the PvP death mark is active and the mark's full duration.
- Skulled and Protect Item death branches.
- Direct outside-then-inside capture by a disposable account while the hill is
  empty; a combat-3 scout cannot produce that evidence while a maxed holder is
  present.
- Exact live contender counts, inventories, food, prayers, skulls, and shared
  controller ownership.
