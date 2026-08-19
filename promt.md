TASK: Fix horizontal scroll scope on the Load Samples page (React + Tailwind CSS).

FILE: frontend/src/app/load-samples/page.tsx

=== CONTEXT ===
This page has TWO separate card sections stacked vertically:
1. "Select Sample" card — has a dark-blue header bar labelled "Select Sample", a row of filter fields, a data grid (DataTable) with columns like Sample Name, Start, End, EIC Name, Type, Target (BU), and pagination.
2. "Load Samples" card — has a dark-blue header bar labelled "Load Samples", action buttons, a data grid (DataTable) with columns like Customer #, Customer Name, CRO Name, Review Type, Hold, Last Reviewed, Validation Result, and pagination.

=== PROBLEM ===
When the grid content is wider than the screen (e.g. when editing a new sample, or with many columns), the page needs horizontal scrolling. Currently there IS a horizontal scrollbar, but it is placed too deep — it only wraps the inner DataTable, so it appears as a small scroll strip inside the grid and doesn't scroll the whole card nicely.

DESIRED BEHAVIOUR:
- The "Select Sample" card should have ONE horizontal scrollbar that scrolls the entire card's wide content as a unit.
- The "Load Samples" card should have its OWN separate horizontal scrollbar that scrolls its content independently.
- Each scrollbar should belong to its own card — they must be independent, not one shared scroll for the whole page.

=== CURRENT CODE STATE ===
Currently, inside each card, the DataTable is wrapped like this:
  <div className="overflow-x-auto">
    <div className="min-w-[900px]">
      <DataTable ... />
    </div>
  </div>
This wrapper is too far inside (only around the table), which is why the scroll feels cramped and doesn't cover the card properly.

=== WHAT TO IMPLEMENT ===
For EACH of the two cards (Select Sample and Load Samples) independently:

1. Identify the card's outer container div (the one that visually forms the card — usually has border/rounded/background classes).

2. Move the horizontal-scroll responsibility UP to wrap the scrollable content of the card. Concretely, wrap the portion of the card that can become too wide (the grid area including its column headers, rows, and horizontally-aligned content) in:
   <div className="overflow-x-auto">
     <div className="min-w-max">   {/* or a suitable min-width like min-w-[1100px] */}
       ...the grid content...
     </div>
   </div>

3. Use `min-w-max` (content-driven) OR an explicit min-width (e.g. min-w-[1100px]) on the inner div so the content keeps its natural width and the outer div scrolls, instead of the table shrinking to fit.

4. Remove the now-redundant deeper `overflow-x-auto` + `min-w-[900px]` wrapper that was only around the DataTable (so there aren't nested/duplicate scroll containers causing a cramped inner scrollbar). There should be exactly ONE horizontal scroll container per card.

5. Keep the two cards' scroll containers completely separate — do NOT wrap both cards in a single shared scroll container.

=== IMPORTANT SAFETY CONSTRAINTS ===
- This must NOT break any existing functionality. The DataTable, its columns, sorting, inline editing (dropdowns like EIC Name / Type / Target BU, date pickers), pagination, and the filter fields must all keep working exactly as before.
- Do NOT change any DataTable props, column definitions, render functions, state, or event handlers.
- Do NOT change the vertical layout, card headers, buttons, or spacing — only adjust the horizontal-scroll wrapper divs.
- Do NOT touch any other file or component (especially do NOT modify the shared DataTable component or SearchableSelect).
- Only restructure the scroll-wrapper divs within these two card sections in this one file.
- Before finalizing, show the FULL diff, and confirm: (a) there is exactly one horizontal scroll container per card, (b) no DataTable prop or column logic changed, (c) the two cards scroll independently.

=== VERIFICATION AFTER CHANGE ===
- Select Sample card: when content is wide (e.g. creating a new sample), one scrollbar scrolls the whole grid area smoothly.
- Load Samples card: has its own independent scrollbar.
- All dropdowns, date pickers, sorting, pagination still work.
- No layout breakage in normal (non-wide) view.
