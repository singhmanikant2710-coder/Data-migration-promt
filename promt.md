Bug 214 Issue 3 fix — Geoff confirmed Option B: repeat full borrower Commitment on every finding detail row (simple; users won't sum the column, the summary table at top already has correct totals). Show all diffs, do NOT commit.

BACKEND:
1. SqlCrmSummaryTableReportRepository.cs — details query (lines ~289-314): the acc CTE exists elsewhere in this file (summary query, lines 74-78: SUM(COALESCE(a.[Commitment],0)) GROUP BY Review_id) but is not joined into the DETAILS query. Add a join to acc (or an equivalent per-review commitment CTE) in the details query so each detail row can include that review's total commitment.
2. CrmSummaryTableModels.cs — CrmSummaryTableDetailRow (lines ~47-64): add a Commitment (decimal) property.
3. Reader mapping (lines ~346-378): map the new commitment column into the DTO.

FRONTEND:
4. CrmSummaryTablePDF.tsx — details type (lines ~33-42): add commitment to the row shape.
5. cols5 stylesheet (lines ~280-286) → becomes cols6: add a COMMITMENT column. Rebalance the 5 widths (currently summing to 100%, DESCRIPTION at 41%) to fit 6 columns — keep DESCRIPTION as the largest since it carries rich HTML via htmlCell, but give COMMITMENT reasonable width for dollar amounts (e.g. right-aligned, similar width to other numeric columns in this report).
6. Render the Commitment value with formatCurrency, right-aligned, in the new column, for all 5 detail sections (they share one map/render — one change covers all 5).

Do NOT change the summary table (Issue 1/2 area) — those are already fixed. Do NOT add a subtotal or distinct-count logic — full repeat per row, no dedup, as Geoff confirmed.

Show diffs. Rebuild. Run tests if any. Do NOT commit.
