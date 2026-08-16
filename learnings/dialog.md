# Dialog — click by text, once

**Shared fact.** Skill dialogs and quest menus are not in "product order."
A second click on Continue as a 3-option menu renders will eat option 1.

Restless Ghost (`father_aereck.rs2`): the start line is **"I'm looking for a
quest!"** Option 1 is **"Who's Saradomin?"** — that does **not** set
`%prieststart`. A double-clicked Continue started the Saradomin branch and
the quest never began, so Urhney had no "Aereck sent me" line.

```javascript
const d = sdk.getDialog();
if (d?.options?.length) {
  await sdk.clickDialogByText(/looking for a quest/i);
} else {
  await sdk.sendClickDialog(0); // continue only when there is no choice list
}
```

Dedupe by dialog signature (npc text + option labels). Never click the same
page twice. Prefer `clickDialogByText` / `bot.navigateDialog` over raw
indices. Log the options you saw if a talk "succeeds" and the quest var /
inventory does not change.

Waterfall (`hudon.rs2`): the raft's forced Hudon chat only sets
`spoken_to_hudon` if Hudon is found 1 tile west of the crash
(`quest_waterfall.rs2` raft `npc_find`). A miss leaves `%waterfall_quest`
at `started`; Hadley's bookcase is empty until you **talk to Hudon on the
island** (case `started`). Do not treat "I already rafted" as Hudon done.
