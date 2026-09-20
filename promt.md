All confirmations in. Implement the full Checklist Questionnaire feature. Show diffs (backend + frontend), do NOT commit.

CONFIRMED FROM DATA: No stored Checklist_question begins with a digit (verified via LIKE '[0-9]%' check — 0 of 5 distinct questions). So generate the number prefix ourselves; no double-numbering risk.

1. NUMBERING: Sort the distinct Checklist_question set alphabetically (matching Checklist_question ORDER BY, same as the Review Form), assign sequence numbers 1, 2, 3... in that order, and prefix them to the question text ("1. Was the field exam completed timely?") in BOTH the summary table and the detail table. Numbers must be computed once over the full distinct question set for the applied filters, so the same question gets the same number in both tables and across all borrower blocks in the detail section.

2. CHANGE THE REPORT REPOSITORY'S ORDER BY from category-first (cl.[Review_id], cl.[Checklist_category], cl.[Checklist_question]) to question-text-only ascending, to match the Review Form's actual order and make numbering consistent. This changes the detail table's row order from category-grouped to alphabetical — expected and confirmed acceptable.

3. NEW SUMMARY TABLE (before the detail table): CHECKLIST QUESTION (numbered) | COUNT | EXPOSURE.
   - COUNT = number of reviews/borrowers with a "No" response (normalized via the existing NormalizeAnswer() logic — no raw string compare) to that question, within the applied filters.
   - EXPOSURE = sum of committed exposure (dbo.[02_CORE_04_Accounts].Commitment, house-standard SUM/GROUP BY Review_id pattern, mirrored from SqlNonCompliantCovenantsReportRepository.LoadReviewCommitAsync) across those "No"-response reviews, per question.
   - NO totals row.

4. DETAIL TABLE — filter + group: group by Borrower Name (Review ID); include ONLY questions with a "No" response for that borrower (normalized). A borrower with zero "No" responses is excluded entirely. Apply the numbered-question prefix here too.

5. SAMPLE ID REQUIRED: This report must require a Sample ID/Name filter to run — Geoff confirmed this prevents numbering from mixing questions across different sample templates. Add validation: if no Sample ID is selected, block report generation with a clear message (e.g. "Please select a Sample to run this report") rather than silently producing a misleading result. Check the existing pattern for how other reports enforce a required filter, if any exists, and mirror it; otherwise implement the minimal validation at the point where filters are gathered before calling execute.

Show the backend diff (repository ORDER BY change, new summary-table query with the commitment join, NormalizeAnswer reuse) and frontend diff (numbering logic, summary table rendering, detail table grouping/filtering, Sample-ID-required validation) together. Rebuild, run any existing tests. Do NOT commit.
