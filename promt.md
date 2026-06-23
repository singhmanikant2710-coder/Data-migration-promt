File to modify: frontend/src/app/review-history/page.tsx

STEP 1 — First READ the entire page.tsx file. Understand the table rendering 
(rows, columns), the eCIF# cell, the Borrower Name / Linesheet cell with its 
document/pdf icon, and the pagination component. Do NOT edit until you understand 
the current structure.

STEP 2 — Apply these 4 UI changes to ONLY this file:

CHANGE 1 — Remove the radio/circle in each row:
There is a radio button (circle "○") rendered at the start of every table row. 
Remove that radio input/element and its containing cell entirely, so rows no 
longer show a selection circle. Keep all other columns intact and keep column 
alignment correct after removal.

CHANGE 2 — eCIF# column placeholder:
Currently the eCIF# cell renders "-" when there is no value. Change it so that:
- If eCIF value exists (not null, not empty string), show the actual value.
- If it is null or empty, render an empty cell (no "-", no dash). 
Do not change any other column's placeholder behavior.

CHANGE 3 — Align the document/pdf icon in the Borrower Name / Linesheet column:
Currently the pdf/document icon appears right after the borrower text, so its 
position shifts depending on borrower name length (looks misaligned, "jumping" 
left/right per row). Fix it so the icon sits in a consistent fixed position 
within the column for every row — e.g. borrower name on the left and the icon 
aligned in a fixed spot (use a flex layout with justify-between, or a fixed-width 
icon container), so all icons line up vertically down the column regardless of 
name length. Use only existing Tailwind utility classes already used in the project.

CHANGE 4 — Premium pagination (max 10 page buttons + Previous/Next):
Currently all page numbers (1..30+) render at once. Change the pagination to a 
premium style:
- Show a maximum of 10 numbered page buttons at a time (a sliding window around 
  the current page).
- Add "Previous" and "Next" buttons on either side.
- Previous is disabled on page 1; Next is disabled on the last page.
- Keep the existing "Showing 1-10 of 4974 results" text.
- Keep the existing page-change handler / state logic; only change how the page 
  buttons are rendered (the window + prev/next), do not change the data fetching 
  or page size.

CONSTRAINTS:
- Modify ONLY this one file: frontend/src/app/review-history/page.tsx
- Do NOT touch any other file. If another file seems to need changes, STOP and 
  ask me first.
- Do NOT add any new npm package.
- Do NOT change backend calls, API params, or data logic — these are pure UI changes.
- Keep all existing functionality (sample dropdown filter, borrower search, 
  sorting) working.

After editing, give me a short summary of exactly what changed in each of the 
4 areas.
