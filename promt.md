The grid scrolling issue is now working correctly. DO NOT change the grid/table implementation again.

I tested the UI after your previous fix and attached the latest screenshots.

The normal Samples page looks good.

However, there is ONE remaining issue in Create Sample mode:

The top filter/action row is still wider than the available page/container width. On the right side, the Search button is partially cut off / pushed outside the visible container.

This means the top filter section is still causing horizontal overflow.

Please fix ONLY this remaining issue.

Requirements:

1. Keep the current grid implementation exactly as it is.
   - Do NOT remove overflow-x-auto.
   - Do NOT remove the grid horizontal scrollbar.
   - Do NOT change DataTable behavior.
   - Do NOT change table-fixed/min-width implementation.

2. Fix ONLY the top filter/action row:
   - Sample Start Date
   - Sample End Date
   - Sample Year
   - Sample Name
   - Closed
   - Search
   - Clear

3. These controls must fit completely inside the Samples page container.
   - Search must never be cut off.
   - Clear must never be cut off.
   - No control should go outside the container.
   - No page-level horizontal scrolling should be created.

4. Use the available width efficiently.
   - Keep the controls compact.
   - Reduce unnecessary fixed/min widths if any.
   - Allow the filter row to flex/wrap naturally when required.
   - Do NOT give the entire page horizontal scrolling.

5. Important:
   The horizontal scrollbar should exist ONLY for the wide Sample/Load Samples grids, NOT for the filter section and NOT for the entire page.

6. Preserve everything else:
   - No business logic changes.
   - No API changes.
   - No state changes.
   - No filtering/search behavior changes.
   - No sorting/pagination changes.
   - No Create Sample functionality changes.
   - No unrelated refactoring.

Make the smallest possible CSS/layout-only change.

Final expected behavior:

MAIN PAGE
→ fixed/stable
→ no horizontal page overflow

FILTER ROW
→ compact
→ aligned
→ all controls fully visible
→ Search + Clear fully visible

GRID
→ independent horizontal scroll
→ wide editable columns remain inside grid
→ Create Sample does not expand the page
