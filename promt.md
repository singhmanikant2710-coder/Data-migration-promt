READ-ONLY. No edits. Find one regression, quote exactly, stop. No loop.

REGRESSION: The "Month" column in the Blackbook edit Monthly Summary table used to show month keys (202607 etc.) but now shows "—" (blank) after our recent change. We recently changed buildMonthSummaryColumns to receive seriesWithEdits / rolling24WithEdits instead of enrichedSeries / rolling24. The Month column render is: render: (row) => row.monthKey || "—". So the rows now passed likely lack a top-level monthKey.

In frontend/src/app/blackbook/edit/page.tsx, quote the FULL definitions of the two useMemo blocks: seriesWithEdits and rolling24WithEdits. I specifically need to see how each row object is constructed when merging edits. Show whether the mapped row preserves the top-level monthKey field, or whether it only spreads/merges .values and drops monthKey.

Compare: quote how enrichedSeries rows are shaped (do they have top-level monthKey?) vs how seriesWithEdits rows are shaped after the merge.

OUTPUT:
- A) seriesWithEdits full useMemo, verbatim.
- B) rolling24WithEdits full useMemo, verbatim.
- C) State plainly: does the merged row object in seriesWithEdits/rolling24WithEdits include a top-level monthKey property? yes/no, with the exact line. If no, that's the regression — the merge builds { ...p.values, ...pending } (or similar) into a new object without carrying monthKey.
- No fix yet. Findings only.
