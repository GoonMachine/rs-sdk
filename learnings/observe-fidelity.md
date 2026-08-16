# Observe fidelity — do not lazy-probe

**Shared fact.** Both Cloud agents. A walk that only checks `(x, z)` at the
end is not a measurement.

The SDK already publishes death and combat. Use it. A 1 HP corridor clock
that cannot name the killer is a failed probe.

## Minimum log every waypoint / every 15s while moving

Read `sdk.getState()` (or the formatted snapshot). Record:

| Field | Why |
|---|---|
| `player.worldX`, `worldZ`, `level` | tile |
| `player.hp` / `maxHp` | 1 HP vs full; regen |
| `player.combat.inCombat`, `lastDamageTick`, `targetType` | you are being hit |
| `player.lifeId`, `respawnCount`, `lastDeathTick`, `isDead` | death is a counter, not a vibe |
| `nearbyNpcs` where `inCombat` or `combatLevel > 0` | who is on you |
| `sdk.getNewChat({ types: [0] })` | "Oh dear, you are dead!" |
| `combatEvents` tail | damage in / out |

`/status/:name` on the public demo host is **not** this. It is name + tile +
controller only. Do not treat it as HP.

## What counts as a death

`lifeId` changed, or `respawnCount` increased, or chat matched `/oh dear/i`,
or you were not in Lumbridge and now you are at ~`(3222,3218)` **and** HP is
1. Write **all** of: last tile before snap, HP before snap, nearby NPC names
(especially `/skeleton|demon|spider|wizard|bear|wolf/i`), chat line, new
`lifeId`.

Lumbridge-start as the *only* death check will swallow an NPC death mid-path
and restart the clock as if the intended PK just happened.

## What you must not write

- "no deaths" because `walkTo` returned success
- "2.60 min clean" after a Lumbridge snap you did not attribute
- A resume-from-current-tile after a 90s timeout without dumping `lifeId` /
  HP / nearby NPCs

## Walk loop shape

```javascript
const startLife = sdk.getState().player.lifeId;
const startHp = sdk.getState().player.hp;
// ... walkTo or sendWalk ...
const s = sdk.getState();
const p = s.player;
const hostiles = (s.nearbyNpcs || []).filter(n => n.inCombat || n.combatLevel > 0);
const chat = sdk.getNewChat({ types: [0] });
console.log(JSON.stringify({
  t: Date.now(), tile: [p.worldX, p.worldZ], hp: [p.hp, p.maxHp],
  lifeId: p.lifeId, respawns: p.respawnCount, inCombat: p.combat.inCombat,
  hostiles: hostiles.map(n => `${n.name}@${n.distance}`),
  chat: chat.map(m => m.text),
}));
if (p.lifeId !== startLife) {
  throw new Error(`DEATH lastTile hostiles=${hostiles.map(n => n.name)}`);
}
```

A clock with an unattributed death is invalid. Re-run the segment or mark the
cell `died to <name> at (x,z)`.

`bot.walkTo` can fail with "no waypoints" on a reachable tile when
`collision-data.json` is stale (Wizard Tower ladder, swamp). Low-level
`sdk.sendWalk` / `sdk.sendInteractLoc` use live client collision.
