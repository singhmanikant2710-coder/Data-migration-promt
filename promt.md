Single-file edit: frontend/src/components/pdf/CrmPdGradeMigrationPDF.tsx

Fix (Item 3): Vertically center specific matrix rows by adding alignItems: "center" inline to their row containers. styles.tr is shared across all tables, so do NOT modify tr — add alignItems inline only on these matrix rows.

Per Geoff: Accounts matrix = only the CAS PD Totals row; Commitment matrix = all value rows AND totals row.

=== CHANGE 1 — MatrixCount: CAS PD Totals row only ===
BEFORE:
<View style={[styles.tr, styles.trLast, styles.trTotalsReserve]} wrap={false}>
AFTER:
<View style={[styles.tr, styles.trLast, styles.trTotalsReserve, { alignItems: "center" }]} wrap={false}>
(NOTE: this exact same JSX pattern appears in BOTH matrices' totals rows. For THIS change, only apply to the MatrixCount totals row. See Change 3 for MatrixCommitment's totals row.)

=== CHANGE 2 — MatrixCommitment: body/data rows ===
BEFORE:
<View key={`row-amt-${r}`} style={[styles.tr, r1 === rowsWithMetrics.length - 1 ? styles.trLast : {}]}>
AFTER:
<View key={`row-amt-${r}`} style={[styles.tr, r1 === rowsWithMetrics.length - 1 ? styles.trLast : {}, { alignItems: "center" }]}>

=== CHANGE 3 — MatrixCommitment: totals row ===
BEFORE (the MatrixCommitment CAS PD Totals row):
<View style={[styles.tr, styles.trLast, styles.trTotalsReserve]} wrap={false}>
AFTER:
<View style={[styles.tr, styles.trLast, styles.trTotalsReserve, { alignItems: "center" }]} wrap={false}>

CONSTRAINTS:
- Do NOT modify styles.tr (it's shared).
- Add alignItems: "center" ONLY to: MatrixCount totals row, MatrixCommitment body rows, MatrixCommitment totals row.
- Do NOT add it to MatrixCount body rows (Geoff only asked for the Accounts totals row).
- Do NOT touch Detail table, Subreports, or any other rows.
- The two totals-row JSX lines are identical between matrices — be careful to edit the RIGHT one in each matrix (Change 1 = MatrixCount, Change 3 = MatrixCommitment). Show enough surrounding context in the diff to confirm which matrix each edit is in.
- Show the FULL diff.
