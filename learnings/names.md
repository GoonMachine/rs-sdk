# Goonmachines — names

**Shared fact.** The fleet is **Goonmachines**. Cloud VMs stay A and B
(same ids). Operator may say Goonmachine A / Goonmachine B. Do not launch
Cloud C.

Game names are max **12 alphanumeric** (`bun bots/create-bot.ts`). Never
commit `bots/*/`. Never print `bot.env`.

## Keep these (do not recreate)

Progress lives on the existing characters. Same name + new password does
**not** log in.

| Name | Lane | Job |
|---|---|---|
| `qstboot1` | Goonmachine A | 25× quest stack |
| `qstprobe1` | leftover | Phase 1 scout. Not a job. |
| `foodprobe1` | Goonmachine B | Mule + death-watch |
| `foodkill1` | Goonmachine B | Idle until a real 1 HP clock |
| `kitprep1` | Goonmachine B | First Wydin-food gatherer (already created) |

## New characters: `goon` + role + index

| Role | Pattern | Next free | Job |
|---|---|---|---|
| Quest | `goonqstN` | `goonqst1` | Later quest bodies. Not a second Waterfall on A while `qstboot1` is mid-step. |
| Mule | `goonmuleN` | `goonmule1` | Trade / bank. Not a replacement for `foodprobe1`. |
| Kit / gather | `goonkitN` | `goonkit1` | Food, hides, bars. After `kitprep1`. |
| Pile | `goonpileN` | `goonpile1` | Skulled focus-fire. Four-account canary. |
| Scorer | `goonscorN` | `goonscor1` | Uniquely highest combat. Never skulls. |
| Recovery | `goonrecN` | `goonrec1` | 1 HP clock / Lumbridge pickup |

Examples that fit the 12-char cap: `goonqst1`, `goonmule1`, `goonkit1`,
`goonpile1`, `goonscor1`, `goonrec1`.

## Do not

- `goo*` (their fleet)
- `foodboot1`, `agentmachine`, `cdx*`, random 9-char names
- A new password on an old name
- A new Cloud VM to “rename” a character
