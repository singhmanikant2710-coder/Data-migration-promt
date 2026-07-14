Apply this now. I am attaching the client's Access prototype screenshot — that is the exact target.

FILES: frontend/src/components/ui/SearchableSelect.tsx (or wherever SearchableSelect lives) and CrmFindingsAndRatingsSection.tsx

Client's complaint: "Finding code drop-down options are widely dispersed. Previous edit allowed user to see Finding Code Description alongside Finding Code and all rows were even."

Make the OPEN Finding Code dropdown look like the Access grid in the screenshot:

1. MENU WIDTH: fixed, not inherited from the trigger. min-width 640px, max-width 900px, and max-width 90vw. It is a portal overlay so it floats above the table — it must NOT stretch the table or cause page horizontal scroll.

2. STICKY HEADER ROW inside the menu, below the search box, with two labels aligned over their columns:
   "Finding Code"  |  "Finding Description"
   Slightly bolder, subtle background, bottom border. Sticky so it stays visible while scrolling.

3. EACH OPTION ROW — two columns, uniform height:
   - LEFT: the code, fixed width ~90px, nowrap.
   - RIGHT: the description, fills the rest, truncated to a SINGLE LINE with ellipsis (overflow: hidden; text-overflow: ellipsis; white-space: nowrap). NO wrapping — this is what makes every row the same height.
   - The description must be VISIBLE in the row, not only in a tooltip. Also set a title attribute with the full text as a bonus tooltip.

4. GRID LOOK: visible outer border around the menu, a subtle vertical divider between the two columns, and subtle horizontal separators between rows.

5. Keep: search box at top, vertical scroll, max-height ~320px, no horizontal scrollbar inside the menu.

IMPORTANT: add these as OPTIONAL props on SearchableSelect and default to the current behaviour when they are not passed — other SearchableSelect usages in the app must not change.

Do not touch the save path. Option value stays the raw code.

Apply and show me the diff. STOP.
