Single-file edit: frontend/src/app/load-samples/page.tsx

Fix edit-mode horizontal overflow by REDUCING column widths so the edit row 
fits within the container (max-w-7xl ≈ 1280px). The previous fix allowed 
overflow (removed clipping) but didn't reduce width — the internal horizontal 
scrollbar and cut-off Target/Search columns remain. Root cause: sum of fixed 
edit-control widths (~1440px) exceeds the container. Trim widths. Read-only 
view must stay exactly as-is.

Current edit-control widths (verify each line before editing):
- Start date: w-[9rem] (144px)
- End date: w-[9rem] (144px)
- Quarter: w-24 (96px)
- EIC: wrapper w-40 + control w-32
- Type: w-32 (128px)
- Target (BU): wrapper w-64 (256px) + control w-64 (256px)  ← biggest culprit
- (DataTable selection col w-10, actions col w-40 — do not touch)

Make these width reductions (edit-mode controls only):

1. TARGET (BU) — biggest saving. Change the Target edit wrapper from w-64 to 
   w-40, and the SearchableSelect control from w-64 to w-32:
       wrapper: w-64 -> w-40
       control: w-64 -> w-32
   (Trims ~192px.)

2. START and END date inputs — change both DateInputWithCalendar from w-[9rem] 
   to w-[7rem] (144px -> 112px each; MM/DD/YYYY fits fine):
       w-[9rem] -> w-[7rem]  (on both Start and End)
   (Trims ~64px total.)

After these, recompute the approximate width budget and SHOW it:
   selection 40 + Start 112 + End 112 + Quarter 96 + EIC wrapper 160 + 
   Type 128 + Target wrapper 160 + actions 160 + paddings (~24 × columns) 
   + Sample Name/Closed text.
   Confirm the total is now <= ~1280px. 

3. IF the recomputed total is STILL over ~1280px, also reduce Type from w-32 
   to w-28 (128px -> 112px), and re-show the budget.

CONSTRAINTS:
- Only edit page.tsx, only these width classes.
- Do NOT change read-only cell rendering, Type option labels/values (#176 
  fix), the "Select Type" placeholder, DataTable, or globals.css.
- Verify each relevant line read-only before editing it.
- After editing, show the recomputed width budget proving total <= ~1280px 
  (trim further if not).
- Show the changed lines (Target wrapper+control, Start, End, and Type if 
  needed).
