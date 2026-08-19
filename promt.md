Single-file edit: frontend/src/app/load-samples/page.tsx

Fix: The Start/End date inputs in the Select Sample grid (edit mode) are too narrow (w-[6rem]) with iconInside, cutting off "YYYY". Widen to match other working date inputs in this file (w-[12rem]).

BEFORE (Start column, editing):
<DateInputWithCalendar
  value={parentDraft.sample_start_date ?? (r.sample_start_date ?? null)}
  onChange={...}
  className="w-[6rem] border rounded px-2 py-1 text-xs"
  iconInside
/>

AFTER:
<DateInputWithCalendar
  value={parentDraft.sample_start_date ?? (r.sample_start_date ?? null)}
  onChange={...}
  className="w-[10rem] border rounded px-2 py-1 text-xs"
  iconInside
/>

Apply the SAME width change to the End column's DateInputWithCalendar (also currently w-[6rem]).

CONSTRAINTS:
- ONLY change className from "w-[6rem]..." to "w-[10rem]..." on these two date inputs (Start and End, in the parentColumns edit render).
- Do NOT change the onChange logic, iconInside, or any other prop.
- Do NOT touch other DateInputWithCalendar usages (filters, modals, child grid) — they already work.
- Show the diff.


Single-file edit: frontend/src/app/load-samples/page.tsx

Fix: The Select Sample grid wrapper already has overflow-x-auto, but the table has no min-width, so it shrinks instead of triggering scroll when content needs more space (e.g. after the date input widening in Fix A, or when columns vary). Add a min-width wrapper so the table keeps its natural width and the parent scrolls horizontally when needed.

Locate the Select Sample grid's scroll wrapper:
BEFORE:
<div className="overflow-x-auto">
  <DataTable<Sample> ... />
</div>

AFTER:
<div className="overflow-x-auto">
  <div className="min-w-[900px]">
    <DataTable<Sample> ... />
  </div>
</div>

CONSTRAINTS:
- ONLY wrap the Select Sample grid's DataTable in this min-width div. Do NOT change DataTable props.
- Do NOT apply this to the Load Samples (child) grid's DataTable — only the Select Sample (parent) one.
- Do NOT change overflow-x-auto or any other styling on the outer wrapper.
- Show the diff, and confirm which DataTable (parent Select Sample vs child Load Samples) got the wrapper.
