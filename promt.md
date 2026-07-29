Read-only. No edits. No plan. Just report with file paths + exact code. Do NOT modify anyone's work (including Jothi's).

Context (UAT #163): Review Status counts differ from Review Queue by one in three buckets for Sample 354 (In Progress 15 vs 14, Distributed 1 vs 0, Finalized 80 vs 79). Review 20120 (finalized_date populated) appears in Review Queue and IS returned by the backend Review Status Finalized query, yet it does not show in the Review Status Finalized bucket in the UI. So the drop is happening on the frontend, not the backend.

Report:
1) In frontend/src/app/review-status/page.tsx: after the six datasets (finalizedRows, distributedRows, approvedRows, completedRows, inProgressRows, unopenedRows) are set from the API response, is there any further client-side filtering, de-duplication, precedence resolution, or "resolve to single status" logic applied before display or before computing counts? Paste it.
2) How are the tile counts computed on the frontend — directly from the backend statusCounts, or recomputed from the row arrays? Paste that code.
3) How is the grid populated for the selected bucket — does it just show the matching dataset, or does it merge/filter across datasets? Paste the filteredRows / grid-binding logic.
4) Is there any dedup by reviewId, ecif, or borrower name that could remove a review that legitimately belongs in a bucket? Paste it.
5) Trace review 20120 specifically: it has finalized_date set, so it's in finalizedRows. What in the frontend would cause it to be dropped from the Finalized bucket display or count?
6) State exactly what must change and in how many files so the Review Status bucket contents and counts exactly match what the backend returns (and thus match Review Queue).

Use LIVE DB, ignore columns.csv. Output findings only. Change nothing.
