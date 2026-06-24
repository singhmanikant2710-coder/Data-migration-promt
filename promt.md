File to modify: frontend/src/app/review-history/page.tsx

ROOT CAUSE CONFIRMED: DataTable renders each <td> with "whitespace-nowrap", 
which forces the cell to stretch to fit long text and prevents truncation. 
Since cellClassName is applied directly to the <td>, we can override it from 
here for just the customerName column.

FIX — on the customerName column definition, set its cellClassName to override 
the nowrap and cap the width:
cellClassName="!whitespace-normal max-w-[280px] w-[280px] overflow-hidden align-middle"

(The "!" prefix on whitespace-normal forces it to win over DataTable's 
whitespace-nowrap. Keep w-[280px] and max-w-[280px] so the cell is capped.)

Keep the column's className (header) as it currently is. Do NOT change the 
render JSX — leave the existing outer div (with maxWidth 280 style + 
flex/justify-between/w-full) and inner div (overflow hidden + flex-1 min-w-0 
with truncate) exactly as they are; they will now work because the <td> itself 
is finally constrained.

Modify ONLY this file (page.tsx), only the customerName column's cellClassName. 
Do NOT touch DataTable.tsx or any other column. After editing, paste back the 
customerName column definition (the object with key/className/cellClassName/
render) so I can verify.
