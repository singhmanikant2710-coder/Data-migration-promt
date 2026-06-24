File to modify: frontend/src/app/review-history/page.tsx

The customerName column was given className="w-[320px] max-w-[320px]" and 
cellClassName="w-[320px] max-w-[320px]" on the column definition, but the long 
borrower name "HOWARD MIDSTREAM ENERGY PARTNERS" still overflows and overlaps 
the pdf icon instead of truncating with "...". This is happening because the 
table currently uses the browser default table-layout: auto, where column 
width hints can be ignored if content is wider.

FIX: Add the Tailwind class "table-fixed" to the actual <table> element 
rendered by the DataTable component (or, if DataTable accepts a className/
tableClassName prop for the outer <table> tag, pass "table-fixed" through that 
prop from this page — do NOT hardcode it inside DataTable.tsx itself, find 
where this page already passes className="w-full [&_td]:px-4 [&_td]:py-3" to 
the DataTable component and add "table-fixed" to that same className string).

After this change, the existing w-[320px] max-w-[320px] constraint on the 
customerName column combined with table-fixed should force the cell to a 
fixed width, allowing truncate to correctly cut long names with "..." and 
keep the icon aligned.

Only add the "table-fixed" class to the existing className prop passed to 
<DataTable ...> from this page. Do not change anything else, do not touch 
DataTable.tsx. Modify ONLY frontend/src/app/review-history/page.tsx. If adding 
table-fixed requires touching DataTable.tsx itself, STOP and tell me exactly 
what would need to change there, without editing it.
