Single-file edit: frontend/src/app/load-samples/page.tsx

Fix: In CREATE mode the grid exceeds the screen width (~1100px+ vs min-w-[1000px]), cutting off the right side (Search/pagination). In normal mode the page fits fine. Reduce the create-mode dropdown widths slightly (they were over-widened) and lower the grid min-width so the grid fits the screen in create mode, while still scrolling only when genuinely needed. Do NOT touch AppShell.

=== Reduce isCreating dropdown widths (w-48 -> w-40, w-40 stays) ===
EIC Name cell — change isCreating width from w-48 to w-40:
BEFORE: className={isCreating ? "w-48 min-w-0" : "w-28 min-w-0"}   (wrapper)
AFTER:  className={isCreating ? "w-40 min-w-0" : "w-28 min-w-0"}
BEFORE: className={isCreating ? "w-48" : "w-32"}                    (SearchableSelect)
AFTER:  className={isCreating ? "w-40" : "w-32"}

Target (BU) cell — same change (w-48 -> w-40):
BEFORE: className={isCreating ? "w-48 min-w-0" : "w-28 min-w-0"}   (wrapper)
AFTER:  className={isCreating ? "w-40 min-w-0" : "w-28 min-w-0"}
BEFORE: className={isCreating ? "w-48" : "w-32"}                    (SearchableSelect)
AFTER:  className={isCreating ? "w-40" : "w-32"}

Type — keep w-40 as is (already reasonable). No change.

=== Lower the Select Sample grid min-width so it fits the screen ===
BEFORE: <div className="min-w-[1000px]">
AFTER:  <div className="min-w-[900px]">

CONSTRAINTS:
- Do NOT touch AppShell or any shared component.
- Do NOT change the Load Samples grid's min-w (leave it as is).
- Do NOT change any DataTable props, columns logic, dropdowns options, or handlers.
- Keep the non-creating (normal) widths exactly as-is (w-28/w-32).
- Show the diff.
