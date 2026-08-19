Single-file edit: frontend/src/app/load-samples/page.tsx

Fix: Restore readable create-mode field sizes and set the grid min-width so create-mode content scrolls fully (Save/Cancel are inside the scroll container, confirmed). Normal/search mode unchanged. Do NOT touch AppShell.

1. EIC Name — w-40 -> w-44 (create mode only):
   wrapper: className={isCreating ? "w-44 min-w-0" : "w-28 min-w-0"}
   SearchableSelect: className={isCreating ? "w-44" : "w-32"}

2. Target (BU) — w-40 -> w-44 (create mode only):
   wrapper: className={isCreating ? "w-44 min-w-0" : "w-28 min-w-0"}
   SearchableSelect: className={isCreating ? "w-44" : "w-32"}

3. Type — w-40 -> w-44 (create mode only):
   Select: className={isCreating ? "w-44" : "w-28"}

4. Select Sample grid inner div — min-w-[900px] -> min-w-[1200px] so create-mode scrolls fully to Save/Cancel:
   <div className="min-w-[1200px]">

CONSTRAINTS:
- Non-creating widths stay w-28/w-32 (search/view mode unchanged).
- Do NOT touch AppShell, shared components, DataTable, columns logic, handlers.
- Load Samples grid min-w unchanged.
- Show the diff.
