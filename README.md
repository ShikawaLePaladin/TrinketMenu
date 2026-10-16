# TrinketMenu (1.12 / Turtle-like servers)

TrinketMenu shows your two equipped trinkets in a small bar. Hover one to open a menu of every trinket in your bags,
left click a bag trinket to equip it in the top slot, right click for the bottom slot. It also has an **auto queue**
(sort your trinkets, and they are swapped in as they come off cooldown), a queue for swaps requested in combat or while
dead, cooldown numbers, "ready" notifications, key bindings (`Use Top Trinket`, `Use Bottom Trinket`, `Toggle TrinketMenu`)
and a minimap button. Slash commands: `/trinket`, `/trinket opt`, `/trinket lock|unlock`,
`/trinket scale main|menu <n>`, `/trinket load top|bottom <profile>`, `/trinket reset`.

The original, unmodified documentation is in `readme.txt`.

## Credits
- **Gello** - original author of TrinketMenu (3.41, 2006).
- **Ogrisch** - the "Fix" release this folder is based on.
- **S4V4GENZ** - upstream repository (`github.com/S4V4GENZ/TrinketMenu`).
- Bug-fix pass 2026-10 (version `3.41-octo1`) for the OctoWoW server - see [CHANGELOG.md](CHANGELOG.md).

## Installation
Copy the `TrinketMenu` folder into `Interface\AddOns\` of your WoW 1.12 client (the folder must keep that exact name,
so that the saved variables `TrinketMenuOptions`, `TrinketMenuPerOptions` and `TrinketMenuQueue` are found again).
Existing saved variables keep working.

## What was fixed in 3.41-octo1
- combat / death queue no longer loses a trinket when the swap cannot be made right when combat ends;
- auto queue knows the bagged trinkets right after login, waits while ItemRack is swapping a set;
- "Notify when ready" works again (use hooks restored);
- missing options in old saved variables are filled with defaults (no more nil errors);
- the queue window: real "Delay" box, no "table index is nil", uncached items shown as "Item <id>";
- profiles: loading a shorter profile leaves no leftovers; `/trinket load` handles names with spaces and unknown names;
- tiny tooltips, empty trinket slot, shift+click, scaling and several other small errors.

Details in [CHANGELOG.md](CHANGELOG.md).
