STOP. The issue is NOT vertical height.

The actual bug is HORIZONTAL LAYOUT / CONTENT CLIPPING after clicking "Create Sample".

Look carefully at the attached screenshots.

BEFORE Create Sample:
- The Samples page fits correctly inside the viewport.
- The main page/container stays within the available screen width.

AFTER Create Sample:
- The "(new)" editable row appears.
- Because the editable Sample columns are wider, the Select Sample grid is expanding beyond its parent/page width.
- The RIGHT SIDE OF THE PAGE/CONTENT IS GETTING CUT OFF / CLIPPED.
- The main page is effectively being pushed wider instead of keeping a fixed viewport width and scrolling ONLY inside the grid.

THIS IS THE EXACT ISSUE TO FIX.

EXPECTED BEHAVIOR:
Use the same layout behavior as the Review Queue screen shown in the reference screenshot.

The MAIN PAGE must remain fixed/contained within the available viewport.

Only the WIDE DATA GRID should horizontally scroll.

Desired structure:

Page/Main container
  └── Select Sample section (width: 100%, cannot expand page)
       └── Grid wrapper (width: 100%, min-width: 0, overflow-x: auto)
            └── Wide DataTable / editable row
                 └── horizontal scrollbar INSIDE this grid

IMPORTANT:
The wide table is allowed to be wider than the visible grid area.
The table must NOT force its parent/page to become wider.

The page itself must NOT get horizontally expanded.
The right side of the page must NOT be cut off.

Use the Review Queue screen as the visual/reference behavior:
- Main screen remains stable.
- Grid stays inside its section.
- Horizontal scrollbar belongs to the grid.
- Wide columns remain accessible by scrolling the grid.

CRITICAL CSS/LAYOUT REQUIREMENTS:

1. Find the actual parent/container that is expanding when Create Sample is enabled.

2. Make sure the main/page container has:
   - width: 100%
   - max-width behavior preserved
   - min-width: 0 where required
   - NO width expansion caused by child content

3. The Select Sample section must be allowed to shrink within its parent:
   - min-width: 0

4. The immediate grid wrapper must own horizontal scrolling:
   - width: 100%
   - max-width: 100%
   - min-width: 0
   - overflow-x: auto

5. The DataTable itself may retain its required wide/fixed column layout, BUT its width must not expand the page.

6. Check ALL relevant flex/grid parents for the common issue:
   min-width: auto

   If a flex/grid parent is preventing shrinking, use:
   min-width: 0

   only at the appropriate container level.

7. Do NOT solve this by:
   - reducing column widths
   - reducing font size
   - hiding columns
   - clipping the table
   - adding overflow-hidden to hide the problem
   - adding page-level overflow-x-auto
   - changing browser/page width
   - using arbitrary fixed widths
   - changing business logic

8. Do NOT remove the existing horizontal scrollbar from the grid.

9. Do NOT modify:
   - sorting
   - filtering
   - pagination
   - Create Sample logic
   - editable row logic
   - API calls
   - validation
   - state management
   - DataTable functionality

10. Existing normal Samples page must remain visually unchanged.

MOST IMPORTANT TEST:

Test BOTH states:

STATE 1 — Create Sample OFF
The page must look exactly as it currently does.

STATE 2 — Create Sample ON
The "(new)" row appears.

In STATE 2:
- Page width must remain exactly within viewport.
- Nothing on the right side should be cut off because of the table.
- Select Sample grid gets its OWN horizontal scrollbar.
- Scrollbar must be inside the Select Sample grid.
- User can horizontally scroll and access all editable columns.
- Load Samples section must remain within the same page width.
- No page-level horizontal expansion.

Compare the implementation with the Review Queue screen:
The Review Queue proves that a wide table can scroll horizontally WITHOUT expanding/cutting the main page.

Find why the Samples page behaves differently and make the minimum targeted CSS/layout fix.

DO NOT blindly change heights.

DO NOT refactor the page.

First identify the exact element causing the horizontal overflow, then fix that parent/child relationship.

After fixing:
1. Run TypeScript/build.
2. Test Create Sample OFF.
3. Test Create Sample ON.
4. Confirm the page itself no longer expands/cuts.
5. Confirm only the grid scrolls horizontally.
6. Tell me exactly which wrapper/class was changed.
