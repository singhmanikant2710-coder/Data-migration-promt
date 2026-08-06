Single-file edit: frontend/src/components/pdf/CrmPdGradeMigrationPDF.tsx

Fix the bottom Totals row splitting across pages (currently the Totals row has 
no wrap, so it can break mid-row — label on one page, values on another — 
which must never happen). Generic fix that works for ANY row count (5, 13, 20, 
30 rows). Apply to BOTH MatrixCount and MatrixCommitment.

Do NOT wrap the whole matrix (large matrices exceed one page and must stay 
splittable). Only protect the Totals row + footnote as an atomic unit.

CHANGE 1 — Make the Totals row atomic (never splits mid-row):
The bottom Totals row is currently:
    <View style={[styles.tr, styles.trLast]}>   ...Totals cells...   </View>
Add wrap={false} so the entire Totals row stays together on one page (label + 
all values never split across a page boundary):
    <View style={[styles.tr, styles.trLast]} wrap={false}>   ...cells...   </View>
Apply in BOTH matrices.

CHANGE 2 — Keep the Totals row from orphaning (push it with enough space ahead):
Add minPresenceAhead to the Totals row so that if there isn't enough vertical 
space left on the page for the full Totals row, React-PDF moves the whole 
Totals row to the next page instead of cramming/splitting it. Add to the 
Totals row's style a minPresenceAhead sized to the Totals row height (~26pt):
    <View style={[styles.tr, styles.trLast, { minPresenceAhead: 26 }]} wrap={false}>
(This ensures the Totals row never sits alone at the very bottom edge getting 
clipped — it moves down cleanly with its data intact.)

CHANGE 3 — Bind the footnote to the Totals row (stay together):
Currently the footnote is a sibling after the table, with no binding, so it 
can drift to the next page alone. Keep the footnote visually tied to the 
Totals row by adding minPresenceAhead on the Totals row large enough to also 
reserve space for the footnote, OR (cleaner) keep the footnote's marginTop 
small so it hugs the table. Set the Totals row minPresenceAhead to ~50 (Totals 
row ~26pt + footnote ~24pt) so the renderer keeps both together:
    <View style={[styles.tr, styles.trLast, { minPresenceAhead: 50 }]} wrap={false}>
This way, when the Totals row moves to a new page, there is guaranteed space 
for the footnote right after it — they stay together, and the Totals row never 
splits.

SUMMARY of the Totals row wrapper (both matrices):
    <View style={[styles.tr, styles.trLast, { minPresenceAhead: 50 }]} wrap={false}>

CONSTRAINTS:
- Only add wrap={false} + minPresenceAhead:50 to the bottom Totals row in both 
  matrices.
- Do NOT wrap the whole matrix or the data rows (they must stay splittable for 
  large datasets — 20+ rows).
- Do NOT change calculations, fmt(), widths, colors, labels, or the footnote 
  text/placement.
- Keep the footnote where it is (sibling after the table).
- This is generic — works for any row count, no dataset-specific logic.
- Only edit this one file. Show the updated Totals row wrapper in both matrices.
