Read-only. No edits. No plan. Just report with file paths + exact code. Do NOT modify anyone's work (including Jothi's).

Context (UAT #165): Add ascending/descending column sorting to the Review Status table, matching how Review Queue and Review History already do it (clicking a column header sorts the rows).

Report:
1) How does Review Queue implement column sorting? In frontend/src/app/review-queue/page.tsx, paste: the sort state (sortBy / sortDir or similar), the handleSort function, and how sorted rows are computed (the useMemo that sorts). Also show how the table component receives sortBy/sortDir/onSort.
2) What table component does Review Queue use (e.g. DataTable) and does it render clickable sortable headers? Paste the component usage showing the sortable props.
3) In Review Status (frontend/src/app/review-status/page.tsx), how is the table currently rendered? Is it a plain <table> with hardcoded <th> headers, or the same DataTable component? Paste the current header row and the rows binding (pagedRows).
4) What columns does the Review Status grid show, and what field does each bind to on the row object (reviewId, borrower, reviewer, exposure, bankPd, casPd, status, completed, manager)?
5) State exactly what must change and in how many files to add ascending/descending sorting to the Review Status table, reusing the Review Queue pattern. Note whether Review Status can switch to the same DataTable component or needs sort state + a sorted useMemo added to its existing table.

Use LIVE DB, ignore columns.csv. Output findings only. Change nothing.
