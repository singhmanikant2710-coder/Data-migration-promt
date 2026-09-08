Bug 216 — CRM Scorecard Results PDF: at a page break (top of page 2), one record's Customer Name cell is blank/missing and the subsequent columns shift left by one position. READ-ONLY, no edits. One pass, answer, STOP.

The report is a table (Customer Name (Review ID) | Scorecard ID | Eval Date | Status | 4 numeric columns). At the top of page 2, the first record shows an empty Customer Name column, and all following columns are shifted one cell to the left. Normal rows render fine.

Investigate:
1. Find the CRM Scorecard Results PDF component (ScorecardResultsPDF.tsx). How is the table/rows rendered? Are rows allowed to split across page boundaries? File + line.
2. This is a classic @react-pdf page-break row-splitting issue. Check: is there a `wrap={false}` on each data row `<View>` to keep it intact across pages? If rows can split, a row landing exactly on the page boundary can render partially (e.g. the Customer Name cell splits/disappears), causing the shift.
3. Look at how the Customer Name cell specifically renders — is it multi-line (customer name + review id on separate lines)? A multi-line first cell splitting across a page boundary is the likely cause of the "blank name + shifted columns" symptom.
4. Check the header row and column width definitions — are columns fixed-width? If fixed, a missing first-cell VALUE (but present cell) would shift text visually. Or is the row a flex layout where a missing cell collapses and shifts the rest?
5. Identify the exact cause: (a) rows split across pages (need wrap={false}), (b) the multi-line Customer Name cell breaks at the page boundary, or (c) something else. And identify the fix location.

Report file paths + line numbers + whether rows have wrap={false} + the root cause. Do NOT fix yet.
