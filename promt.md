Correction: I meant the COMPENSATE variant (your "Option C"), not reflow.
In CrmSummaryPDF.tsx also change the header's marginBottom from 20
(spacing.lg + 4) to 12, so the bar height matches Management and the
total space above the content is unchanged (your 0/26 pagination diffs).
Only that one line plus a short comment. Re-run the same page-count
check (5 shapes + 26-step sweep) and confirm 0 diffs vs before this task.
Delete any temp harness afterwards.

Report ADDED/REMOVED (file:line), the page-count table, and the build
result. Do not commit.
