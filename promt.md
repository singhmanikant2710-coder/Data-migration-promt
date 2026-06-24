File to modify: frontend/src/app/review-history/page.tsx

In the "customerName" column's render function, the borrower name + pdf icon 
cell currently looks like:

<div className="flex items-center justify-between gap-2 min-w-0">
  <button ... className="text-slate-800 font-medium hover:underline cursor-pointer bg-transparent border-0 p-0 text-left truncate">
    {r.customerName ?? "-"}
  </button>
  <div className="shrink-0 w-6 flex justify-end">
    <Button ...>...</Button>
  </div>
</div>

Make this single change only:
1. Add "w-full" to the outer container's className, so it becomes:
   "flex items-center justify-between gap-2 min-w-0 w-full"
2. Wrap the name <button> in a new div with className="flex-1 min-w-0", 
   keeping the button's existing className and truncate behavior unchanged.

Do not change anything else — not the icon button, not the SVG, not any other 
column or cell. Modify ONLY this file (page.tsx). Do NOT touch any other file. 
If this requires touching DataTable or any other component, STOP and ask me first.
