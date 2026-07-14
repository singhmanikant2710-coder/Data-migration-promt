UAT #53 (REOPENED by client) — CRM Findings, Finding Code dropdown. Fix all three issues in one pass.

CONTEXT: I have reverted all previous attempts. Start from the current clean state.

The Finding Code dropdown already works (shows code + description), but the client has reopened it with three problems. Their exact words:

(1) "Drop-down options still include Inactive codes."
(2) "Finding code drop-down options are widely dispersed. Previous edit allowed user to see Finding Code Description alongside Finding Code and all rows were even."
(3) "When selected and Saved, the Finding Code field shows the Finding Code and Description - We only need to see the Finding Code."

I am attaching the client's Access prototype screenshot. That is the target. It shows an open dropdown that looks like a small bordered GRID with:
- Column headers inside the dropdown: "Finding Code" | "Finding Description"
- Two aligned columns, code on the left, description on the right
- Every row the same height, single line each
- The closed field showing ONLY the code (e.g. "DI-101")

---

FIX 1 — Exclude inactive codes.
dbo.[03_LIBRARY_01_CAS Findings] has an [Active] BIT column (65 rows Active=1, 2 rows Active=0, including the legacy CS-116OLD). Use the LIVE DB, ignore columns.csv.
IMPORTANT: The dropdown options may NOT come from GET /api/v1/findings/library — they likely come from response.lookups.findingCodes in the review payload (populated by a query in SqlReviewRepository.cs). Find which query ACTUALLY feeds the dropdown options and apply WHERE [Active] = 1 there. Apply it to the findings/library query too (that one feeds the CAS Findings Library modal). Report which queries you changed.
Do not break saved data: if a review already has a row whose code is now inactive, that saved value must still display for that row rather than going blank — use the existing ensureIncludesSelected/label-fallback behaviour.

FIX 2 — Make the open dropdown look like the Access grid.
The list must be a bordered grid with uniform rows:
- MENU WIDTH: fixed and generous — min-width 640px, max-width 900px, max-width 90vw. It must NOT inherit the trigger/column width. It is a portal overlay, so it floats above the table and must NOT cause page horizontal scroll.
- A STICKY HEADER ROW at the top of the list (below the search box) with two labels: "Finding Code" on the left, "Finding Description" on the right, aligned exactly over their columns. Slightly bolder, subtle background, bottom border.
- EACH OPTION ROW: two columns —
  LEFT: the code, fixed width ~90px, nowrap.
  RIGHT: the description, filling the rest, truncated to a SINGLE LINE with ellipsis (overflow hidden, text-overflow ellipsis, white-space nowrap). No wrapping. This is what makes every row the same height — the client's "all rows were even".
- Add a title attribute on each row with the full description (hover tooltip), but the description MUST ALSO be visible in the row itself — not tooltip-only.
- GRID LOOK: visible outer border around the menu, a subtle vertical divider between the code and description columns, and subtle horizontal separators between rows.
- Keep the search box at the top, vertical scroll, max-height ~320px. No horizontal scrollbar inside the menu.

FIX 3 — Closed field shows ONLY the code.
The closed SearchableSelect trigger must render just the code (e.g. "CS-104"), never "CS-104 - Covenant compliance…". The description appears ONLY in the open list. This is the renderSelected prop — it must return only the code.

---

HARD CONSTRAINTS:
- The option VALUE stays the raw finding code. The save path must not change at all.
- No new API calls, no fetch loops.
- Do not touch the CRM Findings save/persist logic.
- Do not break the CAS Findings Library modal (the Info button) — it uses the same library data.

Report which queries feed the dropdown (Fix 1) BEFORE you apply, then apply all three fixes and show me the diffs.
