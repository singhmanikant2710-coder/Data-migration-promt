Bug 225 — CRM Summary PDF: Scorecard Assessment table rows split across page boundaries (same mechanism as Bug 216), causing columns to shift left on the following page. Confirmed by Geoff on pages 7&8, 13&14, 17&18. READ-ONLY, no edits. One pass, answer, STOP.

1. Find the CRM Summary PDF component (CrmSummaryPDF.tsx) and the Scorecard Assessment table's row rendering (the table with SCORECARD ID | DATE | BANK PD | BANK LGD | CAS PD | CAS LGD | SCORECARD TYPE | SCORECARD ASSESSMENT columns). File + line.
2. Does the data-row <View> have wrap={false}? Does the header row have it?
3. Confirm the Scorecard ID cell is the multi-line cell (from the Bug 191 hyphen-wrap fix) causing the row height variance that triggers the split, same as Bug 216's Customer Name cell.
4. Report the exact line(s) needing wrap={false}, mirroring the Bug 216 fix pattern (and NonCompliantCovenantsPDF/CroProductionSummaryPDF's correct usage).

Report file + line numbers. Do NOT fix yet.
