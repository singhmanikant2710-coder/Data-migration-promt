Single-file edit: frontend/src/app/load-samples/page.tsx

The file currently builds fine. All #176 items are done except the scroll/fit pattern. Currently both grids wrap the DataTable in a <div className="min-w-[1000px]"> which forces the card wider than the screen. Remove the min-w wrapper and let the DataTable be w-full, matching the Review Queue page (which uses overflow-x-auto + DataTable w-full, NO min-w).

=== Select Sample grid (around line 2127) ===
BEFORE:
<div className="mt-3 border border-slate-200 rounded-lg shadow-sm overflow-x-auto w-full">
  <div className="min-w-[1000px]">
    ... (Showing bar, DataTable, Pagination) ...
  </div>
</div>

AFTER (remove the min-w-[1000px] wrapper div — unwrap its children up one level, keeping them all inside the outer overflow-x-auto div):
<div className="mt-3 border border-slate-200 rounded-lg shadow-sm overflow-x-auto w-full">
  ... (Showing bar, DataTable, Pagination) ...
</div>

=== Load Samples child grid (around line 2322) ===
BEFORE:
<div className="mt-3 border border-slate-200 rounded-lg shadow-sm overflow-x-auto w-full">
  <div className="min-w-[1000px]">
    ... (Showing bar, DataTable, Pagination) ...
  </div>
</div>

AFTER (remove the min-w-[1000px] wrapper the same way):
<div className="mt-3 border border-slate-200 rounded-lg shadow-sm overflow-x-auto w-full">
  ... (Showing bar, DataTable, Pagination) ...
</div>

CONSTRAINTS:
- ONLY remove the two <div className="min-w-[1000px]"> opening tags and their matching closing </div> tags — unwrap the children up one level. Do NOT delete or move any child (Showing bar, DataTable, Pagination, buttons).
- The DataTables already have className="w-full" (keep it). If a DataTable does NOT have w-full, add it.
- Do NOT change any DataTable props, columns, dropdown widths, or logic.
- Do NOT touch AppShell.
- CRITICAL: after removing each min-w div, ensure the JSX tags still match (each removed opening <div> must have exactly one matching </div> removed). The file must still build.
- Show the FULL final JSX of both grid blocks so I can verify tags are balanced and nothing was dropped.
