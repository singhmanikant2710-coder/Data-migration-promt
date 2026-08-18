Single-file edit: frontend/src/components/pdf/CrmPdGradeMigrationPDF.tsx

Fix (Item 2): Set font size to 9 for the blue column headers AND the value cells in the TWO matrices ONLY (MatrixCount, MatrixCommitment). Do NOT change shared styles (thDark, td) because they're used by detail tables and subreports too.

=== STEP 1 — Matrix headers (both matrices) ===
thCompact is used ONLY by the two matrix header rows. Change its fontSize from 10 to 9.

BEFORE (thCompact style): fontSize: 10
AFTER  (thCompact style): fontSize: 9

=== STEP 2 — MatrixCommitment value cells (already have inline fontSize: 10) ===
In MatrixCommitment, change every inline `fontSize: 10` to `fontSize: 9` in the cell style arrays. These appear in: the body value cells, and the totals-row CAS PD value cells. (There are inline fontSize: 10 occurrences specifically within MatrixCommitment cell style arrays.)

Change each: fontSize: 10  ->  fontSize: 9   (ONLY within MatrixCommitment cell style arrays)

=== STEP 3 — MatrixCount value cells (currently rely on shared td=10, NO inline fontSize) ===
In MatrixCount, add inline `fontSize: 9` to the matrix cell style arrays so they don't depend on shared td. Add it to:
- body value cells (the {String(v)} cells)
- row-end sum cell, rowChanges cell, rowPct cell
- totals-row col-total cells, grandTotal, grandRowChanges, and grand pct cell

For each of these MatrixCount cells, add `fontSize: 9` into the existing inline style object (next to textAlign: "center").

Example (body cell):
BEFORE: style={[styles.td, { flexBasis: "4.25%", flexGrow: 0, flexShrink: 0, textAlign: "center", backgroundColor: bg, color: fg }]}
AFTER:  style={[styles.td, { flexBasis: "4.25%", flexGrow: 0, flexShrink: 0, textAlign: "center", fontSize: 9, backgroundColor: bg, color: fg }]}

=== ALSO — the "CAS PD Totals" label cells in BOTH matrices ===
Both matrices' totals-row label uses [styles.th, { ..., fontSize: 10 }]. Change that inline fontSize: 10 -> 9 in BOTH matrices (so the label matches the row).

CONSTRAINTS:
- Do NOT change shared styles.thDark, styles.td, styles.th definitions themselves.
- Only change: thCompact fontSize (10->9), and inline fontSizes within the two matrices' cell style arrays.
- Do NOT touch DetailTable, Subreports, or any non-matrix section.
- Do NOT change flexBasis, textAlign, colors, tdClamp, or widths.
- Show the FULL diff so I can confirm no shared style was altered and no matrix cell was missed.
