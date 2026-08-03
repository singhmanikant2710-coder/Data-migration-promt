Single-file edit: frontend/src/components/pdf/CroProductionSummaryPDF.tsx

Two fixes for the CRO Review Production report. Do NOT touch pageSetup.ts, 
page size, orientation, or margins.

=== FIX A: Month header overlap (Summary by Reviewer table) ===

In the "SUMMARY BY REVIEWER (MONTHLY COUNTS)" table, the month headers 
(JAN-2026, FEB-2026, ... DEC-2026) overlap because "JAN-2026" is too wide 
for the narrow month columns. Fix by making the month labels two-line 
(month on line 1, year on line 2) and reducing the month header font size.

1. In buildMonthlyMatrix, where labels are built:
       const labels = MONTH_ABBR.map((abbr) => (y ? `${abbr}-${y}` : abbr));
   change to:
       const labels = MONTH_ABBR.map((abbr) => (y ? `${abbr}\n${y}` : abbr));

2. In the month header <Text> cells (the ones mapping matrix.labels with 
   flexBasis: monthColWidth), add a smaller fontSize so they fit. Change:
       style={[styles.th, styles.tdCenter, { flexBasis: monthColWidth }]}
   to:
       style={[styles.th, styles.tdCenter, { flexBasis: monthColWidth, fontSize: 7 }]}

   Apply the smaller fontSize ONLY to the month header cells. Leave the 
   REVIEWER NAME and TOTALS header cells as-is.

=== FIX B: Fill the empty space after CAS PD in the reviewer detail table ===

After removing the "(STATUS NOT SELECTED)" column, the reviewer detail table 
columns (col.d1..col.d7) no longer fill the full row width, leaving empty 
space after CAS PD. Rebalance the column widths so they sum to 100% and fill 
the row. Widen COMMITMENT (col.d4) and OUTSTANDING (col.d5) to absorb the 
freed space (the old status column was ~10%).

Show me the current col definition (col.d1..col.d7 with their flexBasis 
percentages), then adjust so the total is 100%. Suggested approach: increase 
col.d4 (COMMITMENT) and col.d5 (OUTSTANDING) widths, and if needed col.d1 
(CUSTOMER NAME), so the 7 columns fill the full 100% width with no trailing 
gap after CAS PD (col.d7).

Requirements for Fix B:
- The 7 column widths (d1..d7) must sum to exactly 100%.
- Distribute the freed width primarily to COMMITMENT (d4) and OUTSTANDING 
  (d5) so the CAS PD column ends flush with the right edge (no empty space 
  after it).
- Apply the SAME widths consistently to the header row, all data rows, and 
  the reviewer totals row (so columns stay aligned).
- Keep styles.tdLast on the CAS PD column (d7) as the last column.

CONSTRAINTS:
- Only edit this one file. Do NOT change page size/orientation/margins or 
  any other table.
- For Fix A, only the monthly summary table's labels and month-header font 
  change.
- For Fix B, only the reviewer detail table's column widths change.
- Show: (1) the updated label line, (2) the updated month header cell style, 
  (3) the old vs new col.d1..d7 widths with confirmation they sum to 100%.
