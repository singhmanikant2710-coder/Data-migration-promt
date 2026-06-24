URGENT — REVERT the last change.

File: frontend/src/app/review-history/page.tsx

The last edit added "table-fixed" to the className passed to <DataTable ...> 
on this page. This broke the entire table layout — columns are now overlapping 
text, the eCIF# column disappeared, and horizontal scroll stopped working.

Please REMOVE "table-fixed" from that className string, reverting it back to 
exactly what it was before this change (e.g. back to 
className="w-full [&_td]:px-4 [&_td]:py-3" without table-fixed).

Do NOT make any other changes. Modify ONLY this one file. After reverting, 
show me the className string as it now stands so I can confirm it matches 
the pre-change state.


next year daalna hai

File to modify: frontend/src/app/review-history/page.tsx

In the customerName column definition, the cellClassName currently has 
"w-[320px] max-w-[320px]" but the cell still overflows for long borrower names 
because the table uses table-layout: auto (table-fixed was tried and reverted 
as it broke other columns).

FIX: Instead of relying on className for width, add an inline style directly 
on the outer cell wrapper div inside the render function (the div with 
className="flex items-center justify-between gap-2 min-w-0 w-full"). Add:
style={{ maxWidth: 280 }}
to that same div, in addition to its existing className. Keep the className 
exactly as is. Also keep cellClassName/className on the column definition as 
they are (don't remove them).

This forces the flex container itself to a hard pixel cap regardless of the 
table's layout algorithm, so truncate inside it will work correctly without 
needing table-fixed.

Do not touch table-layout, do not add table-fixed anywhere, do not touch any 
other column. Modify ONLY the customerName render JSX's outer div by adding 
the style attribute. Modify ONLY this file.
