Single-file edit: frontend/src/app/load-samples/page.tsx

The edit-mode overflow is reduced but not fully gone — trim a bit more. 
Continue reducing only the call-site width classes in this page (do NOT touch 
the shared SearchableSelect or DateInputWithCalendar components).

Apply these additional reductions:

1. TARGET (BU) wrapper: w-32 -> w-28 (128px -> 112px)
2. EIC NAME wrapper: w-32 -> w-28 (128px -> 112px)
3. TYPE dropdown: w-32 -> w-28 (128px -> 112px)
4. QUARTER: w-24 -> w-20 (96px -> 80px) — it's a short string like "2025-Q3", 
   fits in w-20.
5. Keep the date inputs at w-[6rem] (already reduced) and keep min-w-0 on the 
   EIC/Target wrappers.

CONSTRAINTS:
- Only edit page.tsx call-site classes. Do NOT modify SearchableSelect.tsx or 
  DateInputWithCalendar.tsx (shared — other pages must not break).
- Only change: Target wrapper w-32->w-28, EIC wrapper w-32->w-28, Type 
  w-32->w-28, Quarter w-24->w-20.
- Do NOT change read-only rendering, Type options/placeholder (#176), 
  DataTable, or globals.css.
- After editing, show the changed lines.
