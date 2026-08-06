Single-file edit: frontend/src/components/pdf/CrmPdGradeMigrationPDF.tsx

MAJOR STRUCTURAL CHANGE (client-confirmed): Move "# Changes" and "% Change" 
from bottom ROWS (per CAS PD column) to right-side COLUMNS (per Bank PD row). 
Remove the bottom Changes and % Change rows; KEEP the bottom Totals row. 
Apply to BOTH MatrixCount and MatrixCommitment.

=== PART 1: Remove bottom "Changes" and "% Change" rows ===
In the <View wrap={false}> summary group at the bottom, REMOVE the "Changes" 
row and the "% Change" row entirely. KEEP only the "Totals" row. (The column-
wise changes/pctChange computations that fed those rows can be removed too, 
but only if unused elsewhere — verify.)

=== PART 2: Add two new RIGHT-SIDE columns: "# Changes" and "% Change" ===
Compute PER ROW (for each data row r):
  - rowDiagonal (Count):  byFromTo.get(`${r.fromPd}|${r.fromPd}`) || 0
  - rowDiagonal (Commitment): (byFromTo.get(`${r.fromPd}|${r.fromPd}`) || 0) / 1_000_000
  - rowChanges = r.sum - rowDiagonal
  - rowPct = r.sum > 0 ? (rowChanges / r.sum * 100) : 0

Add TWO new columns AFTER the existing "Totals" column, in BOTH the header 
and every data row AND the bottom Totals row:
  - "# Changes" column: shows rowChanges 
    (Count: integer String(rowChanges); Commitment: money fmt(rowChanges))
  - "% Change" column: shows `${rowPct.toFixed(1)}%`

For the bottom Totals row's two new cells:
  - "# Changes" total = sum of all rowChanges (grand changes)
  - "% Change" total = (grand changes / grandTotal * 100).toFixed(1) + "%"

=== PART 3: Redistribute column widths (add 2 columns, sum = 100%) ===
Current: BANK PD 8% + 16×5% (80%) + Totals 12% = 100%.
Change to (Option A): 
  - BANK PD: 8%
  - 16 PD columns: 4.5% each = 72%
  - Totals: 12%
  - # Changes: 4%
  - % Change: 4%
  Total: 8 + 72 + 12 + 4 + 4 = 100%

Update BOTH header rows and all body/Totals rows to these widths:
- Header Row 1 (grouping): blank 8% | "CAS PD" span 72% (was 80%) | then over 
  the Totals + # Changes + % Change (12+4+4=20%) — either leave that 20% blank 
  or split; simplest: blank 8% | "CAS PD" 72% | blank 20%.
- Header Row 2 (labels): BANK PD 8% | 16 cols 4.5% | Totals 12% | 
  "# Changes" 4% | "% Change" 4%.
- Data rows and Totals row: mirror the same widths, adding the two new cells.

=== PART 4: Page break between the two matrices ===
Add a page break so MatrixCommitment always starts on a new page (client wants 
the Commitment matrix to never split from a page shared with the Accounts 
matrix). Add a self-closing <View break /> BEFORE MatrixCommitment in 
CrmPdGradeMigrationPage:
    <MatrixCount ... />
    <View break />
    <MatrixCommitment ... />
    <DistCharts ... />
(Do NOT add a break before DistCharts — only between the two matrices.)

CONSTRAINTS:
- Per-row changes/%: rowChanges = rowTotal − rowDiagonal; rowPct = rowChanges/
  rowTotal. Confirmed by client (Bank PD-13: 7/15 = 46.7%).
- Keep the bottom Totals row; remove only the bottom Changes and % Change rows.
- Widths must sum to 100% (Option A: 8 + 72 + 12 + 4 + 4).
- Apply identically to BOTH matrices; mirror header and body widths exactly.
- Commitment: new "# Changes" column uses fmt() ($MM), diagonal /1M for units.
- Keep colLabel labels, cell colors, and all other logic unchanged.
- Do NOT touch pageSetup.ts, page size, margins, footer, or backend.
- Only edit this one file. Show: removed bottom rows, new per-row columns 
  (header + body + Totals), width redistribution, and the page break.
