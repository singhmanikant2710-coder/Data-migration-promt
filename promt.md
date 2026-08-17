Single-file edit: frontend/src/app/load-samples/page.tsx

The edit-mode overflow is caused by internal padding, NOT the width classes. 
Diagnostics found: DateInputWithCalendar adds pr-10 (2.5rem) internally so 
w-[7rem] actually renders ~10rem; SearchableSelect's passed w-32 is overridden 
by the parent wrapper's w-40 and adds px-3, so EIC/Target render wider than 
intended. Fix ONLY at the call-site in this page (do NOT modify the shared 
SearchableSelect or DateInputWithCalendar components — other pages use them).

Reduce the EFFECTIVE footprint by shrinking the wrapper widths (which actually 
control the rendered size) and the date widths further to compensate for the 
internal padding:

1. TARGET (BU): the wrapper div className="w-40" is what actually sizes it 
   (the inner control is w-full). Reduce the wrapper from w-40 to w-32:
       Target wrapper: w-40 -> w-32
   (The inner SearchableSelect stays w-full/w-32 — the wrapper now caps it at 
   128px instead of 160px.)

2. EIC NAME: same — reduce its wrapper div from w-40 to w-32:
       EIC wrapper: w-40 -> w-32

3. START and END date inputs: since pr-10 adds ~2.5rem internally, reduce the 
   width class to compensate. Change w-[7rem] to w-[6rem] on both:
       Start: w-[7rem] -> w-[6rem]
       End:   w-[7rem] -> w-[6rem]
   (The internal pr-10 still reserves the icon space; the visible input just 
   gets tighter but MM/DD/YYYY still fits since pr-10 area holds the icon.)

4. Add min-w-0 to the EIC and Target wrapper divs so they can shrink within the 
   table cell if still tight:
       wrapper className: "w-32" -> "w-32 min-w-0"
   (This lets flex/table shrink them rather than overflow.)

CONSTRAINTS:
- Only edit page.tsx call-site classes. Do NOT modify SearchableSelect.tsx or 
  DateInputWithCalendar.tsx (shared components — other pages/tabs must not 
  break).
- Only change: Target wrapper w-40->w-32, EIC wrapper w-40->w-32, both date 
  inputs w-[7rem]->w-[6rem], add min-w-0 to EIC+Target wrappers.
- Do NOT change read-only rendering, Type options/placeholder (#176), DataTable, 
  or globals.css.
- After editing, show the changed lines.
