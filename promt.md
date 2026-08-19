The horizontal scroll issue is NOT fixed yet.

I verified the UI after your previous implementation. In the Select Sample card, the horizontal scrollbar still appears directly under the DataTable/grid, instead of behaving as the single scroll container for the entire grid area.

Do NOT assume that moving overflow-x-auto one level up solved the issue.

Analyze the actual rendered DOM structure in frontend/src/app/load-samples/page.tsx and determine which element is currently producing the horizontal scrollbar.

Required behavior:

1. Select Sample card:
   - Exactly ONE horizontal scroll container for the grid area.
   - The scroll container must contain the complete grid content:
     - grid/header toolbar
     - DataTable header
     - DataTable rows
     - horizontally aligned grid content
   - The table/content must have a width larger than the viewport when required.
   - Scrolling horizontally must move the complete grid content together.
   - There must NOT be a nested horizontal scrollbar around only the DataTable.

2. Load Samples card:
   - Exactly ONE independent horizontal scroll container.
   - It must behave independently from Select Sample.

Important:
The previous implementation used:

<div className="overflow-x-auto">
  <div className="min-w-max">
    ...
    <DataTable ... />
  </div>
</div>

This did NOT solve the UI issue. Do not repeat the same structure without verifying which element actually owns the scrollbar.

Inspect the DataTable DOM/CSS behavior as read-only analysis. Do NOT modify the shared DataTable component or any other file.

If DataTable itself creates an internal overflow container, identify exactly how that prevents the parent card wrapper from becoming the scroll owner. Then solve it ONLY from frontend/src/app/load-samples/page.tsx, without changing DataTable props, column definitions, render functions, state, event handlers, or business logic.

Do not change:
- DataTable props
- columns
- render functions
- state
- event handlers
- sorting
- pagination
- dropdowns
- date pickers
- filters
- card headers
- vertical spacing/layout
- any other file

Only restructure the horizontal-scroll wrappers in the two card sections.

Before editing:
1. Show the current relevant DOM/wrapper hierarchy.
2. Identify which exact element currently owns the horizontal scrollbar.
3. Explain why the previous fix did not work.

Then implement the minimal fix.

After editing:
1. Show the FULL diff.
2. Confirm exactly ONE overflow-x-auto exists for Select Sample.
3. Confirm exactly ONE overflow-x-auto exists for Load Samples.
4. Confirm there is no nested horizontal overflow wrapper around DataTable in either card.
5. Confirm no DataTable props or column/business logic changed.
6. Confirm both cards scroll independently.
7. Verify the resulting DOM structure logically guarantees that the outer card grid wrapper, rather than an inner table wrapper, owns horizontal scrolling.

Do not claim the issue is fixed unless the actual DOM structure supports the required scroll behavior.
