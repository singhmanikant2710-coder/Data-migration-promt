Find and open the shared DataTable component file (likely 
frontend/src/components/table/DataTable.tsx).

READ-ONLY. Do NOT edit anything. Report in plain text:

1. How does DataTable render each body cell (<td>)? Paste the exact <td> 
   JSX/element including its className.
2. Does the <td> (or the <table>, <tbody>, or <tr>) have any of these classes 
   or styles anywhere: "whitespace-nowrap", "white-space: nowrap", a fixed 
   "width", "max-width", or "overflow-x"? List exactly which element has which.
3. Does DataTable apply the column's cellClassName to the <td>, or to an inner 
   wrapper div inside the <td>? (i.e. when I pass cellClassName, where does it 
   actually land?)
4. Is there a global CSS file (globals.css or similar) that sets white-space 
   or table cell rules? If you can see one, report any td/table rules in it.

Report findings only. Do NOT modify any file.
