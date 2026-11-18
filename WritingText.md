1: A – no unsaved changes → cancel straight away, no dialog.
2: A – keep today's behaviour: Discard clears the #177 draft as Cancel does now.
3: A – rename to "Undo" only in the Review Form tab comment boxes; Email modal and Help Tips keep "Cancel".
4: B – fix it now with the smallest change: Undo must restore the box to its last loaded/saved value and must never empty a box that has stored text, whether or not the user clicked into it first. If nothing was edited in that box, Undo does nothing and stages nothing. Report root cause and the fix separately (file:line), plus before/after for: Undo without clicking in, Undo after typing, Undo then Save (stored text unchanged), box that is genuinely empty.
Proceed with Phase 3 and 4.
