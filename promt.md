Single-file edit: frontend/src/app/load-samples/page.tsx

Fix: Make the Load Samples grids behave EXACTLY like the Review Queue page (card fits the screen, inner table scrolls horizontally within the card). Review Queue does NOT use a fixed min-w; instead it uses overflow-x-auto + a full-width DataTable, letting natural column widths trigger scroll. Remove the forced min-w-[...] wrapper and let the table scroll naturally.

=== Select Sample grid ===
BEFORE:
<div className="mt-3 border border-slate-200 rounded-lg shadow-sm overflow-x-auto w-full">
  <div className="min-w-[1200px]">
    <DataTable<Sample> ... />
  </div>
</div>

AFTER (remove the min-w wrapper, add w-full to DataTable — Review Queue pattern):
<div className="mt-3 border border-slate-200 rounded-lg shadow-sm overflow-x-auto w-full">
  <DataTable<Sample> ... className="w-full" />
</div>

(If the DataTable already has a className, append " w-full" to it instead of overwriting. Do NOT change any other DataTable prop.)

=== Load Samples (child) grid ===
Apply the SAME change — remove its min-w-[1000px] inner wrapper and add w-full to that DataTable, so it also matches Review Queue.
BEFORE:
<div className="... overflow-x-auto ...">
  <div className="min-w-[1000px]">
    <DataTable ... />
  </div>
</div>
AFTER:
<div className="... overflow-x-auto ...">
  <DataTable ... className="w-full" />
</div>

CONSTRAINTS:
- Remove ONLY the min-w-[...] inner wrapper divs (both grids) and add w-full to each DataTable's className.
- Keep the outer overflow-x-auto wrapper divs as they are.
- Do NOT change any DataTable props, columns, the isCreating dropdown widths (keep w-44), or any logic/handlers.
- Do NOT touch AppShell or shared components.
- Show the FULL diff.
