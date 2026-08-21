Apply the Account Information table fix, but ReviewPDF.tsx (Part 3, sections C
and D) is malformed and would break the build. Files 1 (InitialMemoPDF) and 2
(FinalMemoPDF) look correct — apply those.

For ReviewPDF.tsx, your diff has errors:
1. Duplicate <View style={styles.tableRow}> opening tags in the first data row,
   plus an orphan "})()}</Text>" fragment.
2. Invalid JSX/TS: "{r?.outstandingBalance as any : "-"}" and
   "{r?.committedExposure as any : "-"}" — this is a broken ternary (missing "?")
   and won't compile.
3. Duplicate Balance/Commitment <Text> cells (both the old 14%/15% and new 13%).

Re-do ReviewPDF.tsx cleanly. For its bespoke Account Information table only:
- Header + first row + subsequent rows + empty-state row.
- Account Number column: flexBasis 14% -> 21%, add styles.wrapAnywhere.
- Scorecard ID column: 25% -> 29%.
- Balance: 15% -> 13%. Commitment: 14% -> 13%.
- Narrow PD/LGD (tableCellHeaderNarrow/tableCellValueNarrow): flexBasis 8% -> 5%.
- Keep the existing ternaries intact (e.g. r?.outstandingBalance != null ?
  formatCurrency(...) : "-"). Do NOT alter the value expressions — only change
  flexBasis and add wrapAnywhere.
- EXACTLY one <Text> per cell, one <View> per row. No duplicates, no orphan
  fragments.

After editing, show me the full Account Information block from ReviewPDF.tsx
(header + first row + one subsequent row + empty-state) so I can confirm no
duplicates and no broken ternaries. Then run tsc. Auto-approve OFF.
