CONFIRMED Hypothesis B — Network tab shows TWO DISTINCT saves with DIFFERENT payloads (not a duplicate of the same call):
Save #1: includes customerInfo:{change:"Upsert", data:{...}} AND transactions.
Save #2: includes ONLY transactions (customerInfo missing — already cleared by save #1).
Both stack traces show onClick as the trigger, not an effect.

This means there are TWO independent click-triggered save paths, not a re-entrant single path. Find the FIRST save's trigger — it is NOT handleSaveAndLeave (or if it is, something is calling save from a different click before Save-and-leave fires). READ-ONLY, no edits. Answer, STOP.

1. Is there a SEPARATE save call bound to a field's onBlur (e.g. the Customer Background RichTextEditor) that fires when focus leaves the field — independent of the Save/Save-and-leave buttons? Search for any handleBlur / onBlur that calls saveReview or a save function, not just normalizeHtml.
2. Trace the exact sequence: user clicks "Home" while focus is still in the Customer Background editor. Does the click on "Home" (or on the "Save and leave" button inside the resulting modal) cause a BLUR event on the editor to fire FIRST, and does anything hook that blur to trigger a save?
3. Is there any auto-save-on-blur or auto-save-on-section-change feature (separate from UAT #177's draft/localStorage layer) that calls the REAL save API (not just localStorage) when a field loses focus or a section is exited?
4. Check TopChromeBar's Save button and any other "Save" trigger that could be clicked/activated between when the user clicks Home and when the "Save and leave" button in the modal is actually clicked — e.g. does clicking "Home" itself, before the modal appears, briefly trigger something?
5. Report the exact second (actually FIRST, chronologically) save call site — the one that fires with customerInfo before "Save and leave" is even clicked.

Report the exact trigger of the first (customerInfo-bearing) save. Do NOT fix yet.
