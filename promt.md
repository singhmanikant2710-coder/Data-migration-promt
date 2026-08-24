SINGLE-FILE, MINIMAL, BOUNDED EDIT. Only edit frontend/src/app/blackbook/edit/page.tsx. Do not touch any other file. Do not refactor. Show me the diff before applying. Do not run the terminal.

GOAL: Fix "TTM/YTD values not pulling for a newly entered month" (BCAT defect #35, also #29/#30/#31). Root cause: buildMonthSummaryColumns is called with plain `enrichedSeries` and plain `rolling24`, which do NOT include the user's in-flight edits for the newly entered month. The file already builds `seriesWithEdits` and `rolling24WithEdits` (edits merged + hydrated to canonical cur* keys), and every other consumer (MonthSummaryTable, DetailGrid, DetailHeaderTiles, industry mappers) already uses those. Only the column builder was left on the plain arrays.

EXACT CHANGE — apply to BOTH call sites of buildMonthSummaryColumns in this file:

1) The main `monthSummaryColumns` useMemo (currently):
   buildMonthSummaryColumns({
     series: enrichedSeries,
     rolling24,
     ...
   })
   with dep array [enrichedSeries, rolling24, industry, name, customLabels]

   CHANGE TO:
   - series: enrichedSeries        -> series: seriesWithEdits
   - rolling24,                    -> rolling24: rolling24WithEdits
   - dep array [enrichedSeries, rolling24, ...] -> [seriesWithEdits, rolling24WithEdits, industry, name, customLabels]

2) The seeding call site (the `cols = buildMonthSummaryColumns({ series: enrichedSeries, rolling24, ... })` block used to discover editable fields):
   - Apply the SAME two swaps: series -> seriesWithEdits, rolling24 -> rolling24WithEdits.
   - If this call site is inside a function/effect that does NOT already close over seriesWithEdits/rolling24WithEdits in scope, tell me instead of guessing — do not move code around.

CRITICAL SAFETY CHECKS before applying (report findings if any fail, do NOT force the edit):
a) Confirm `seriesWithEdits` and `rolling24WithEdits` are DECLARED (via useMemo) BEFORE both buildMonthSummaryColumns call sites in source order. If either call site is above their declaration, a TDZ/ReferenceError will occur — in that case STOP and report; do not reorder.
b) Confirm `seriesWithEdits` / `rolling24WithEdits` do NOT depend (directly or transitively through their own useMemo deps) on `monthSummaryColumns` — i.e. no circular dependency. Report the dep arrays of both WithEdits memos.
c) Do NOT change the definitions of seriesWithEdits / rolling24WithEdits, buildMonthSummaryColumns, or any calc function. Only swap the two arguments + the two dep arrays at the call sites.

After the change: show me the unified diff (only the changed lines + context). Do not build, do not run anything. I will review the diff before you apply.
