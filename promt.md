File: frontend/src/app/review-history/page.tsx
READ-ONLY DIAGNOSTIC. Do NOT edit anything. Only read and report back.

Two earlier fixes did not work. Investigate and report exactly:

1. RADIO CIRCLE: Each table row still shows a circle (radio button) at the far 
   left, before the "Sample / Review Name" column. Find the exact JSX/element 
   that renders this circle in each row. Report:
   - The exact line(s) and the element (is it an <input type="radio">, a styled 
     <div>, an icon, a CSS pseudo-element, or part of a shared component?)
   - Why a previous attempt to remove it may have failed (e.g. it's rendered in 
     a child component, or via a CSS class).

2. DOCUMENT/PDF ICON ALIGNMENT: In the "Borrower Name / Linesheet" column, the 
   pdf/document icon still sits immediately after the borrower text, so its 
   horizontal position changes with each borrower name length (not aligned in a 
   fixed column position). Report:
   - The exact JSX that renders the borrower name + icon together in the cell.
   - The current layout/classes used (is it inline? flex? what container?).
   - What change would make the icon sit at a fixed position so all icons align 
     vertically (e.g. flex justify-between on the cell, or a fixed-width name 
     container with the icon after).

Report findings only. Do NOT make any edits. After you report, I will tell you 
what to change.
