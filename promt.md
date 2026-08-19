READ-ONLY. Diagnostics only. Do NOT change anything.

Files: 
- frontend/src/app/review-queue/page.tsx (the Review Queue page)
- frontend/src/app/load-samples/page.tsx (for comparison)

The Review Queue page has the EXACT desired behaviour: the card fits the screen, and the wide table inside it scrolls horizontally WITHIN the card (with a scrollbar at the bottom of the grid). I want to apply the same pattern to Load Samples.

Show me ONLY (no edits):
1. In review-queue/page.tsx: the grid/table wrapper structure — the div(s) around its DataTable. Show the EXACT classNames: the outer card div, the overflow-x-auto wrapper, and any min-w / w-full on the inner div. I want to see how Review Queue achieves "card fits screen + inner table scrolls".
2. In load-samples/page.tsx: the SAME wrapper structure around the Select Sample DataTable (the overflow-x-auto + min-w-[1200px] divs), so I can compare what's different.
3. Specifically: does Review Queue use overflow-x-auto WITHOUT a large fixed min-w (letting the table's natural column widths trigger scroll), while Load Samples forces min-w-[1200px]? Compare the two approaches.

Read once. Findings only. No edits. I want to copy Review Queue's exact scroll pattern to Load Samples.
