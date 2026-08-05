Single-file edit: frontend/src/components/pdf/CrmPdGradeMigrationPDF.tsx

Add two calculated rows ("Changes" and "% Change") below the Totals row in 
BOTH MatrixCount and MatrixCommitment, and update the column labels for 
13-16. Client-confirmed formulas.

=== COLUMN LABELS (both matrices) ===
In the header row of BOTH MatrixCount and MatrixCommitment, where columns 
1-16 are rendered as String(pd), change the labels for columns 13-16 to 
include suffixes:
  13 -> "13 / SM"
  14 -> "14 / SUB"
  15 -> "15 / DFUL"
  16 -> "16 / LOSS"
Columns 1-12 stay as their plain number. 
Suggested: replace {String(pd)} with a label lookup, e.g.:
  const colLabel = (pd) => 
    pd === 13 ? "13 / SM" : pd === 14 ? "14 / SUB" : 
    pd === 15 ? "15 / DFUL" : pd === 16 ? "16 / LOSS" : String(pd);
Then render {colLabel(pd)} in the header cells of both matrices.

=== "CHANGES" ROW (both matrices) ===
Below the existing Totals row, add a "Changes" row. For each CAS PD column i 
(toPd = i+1):
  Changes[i] = colTotals[i] - diagonal[i]
where diagonal[i] is the no-change cell for that column (fromPd === toPd):
  - MatrixCount: diagonal[i] = byFromTo.get(`${i+1}|${i+1}`) || 0
  - MatrixCommitment: diagonal[i] = (byFromTo.get(`${i+1}|${i+1}`) || 0) / 1_000_000  
    (divide by 1M to match the $MM display units used for cells/colTotals)

Compute the changes array before the return, e.g.:
  const changes = colTotals.map((tot, i) => {
    const diagKey = `${i+1}|${i+1}`;
    const diag = <MatrixCount: (byFromTo.get(diagKey) || 0)> 
                 <MatrixCommitment: (byFromTo.get(diagKey) || 0) / 1_000_000>;
    return tot - diag;
  });
  const changesGrandTotal = changes.reduce((a, b) => a + b, 0);

Render a new row (same structure as Totals row):
  - First cell: "Changes"
  - Per-column: the changes[i] value (MatrixCount: integer count; 
    MatrixCommitment: money($MM), same formatting as the cells — show blank/
    "$0" for zero exactly as the Totals row does)
  - Last cell: changesGrandTotal (right-aligned, same format as Totals grand)

=== "% CHANGE" ROW (both matrices) ===
Below the "Changes" row, add a "% Change" row. For each column i:
  pctChange[i] = colTotals[i] > 0 ? (changes[i] / colTotals[i] * 100) : 0
Render:
  - First cell: "% Change"
  - Per-column: `${pctChange[i].toFixed(1)}%` (e.g. "0.0%", "51.9%"), centered/
    right-aligned to match the row style
  - Last cell: overall = changesGrandTotal / grandTotal * 100 (guard: 
    grandTotal > 0 ? ... : 0), formatted as "X.X%"

CONSTRAINTS:
- Use the EXISTING colTotals, byFromTo, grandTotal already computed in each 
  matrix — do NOT recompute or change existing aggregation.
- "Changes" = column total minus diagonal (off-diagonal). "% Change" = 
  Changes / that column's total (per-column basis). Confirmed by client.
- Match the visual style of the existing Totals row (cell widths, alignment, 
  zero-handling: MatrixCount blank for 0, MatrixCommitment "$0" for 0 — 
  mirror whatever the Totals row does).
- For MatrixCommitment, remember cells/colTotals are in $MM (divided by 1M), 
  so diagonal must also be divided by 1M for consistent subtraction.
- Do NOT change the data cells, the existing Totals row, colors, or any 
  other section.
- Do NOT touch pageSetup.ts, backend, or other files.
- Apply to BOTH MatrixCount and MatrixCommitment.
- Only edit this one file. Show: the colLabel change, the changes/% Change 
  computation, and the two new rows for both matrices.
