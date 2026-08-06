READ-ONLY. Diagnostics only. Do not change anything.

File: frontend/src/components/pdf/CrmPdGradeMigrationPDF.tsx

MAJOR STRUCTURAL CHANGE: The client wants "# of Changes" and "% Change" moved 
from the Y-axis (per CAS PD column, current) to the X-axis (per Bank PD ROW). 

CURRENT: Changes/% Change are rendered as two ROWS at the bottom (per CAS PD 
column): changes[i] = colTotals[i] - diagonal[i], pctChange[i] = changes[i]/
colTotals[i].

NEW (client-confirmed): For EACH Bank PD ROW:
- "# of Changes" for that row = number of accounts in that row that changed 
  (off-diagonal in that row, i.e. cells where toPd != fromPd for that fromPd row)
- "% Change" for that row = # Changes / total accounts in that row (rowTotal)
- Example: Bank PD-13 row has 15 accounts total, 7 changed → 7/15 = 46.7%
- These become new COLUMNS on the right (alongside the existing row Totals 
  column), NOT bottom rows.

Show me (no edits, for BOTH MatrixCount and MatrixCommitment):
1. The current row-building logic: how each row's cells, rowTotal (sum), and 
   the diagonal are computed. Show where `sum` (rowTotal) is calculated and 
   whether the per-row diagonal is accessible (byFromTo.get(`${fromPd}|${fromPd}`)).
2. The current header structure (2-row: "CAS PD" grouping + 1-16 + "Totals"). 
   To add "# Changes" and "% Change" as columns after "Totals", I need to 
   know the column width layout (currently 8% + 16×5% + 12% = 100%).
3. The current bottom Totals/Changes/%Change rows (grouped in <View wrap=false>). 
   The Changes and % Change ROWS will be REMOVED (replaced by per-row columns); 
   the Totals row stays. Confirm their structure.
4. For each data row: can I compute per-row changes = rowTotal − rowDiagonal 
   (where rowDiagonal = byFromTo.get(`${fromPd}|${fromPd}`) for count, or /1M 
   for commitment), and per-row % = changes/rowTotal? Confirm rowTotal and the 
   diagonal are available per row.
5. Column width impact: adding 2 columns (# Changes, % Change) means 
   redistributing widths. Currently BANK PD 8% + 16 cols × 5% (80%) + Totals 
   12%. Show if we can shrink to fit 2 more columns (e.g. reduce PD cols or 
   Totals width).

Do not edit anything. Confirm per-row changes/diagonal availability and the 
column layout so I can add "# Changes" and "% Change" as right-side columns 
and remove the bottom Changes/% Change rows. Findings only.
