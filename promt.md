READ-ONLY. Diagnostics only. Do not change anything.

File: frontend/src/app/load-samples/page.tsx
Also check: frontend/src/components/table/DataTable.tsx and the SearchableSelect 
+ DateInputWithCalendar components.

Edit-mode row STILL overflows horizontally despite reducing width classes. The 
className widths aren't producing the expected total — something is enforcing a 
larger minimum width. Find the REAL constraint (no edits):

1. TABLE min-width: DataTable's table uses className "min-w-full". Does 
   "min-w-full" force the table to be at least 100% of its container, 
   preventing columns from shrinking? More importantly, is there any min-width 
   on the table that makes it grow to fit content (so reducing cell widths 
   doesn't help because other content sets the floor)? Show the table element's 
   width/min-width classes.

2. SearchableSelect internal width: The EIC and Target use SearchableSelect. 
   Show the SearchableSelect component's ROOT element — does it have its own 
   min-width, fixed width, or padding that IGNORES the className we pass (w-32/
   w-40)? The passed className might not reach the widest inner element (e.g. 
   the dropdown input or the selected-value display). Show SearchableSelect's 
   internal structure and any hardcoded min-w / width / px padding.

3. DateInputWithCalendar internal width: Same check — does it have an internal 
   min-width or fixed width that ignores w-[7rem]? Show its root element's 
   width constraints and the calendar icon/button width.

4. Column/cell floor: In DataTable, do td/th cells have whitespace-nowrap or a 
   min-width that prevents shrinking? Show the td/th classes. Does 
   "whitespace-pre-wrap break-words" allow wrapping (good) or is something 
   nowrap (forces width)?

5. What is the ACTUAL widest column in edit-mode? Given the components' real 
   min-widths (not just our className), which control is actually forcing the 
   overflow — is it SearchableSelect's internal min-width regardless of our 
   wrapper class?

Do NOT edit. Show: the table's min-width, SearchableSelect's real internal 
width/min-width (does it honor our className?), DateInput's internal width, td/
th nowrap/min-width, and which control truly sets the width floor. Findings 
only.
