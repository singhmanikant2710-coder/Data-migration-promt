Bug 195 fix. SINGLE FILE, minimal change. Show diff before applying.

FILE: frontend/src/app/reports/page.tsx
The backend already returns samples ordered by Sample_id DESC, but the frontend re-sorts by sample_start_date (lines ~280-290), overriding it. Geoff wants the Sample Name Selection dropdown ordered by Sample_id DESCENDING.

Change the .sort() comparator (lines ~283-288) so it sorts ONLY by id descending:
   .sort((a, b) => b.id - a.id)
Drop the getStartEpoch primary comparison. If getStartEpoch (lines ~259-271) becomes unused after this, remove it too (or leave it if referenced elsewhere — check first; only remove if truly unused).

Do NOT change the backend, the options builder (sampleOptions useMemo), the SearchableSelect component, or the data-loading/dedup logic. Only the sort comparator.
List every line changed.
Commit: "Fix Bug 195: order Reports sample dropdown by Sample_id DESC (remove start-date re-sort)".
