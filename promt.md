Single-file edit: frontend/src/app/load-samples/page.tsx

Fix: The two card grids use `overflow-x-auto` on a div whose inner child has `min-w-max`. Because the scroll div lacks `w-full` and the inner uses unbounded `min-w-max`, the content stretches the whole card beyond the viewport, cutting off the right side of the page (Search button, pagination). Constrain the scroll container to full width and give the inner a bounded min-width so the table scrolls INSIDE the card instead of overflowing the page.

=== Select Sample card ===
Line 2127 - add w-full to the scroll container:
BEFORE:
<div className="mt-3 border border-slate-200 rounded-lg shadow-sm overflow-x-auto">
AFTER:
<div className="mt-3 border border-slate-200 rounded-lg shadow-sm overflow-x-auto w-full">

Line 2128 - replace min-w-max with a bounded min-width:
BEFORE:
<div className="min-w-max">
AFTER:
<div className="min-w-[1000px]">

=== Load Samples card ===
Line 2322 - add w-full to the scroll container:
BEFORE:
<div className="mt-3 border border-slate-200 rounded-lg shadow-sm overflow-x-auto">
AFTER:
<div className="mt-3 border border-slate-200 rounded-lg shadow-sm overflow-x-auto w-full">

Line 2323 - replace min-w-max with a bounded min-width:
BEFORE:
<div className="min-w-max">
AFTER:
<div className="min-w-[1000px]">

CONSTRAINTS (UAT is running — must not break anything):
- ONLY change these 4 lines (2127, 2128, 2322, 2323): add `w-full` to the two scroll containers, and change `min-w-max` -> `min-w-[1000px]` on the two inner divs.
- Do NOT change any DataTable props, columns, render functions, dropdowns, date pickers, pagination, filters, state, or handlers.
- Do NOT change any other div, className, or file.
- Do NOT touch the outer page container (line 2034) or the filter rows.
- Show the FULL diff of exactly these 4 line changes.
