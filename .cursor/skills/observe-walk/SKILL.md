---
name: observe-walk
description: Log HP, lifeId, hostiles, and death chat on a timed walk. Use when clocking a rejoin, PvP death, or any walk where “walkTo succeeded” is being treated as “no deaths.”
---

# Observe a walk

Read [`learnings/observe-fidelity.md`](../../learnings/observe-fidelity.md).
A clock that cannot name the killer is invalid. Public `/status/:name` is
not HP — use `sdk.getState()`.
