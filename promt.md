File to modify: frontend/src/app/review-history/page.tsx

The customerName column's long text (e.g. "HOWARD MIDSTREAM ENERGY PARTNERS") 
still overlaps the pdf icon. Two previous attempts (column className width, 
and table-fixed) did not fix it — table-fixed broke other columns and was 
reverted.

FIX THIS TIME: In the render function for the customerName column, find the 
outer container div (className="flex items-center justify-between gap-2 
min-w-0 w-full"). Add an inline style directly to it:
style={{ maxWidth: '280px' }}

Then find the inner wrapper div around the name button (the one with 
className="flex-1 min-w-0"). Add an inline style to it too:
style={{ overflow: 'hidden' }}

Do not remove or change any existing className. Only ADD these two style 
attributes to these two specific divs. Do not touch table-layout, do not add 
table-fixed, do not touch any other column, do not touch DataTable.tsx.

Modify ONLY this file: frontend/src/app/review-history/page.tsx. After editing, 
paste back the exact JSX block you changed so I can verify before testing.
