#254 – Fixed. Tested in Dev and pushed to the develop branch. Not yet deployed to QA.

What was wrong: #235 already stopped the comments from disappearing when a new row is added, but your sequence uncovered a second problem. The comments and mitigation are a single value for the whole review, yet the screen kept a separate copy for each row. When comments were edited after adding another row, Save could pick up the older copy instead of the latest edit, which is why the original comments came back.

What changed: Editing the comments or mitigation now updates every row at once, so whichever row is selected shows the same value and Save always keeps the latest edit. This applies to both unsaved and previously saved comments.

Tested in Dev: your exact sequence; saved comments followed by adding rows and editing; changing mitigation; deleting a row after editing comments; and saving other changes without touching the comments (stored comments unchanged).

Covenants is unaffected. Please re-test once deployed to QA.
