#248 – Implemented. Tested in Dev and pushed to the develop branch. Not yet deployed to QA.

What changed:
- The "Cancel" button in the comment box toolbars now reads "Undo" on every Review Form tab. Undo puts the box back to its last saved text. (The Email window and Help Tips maintenance keep their "Cancel" button.)
- While doing this we found and fixed an existing problem: pressing the comment box button without first clicking into the box could empty it, and saving would then remove the stored text. Undo now never empties a box that was not edited.
- Clicking "Cancel" in the top Review Form banner now asks "Discard unsaved changes?" when there are unsaved edits. "Keep editing" returns to the form with all edits kept; "Discard changes" works as Cancel did before. If there are no unsaved edits, Cancel closes edit mode straight away without asking.

Tested in Dev: Undo with and without editing, Undo followed by Save, saving edits across several tabs, and the top Cancel with and without unsaved changes.

I will confirm here once deployed to QA.
