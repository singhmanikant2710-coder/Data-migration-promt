Bug 214 — CRM Summary Table report, 3 issues. READ-ONLY, no edits. One pass, answer everything, STOP.

CONTEXT: 
1. Exposure column under BORROWER FINDING TOTALS double-counts borrowers with 2+ findings for that CRM Component (Count total is correct, Exposure is not — e.g. The Collier at Clift Farm LLC, Sample ID 353).
2. Percent totals under UNSATISFACTORY TRANSACTIONS and BORROWER FINDING TOTALS (all 4 percentage calculations) need 2 decimal places (e.g. 2/58 = 3.54%) — currently not showing 2 decimals despite a prior attempt ("Not Addressed").
3. NEW: add a "Commitment" column to the detail tables, summing Customer Commitment per row.

Investigate:
1. Find the CRM Summary Table PDF component (likely CrmSummaryTablePDF.tsx) and its backend repository (SqlCrmSummaryTableReportRepository.cs). 
2. EXPOSURE DOUBLE-COUNT: find the exact SQL/aggregation that computes Exposure under "BORROWER FINDING TOTALS". Is it double-counting because a borrower with multiple findings for the same CRM Component gets their exposure summed once per finding row instead of once per borrower? Paste the exact query/aggregation logic. Also confirm the Count aggregation (which is correct) so we can see why one is right and the other isn't.
3. PERCENT FORMATTING: find where the 4 percentage columns (under UNSATISFACTORY TRANSACTIONS and BORROWER FINDING TOTALS) are formatted/rendered. What format string/logic is used today? Why might a prior fix attempt not have worked (e.g. wrong file, wrong component, a second render path)?
4. COMMITMENT COLUMN: find the "detail tables" in this report (the per-row/per-borrower tables, not the totals tables). Is Commitment data already available in the backend response (reuse from other reports' SUM(Accounts.Commitment) pattern), or does it need to be added to the query/DTO? List each detail table that would need this new column.

Report file paths + line numbers for all three issues, the double-count root cause, why percent formatting didn't stick, and whether Commitment data needs a backend addition. Do NOT propose or write a fix yet.
