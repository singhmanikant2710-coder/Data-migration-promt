File to modify: frontend/src/app/review-history/page.tsx

In the "customerName" column definition (inside the columns array, the one 
whose render function returns the borrower name + pdf icon), add a maxWidth 
(or equivalent width constraint) to the column definition itself — NOT inside 
the render JSX, but on the column config object (same level as "key" and 
"render" for this column).

Set it to a reasonable fixed cap, e.g.:
  maxWidth: "320px" 
(use whatever property name the DataTable component's column type already 
supports — check the column type/interface for DataTable first; it may be 
"width", "maxWidth", or a className-based option. Use whichever one DataTable 
actually respects).

After setting this constraint, the existing inner JSX (the flex container with 
w-full, the flex-1 min-w-0 wrapper around the name button with truncate, and 
the shrink-0 icon wrapper) should now correctly truncate long names with "..." 
and keep the icon aligned, since the cell will no longer stretch to fit the 
full text.

Do not change any other column's width. Do not touch the render JSX further — 
only add the width constraint to the column definition. Modify ONLY this file 
(page.tsx). If DataTable's column type doesn't support a width/maxWidth prop, 
STOP and tell me what options the column type interface actually offers, 
rather than guessing.
