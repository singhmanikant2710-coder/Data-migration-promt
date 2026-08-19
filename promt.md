READ-ONLY. Diagnostics only. Do NOT change ANYTHING. Do NOT edit. UAT is in progress — I only need to understand the current structure before any change.

FILE: frontend/src/app/load-samples/page.tsx

Use the view/read tool. Paste the ACTUAL literal JSX code (with line numbers) — NOT summaries, NOT /* comments */. I need to see the real structure to plan a safe scroll fix without breaking anything.

Show me:

1. The complete "Select Sample" card — from its outermost wrapping <div> (the one forming the card / holding the blue "Select Sample" header) through: the header bar, the filter row (Sample Start Date, End Date, Year, Name, Closed, Search button), the "Showing X of Y" bar, the grid wrapper div(s), the <DataTable>, and pagination — down to that card's closing </div>. Include EVERY nested <div className="..."> exactly as written.

2. The complete "Load Samples" card — same, from its outer <div> through header/buttons, the grid wrapper div(s), <DataTable>, pagination, to its closing </div>.

3. Specifically point out (with line numbers) EVERY place that currently has:
   - overflow-x-auto
   - min-w-[...] or min-w-max
   - max-w-... or w-full
   - any fixed width / min-width class
   in these two card sections.

4. The outermost page container div and its className (the one wrapping both cards).

RULES:
- Paste real code lines with line numbers, exactly as in the file.
- Do NOT replace code with descriptions or comments.
- Do NOT edit anything — this is read-only for planning.
- If long, paste all of it.
