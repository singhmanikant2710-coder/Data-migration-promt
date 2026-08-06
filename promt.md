Single-file edit: frontend/src/components/pdf/CrmPdGradeMigrationPDF.tsx

Change column widths only. Apply to BOTH MatrixCount and MatrixCommitment 
identically. Do NOT change text, colors, alignment, or logic — only flexBasis 
percentages.

Client wants: reduce "Totals" column to match "# Changes" (8%), and give the 
freed 4% to the CAS PD columns (16 columns go from 4% to 4.25%).

NEW WIDTHS (must be applied to EVERY row that has these columns — the CAS PD 
grouping header row, the column-labels header row, all data rows, AND the 
bottom Totals row):
  - BANK PD (first cell): 8%  (unchanged)
  - Each of the 16 CAS PD columns: 4.25%  (was 4%)  → 16 × 4.25 = 68%
  - Totals column: 8%  (was 12%)
  - # Changes column: 8%  (unchanged)
  - % Change column: 8%  (unchanged)
  Total: 8 + 68 + 8 + 8 + 8 = 100%

SPECIAL — the CAS PD grouping header row (currently 8% | 64% | 28%):
  - Blank cell over BANK PD: 8%  (unchanged)
  - "CAS PD" spanning cell: change 64% → 68% (to span the 16 columns at 4.25%)
  - Trailing blank cell (over Totals + # Changes + % Change): change 28% → 24% 
    (8+8+8 = 24%)
  Total: 8 + 68 + 24 = 100%

Locations to update in BOTH matrices:
1. Grouping header row: 64% → 68%, 28% → 24%.
2. Column-labels header row: 16 CAS PD headers 4% → 4.25%; "Bank PD Totals" 
   header 12% → 8%.
3. Data rows: 16 CAS PD cells 4% → 4.25%; row-sum (Totals) cell 12% → 8%.
4. Bottom Totals row: 16 colTotal cells 4% → 4.25%; grandTotal cell 12% → 8%.

CONSTRAINTS:
- ONLY change flexBasis percentage values as specified. Keep flexGrow:0, 
  flexShrink:0, and all other style props unchanged.
- Apply identically to BOTH matrices.
- Every row's widths must still sum to 100%.
- Do NOT change text, alignment, colors, fmt, data, or the footnote.
- Do NOT touch pageSetup.
- Only edit this one file. Show the updated widths for the grouping header, 
  labels header, a data row, and the Totals row, in both matrices.
