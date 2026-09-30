BUG: typing a value in "Inventory" makes "Inventory Turn" show the same
number (ATHENS new 202604, before Save). Inventory Turn is class B: it
must show the stored curInventoryTurn only, "—" when NULL / unsaved
month, and never another field.

1. Quote every place that renders Inventory Turn (Top Strip, Monthly
   Summary, mappings, registry, DetailGrid) and show where it resolves
   to curInventory (fuzzy pick / contains / prefix match, alias list,
   or the unsaved-month blanking not applied because the tile is
   editable/not classified).
2. FIX: Inventory Turn reads curInventoryTurn with exact matching only
   (pickExact) on all surfaces; no fuzzy fallback. Check the same risk
   for A/R Turn Days (curAccountsReceivable) and any other class-B tile
   whose name starts with an input field's name; fix the same way.
Frontend only; no calculation or data change.
GOLDEN: ATHENS 202603 Inventory Turn 57, A/R Turn Days 61 unchanged;
new 202604: typing Inventory / A/R leaves Inventory Turn / A/R Turn Days
"—" until Save, then 57 / 61-style legacy values.
Build, tests, do not commit. Report root cause (file:line), ADDED/
REMOVED, BEHAVIOUR CHANGE incl. NULL, NOT TOUCHED.
