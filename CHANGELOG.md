# Changelog

## 3.41-octo1 (2026-10) - bug-fix pass for WoW 1.12 / Turtle-like servers

Small, targeted fixes on top of TrinketMenu 3.41 (Gello) + the earlier "Fix" by Ogrisch. The saved-variables format is
unchanged (no migration needed). Everything below was checked against a simulated 1.12 world (bags, equipment, cursor,
item locks, combat, cooldowns) and against the real saved variables of the OctoWoW characters; none of it could be
run in the real game client from here.

### Fixed

**Auto queue / combat queue**
- A trinket queued during combat or while dead was **lost** if the swap could not be made at the instant combat ended
  (cursor holding an item, item locked, resurrected while still in combat, ...). The queue is now kept and retried every
  second until the swap really starts (`TrinketMenu.ProcessCombatQueue`).
- The auto queue did not know about the trinkets lying in your bags until the next `BAG_UPDATE`, so after a login nothing
  was swapped in until you looted or moved something. The bags are now scanned at login, trinkets the client does not
  know yet (item not cached) are looked at again a few times, and a remembered bag spot that went stale is searched again.
- The auto queue and the combat queue wait while ItemRack is swapping a set (`Rack.SetSwapping`) instead of fighting
  over the trinket slots.
- `table.setn(list,0)` does not empty a Lua 5.0 table: loading a shorter profile (or `TrinketMenu.SetQueue(...,"SORT",...)`)
  left the old entries in the list and they came back in the saved variables. Lists are now really emptied.
- `FindItem` looked for the name as a plain substring, so "Foo" could find "Foo of Bar" in an earlier bag slot. The whole
  `[name]` is looked for first, then any part of the name (partial names in macros still work).

**Notifications**
- "Notify when ready" never worked: the hooks on `UseInventoryItem` / `UseAction` were commented out, so no use was ever
  recorded. The hooks are back (installed once, original function called first so they can never block a trinket
  use). Also fixed in them: operator precedence (`slot==13 or slot==14 and not Merchant...`) and the action name being
  read from `GameTooltip` instead of the addon's own scanning tooltip.

**Saved variables / options**
- Options missing from older or hand-edited saved variables (`KeepOpen`, `Locked`, `ShowIcon`, `CooldownCount`, ... and
  every per-character key, `TrinketMenuQueue.Stats/Enabled/Profiles`) are now filled with their defaults instead of causing
  nil errors or options that could not be toggled. Silly values (scale <= 0, columns < 1) are replaced.

**Sort / profile window**
- `TrinketMenu_SortDelay` (the "Delay" box) was declared `virtual="true"` in `TrinketMenuQueue.xml`, i.e. only a
  template: the frame was never created and every click in the queue list could fail on a nil frame. Now a real frame.
- Selecting nothing (or the "stop queue here" mark) and writing the delay box raised "table index is nil".
- Items the client has not cached made the list fail (`GetItemQualityColor(nil)`); they now show as "Item <id> (not cached)".
- `idx` leaked as a global in the sort and profile lists.
- `/trinket load top|bottom <profile>`: profile names with several words did not work, and an unknown name **wiped the
  sort list**. The name is now parsed correctly and an unknown profile only prints a message.

**Display / misc**
- Worn trinket icon is taken from the slot itself when `GetItemInfo` has nothing cached (it could be left blank).
- "Tiny tooltips": `GameTooltip:AddLine(nil)` error for trinkets without charges or cooldown.
- Cooldown numbers / spinners: no error for an empty trinket slot (nil cooldown values).
- Shift+click on an empty slot / vanished item no longer errors (`ChatFrameEditBox:Insert(nil)`).
- Scale: `/trinket scale ... 0`, a non-number, or a resize tick without a frame no longer breaks the window.
- Column slider label is correct right after login.
- Duplicate `OnHide` script on the main frame removed (same effect, one handler).
- Plain (non-pattern) searches for item names, so names with `-` or `(` cannot break `AddWatchItem`.

### Not changed
- Sort order, Stats and profiles format, SavedVariables names, `## Interface: 11200`, folder name.
