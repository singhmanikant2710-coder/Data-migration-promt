Context: CASRR (.NET 8 + Next.js/React/TypeScript + SQL Server). Production banking app: existing behaviour must not break.

TASK (follow-on to UAT #193, separate commit): On the Review Form SCREEN (not PDF), a table pasted from Word sticks out of the rich-text field box — it starts left of the box edge and can extend past the right edge — both while editing (RichTextEditor contentEditable) and in read mode after Save. Excel-pasted and editor-created tables look fine. Word sends inline styles like margin-left:-5.4pt, width:468pt, and MsoTableGrid classes. PDF output is already correct — do not touch PDF code.

Work in 4 phases:
PHASE 1 (read-only): file:line of the rich-text editor and the read-mode HTML display (every screen that shows saved rich text: Review Form sections, any preview), and their current CSS for tables/images.
PHASE 2: smallest generic CSS-only fix, scoped to the rich-text containers only:
- tables never overflow the field box: max-width:100%; negative left margins neutralised (margin-left:0) inside the container; wide content scrolls horizontally inside the field (overflow-x:auto on the container) instead of spilling out.
- Do NOT rewrite or sanitise the saved HTML; do not change stored data or the PDF path.
- Existing editor-created width:100% tables and Excel tables must look the same as today.
PHASE 3: implement; match existing style approach (global CSS / module / Tailwind — whichever the editor uses).
PHASE 4: build frontend.

Report:
1. Root cause (file:line)
2. ADDED lines (file:line)
3. REMOVED / CHANGED lines (file:line)
4. Screen before vs after: Word table (edit + read mode), Excel table, editor table, pasted image, very wide table
5. Empty field / no table — unchanged
6. Other screens using the same styles checked
7. Build results
8. git status (htmlLayout.ts and test-data/ are ours)
Do not commit or push.
