# Write-back merge — kit first

**Shared fact.** Both Cloud agents. A writes `cloud/ab-a`, B writes
`cloud/ab-b`, both pull `main`. Conflicts are normal. Sitting still to
"carefully integrate" is a jam.

## Physical step wins

If a walk, talk, trade, or kill is pending:

```
git merge --abort 2>/dev/null || true
git rebase --abort 2>/dev/null || true
```

Attach and do the step. Resolve git after the inventory / tile / quest var
changes. A write-up that is not pushed can wait; a parked body cannot.

Time-box a resolve to **one** `git add` + commit. If you are still reading
diffs after that, abort and play.

## When you do merge `origin/main` into your branch

Do not edit the other lane's results file. Do not rewrite Codex / the other
agent's cells into a cleaner story.

| File | On `cloud/ab-a` | On `cloud/ab-b` |
|---|---|---|
| `learnings/ab-results-a.md` | `--ours` | `--theirs` |
| `learnings/ab-results-b.md` | `--theirs` | `--ours` |
| `learnings/server-diffs.md` | keep **both** `[x]` boxes | same |
| playbooks you did not write this commit | `--theirs` | `--theirs` |

`--ours` is your branch, `--theirs` is `main`, during `git merge origin/main`.

```
git fetch origin
git merge origin/main
# on conflict, take the table above, then:
git add learnings/
git commit -m "Merge main; keep lane results"
git push
```

`git pull` at the **start** of a job only if it fast-forwards. Conflict at
job start → abort, play, merge later.

## Operator tell

Stream says merge / conflict / "integrating both" **and** the tile has not
moved → force-send: stop merging, attach, next physical step.
