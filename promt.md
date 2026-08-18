Single-file edit: frontend/src/components/pdf/CrmPdGradeMigrationPDF.tsx

REVERT part of Item 3: Adding alignItems: "center" to the MatrixCommitment BODY rows introduced unwanted horizontal lines/rulings across the value rows. Remove alignItems from the MatrixCommitment body rows ONLY. Keep it on the two totals rows (MatrixCount totals + MatrixCommitment totals), which are fine.

BEFORE (MatrixCommitment body rows):
<View key={`row-amt-${ri}`} style={[styles.tr, ri === rowsWithMetrics.length - 1 ? styles.trLast : {}, { alignItems: "center" }]}>

AFTER (revert to original):
<View key={`row-amt-${ri}`} style={[styles.tr, ri === rowsWithMetrics.length - 1 ? styles.trLast : {}]}>

CONSTRAINTS:
- ONLY remove { alignItems: "center" } from the MatrixCommitment BODY rows.
- KEEP alignItems: "center" on the MatrixCount totals row and the MatrixCommitment totals row (those don't have the issue).
- Do NOT change anything else.
- Show the diff.
