File to modify: frontend/src/app/review-history/page.tsx

The customerName truncation now works (no more overlap, icons aligned). 
Just one polish: long names cut off abruptly without showing "..." ellipsis. 
Ensure the ellipsis shows.

In the inner wrapper div around the name button (currently 
style={{ overflow: 'hidden' }}, className="flex-1 min-w-0"), and on the name 
button itself, make sure the button has the "truncate" class (which adds 
overflow-hidden + text-ellipsis + whitespace-nowrap). If the button already 
has "truncate", instead add "text-ellipsis whitespace-nowrap overflow-hidden" 
explicitly to the button's className so the cut-off shows "..." properly.

Modify ONLY this file, only the customerName render JSX. Do not change the 
cellClassName width fix that's now working. Do not touch any other column.
