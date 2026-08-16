# Learnings — who reads what

Need to know: keep the paste / always-on rule short. Open **one** file when
the class shows up. Do not stuff lookup, dialog, and observe into every
prompt.

| When | Open |
|---|---|
| Cannot find an NPC / loc / “coords are wrong” | [`lookup.md`](lookup.md) |
| Dialog picked the wrong option | [`dialog.md`](dialog.md) |
| Walk / death / HP / who killed you | [`observe-fidelity.md`](observe-fidelity.md) |
| `git` conflict / sitting on `ab-results` | [`merge.md`](merge.md) |
| Quest order at 25× / att vs str / style | [`bootstrap-quest-stack.md`](bootstrap-quest-stack.md) |
| Gear elite / who is online | [`top-players.md`](top-players.md) |
| Runite, hides, shop ladder, B priority | [`scarce-goods.md`](scarce-goods.md) |
| Who does what right now | [`strategy-compete-koth.md`](strategy-compete-koth.md) “Current swarm” |
| New bot name / fleet prefix | [`names.md`](names.md) |

Three audiences. Do not mix them.

| Audience | Files | Do not |
|---|---|---|
| **Operator** (laptop manager) | [`operator.md`](operator.md), [`operator-ingest.md`](operator-ingest.md), [`operator-handoff.md`](operator-handoff.md), [`.cursor/rules/koth-steer.mdc`](../.cursor/rules/koth-steer.mdc) | Paste operator notes into Cloud A/B. Run lite on this machine. Micromanage every quiet tick. |
| **Cloud Agent A / B** | [`cloud-agent-a.md`](cloud-agent-a.md), [`cloud-agent-a-phase2.md`](cloud-agent-a-phase2.md), [`cloud-agent-b.md`](cloud-agent-b.md) | Read `operator.md`. Do the other agent's lane. |
| **Shared facts** (both) | [`strategy-compete-koth.md`](strategy-compete-koth.md), [`server-diffs.md`](server-diffs.md), [`bootstrap-quest-stack.md`](bootstrap-quest-stack.md), [`names.md`](names.md), [`top-players.md`](top-players.md), [`scarce-goods.md`](scarce-goods.md), [`lookup.md`](lookup.md), [`dialog.md`](dialog.md), [`observe-fidelity.md`](observe-fidelity.md), [`merge.md`](merge.md), [`live-probes-2026-08-15.md`](live-probes-2026-08-15.md), [`ab-results-a.md`](ab-results-a.md), [`ab-results-b.md`](ab-results-b.md) | Put steer tactics or API keys here. |

Owner / Discord leads (operator first, agents only if the prompt says so):
[`owner-context.md`](owner-context.md), [`discord-meta-2026-08-15.md`](discord-meta-2026-08-15.md),
[`koth-swarm-snapshot-2026-08-15.md`](koth-swarm-snapshot-2026-08-15.md).

Skill snippets below are for **whoever is running a bot script**, not the
operator loop.

---

# Agent learning snippets

Unless a section explicitly says otherwise, snippets in this directory are
written for the MCP `execute_code` environment. That environment provides
`bot` and `sdk` as globals and supports top-level `await`. Although the API
reference uses TypeScript notation, keep `execute_code` bodies
JavaScript-compatible and omit type-only syntax:

```typescript
const state = sdk.getState();
const result = await bot.walkTo(3222, 3218);
return { result, state: sdk.getState() };
```

Standalone files under `bots/<name>/` use a different wrapper. Import
`runScript`, then destructure the same names once:

```typescript
import { runScript } from '../../sdk/runner';

await runScript(async ({ bot, sdk }) => {
    await bot.walkTo(3222, 3218);
    return sdk.getState();
});
```

For a long or background run, execute the standalone file with
`bun bots/<name>/script.ts`. Do not launch a detached bare snippet: `runScript`
owns connection setup, timeout reporting, signal handling, and shutdown.

Do not paste context-prefixed runner expressions into `execute_code`.

## Action semantics

- Prefer high-level `bot.*` helpers. They attempt to observe method-specific
  evidence. Check the result when a method returns one.
- Low-level `sdk.send*` calls confirm browser-client dispatch only. A successful
  result does not prove that the game server applied the action. Observe the
  intended state, XP, inventory, dialog, or message change before continuing.
- Await every method returning `Promise`, including `scanNearbyLocs()` and
  `scanGroundItems()`.
- `getInventory().length` counts occupied slots. An `InventoryItem.count` is the
  quantity in that slot; `findInventoryItem()` returns only the first matching
  slot.

See [`../sdk/API.md`](../sdk/API.md) for exact signatures.

Live-server meta from the project owner (economy, wilderness, what is already saturated) lives in [`owner-context.md`](owner-context.md), which links the dated Discord evidence and source-checked swarm/PK notes. Read that before picking a long-horizon goal.

Compete-and-counter plan: [`strategy-compete-koth.md`](strategy-compete-koth.md).
Server diffs: [`server-diffs.md`](server-diffs.md). 25× quest order:
[`bootstrap-quest-stack.md`](bootstrap-quest-stack.md). Gear elite:
[`top-players.md`](top-players.md). Scarce goods:
[`scarce-goods.md`](scarce-goods.md).
