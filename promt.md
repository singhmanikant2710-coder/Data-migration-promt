READ-ONLY. Do NOT edit. Investigate the regression.

Our #193 change (added alignSelf:"flex-start" + width:"auto" to styles.table)
caused a WORSE problem: in the PDF, table columns now collapse so narrow that
cell text OVERLAPS and is unreadable (e.g. "Testing" overlapping "ABC1", numbers
stacked). The full-width stretch is gone but the table is now broken.

Report only, no edits:

1. Show the current styles.table (with our added alignSelf:"flex-start",
   width:"auto") and styles.tableCell (flexGrow:1, flexBasis:0).

2. Explain why width:"auto" + flexGrow:1/flexBasis:0 cells causes columns to
   collapse and text to overlap in @react-pdf/renderer. 

3. Propose the SAFEST minimal fix that:
   - keeps the table from stretching to full page width (the original #193 goal),
   - but does NOT collapse columns / overlap text,
   - and does NOT break the many existing tables in memos that were fine before.
   Options to evaluate: (a) remove width:"auto", keep alignSelf, add a sensible
   maxWidth; (b) give cells a minWidth so they can't collapse; (c) fully revert
   the #193 table change (back to original full-width but readable).
   Recommend which is safest for a banking report with many varied tables.

Output: current styles + why it breaks + recommended safe fix. No edits.
