Frontend only. Single file: frontend/src/app/review-status/page.tsx
Do NOT modify the backend. Do not plan. Just apply.

UAT #163: The Review Status grid drops reviews that the backend correctly returns, so its per-bucket counts differ from Review Queue (which is correct). Root cause: the client-side `byDate` filter inside the filteredRows useMemo removes rows whose milestone date falls outside the sample's auto-bound start/end window. Review Queue applies no such per-row date filter. The backend already returns the correct set of reviews per bucket, and the tile counts (from the backend) are already correct — only the grid is under-filtering.

In the filteredRows useMemo:
1) DELETE the byDate function entirely (the one that calls parseMDY / parseInput and compares against start/end).
2) DELETE the parseMDY and parseInput helper functions if they are only used by byDate.
3) Change the final return so rows are filtered by the text filter ONLY:
     return rows.filter((r) => byText(r));
4) Remove startDate and endDate from this useMemo's dependency array (since they're no longer used inside it). Keep every other dependency.

Do NOT change: the switch on selectedBucket that picks the dataset, the byText function, the tile counts, pagination (pagedRows), or the startDate/endDate state and their date input fields (they stay visible and are still passed to the API). Only stop using them to filter grid rows on the client.

Run read-only TypeScript diagnostics on this file only.



Read-only. No edits. Just report. In frontend/src/app/review-status/page.tsx:
1) Search the entire file for every usage of: byDate, parseMDY, parseInput. List each line where they appear. Are they used ONLY inside the filteredRows useMemo, or anywhere else too?
2) Search for every usage of startDate and endDate in the file. List each — are they used outside filteredRows (e.g. in the date input fields, or passed to the API load function)?
Output the line references only. Change nothing.
