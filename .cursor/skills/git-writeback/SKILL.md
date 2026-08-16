---
name: git-writeback
description: Resolve git merge conflicts on cloud/ab-a or cloud/ab-b without stopping the bot. Use when pull/merge conflicts, both datasets look important, or you are about to sit still editing ab-results / server-diffs.
---

# Git write-back

Read [`learnings/merge.md`](../../learnings/merge.md).
Physical step first. Abort the merge, attach, play. Resolve later using
the ours/theirs table. Do not rewrite the other lane's results file.
