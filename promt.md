STOP. The issue is still NOT fixed in the actual UI.

Your previous analysis says the grid frame is now the horizontal scroll owner, but the screenshot still shows the horizontal scrollbar visually behaving as a narrow table/grid scrollbar. Do not mark this task as complete based only on the JSX structure.

We need to debug the ACTUAL rendered DOM and computed widths.

FILE:
frontend/src/app/load-samples/page.tsx

IMPORTANT:
Do not modify DataTable.tsx or any other file.

FIRST: DO NOT EDIT ANYTHING.

Inspect the actual rendered DOM in the browser for both cards and determine:

1. Which exact DOM element has:
   scrollWidth > clientWidth

2. Which exact DOM element has:
   overflow-x: auto / scroll

3. What are the actual computed values for:
   - card width
   - scroll wrapper clientWidth
   - scroll wrapper scrollWidth
   - inner wrapper clientWidth
   - inner wrapper scrollWidth
   - DataTable width
   - DataTable scrollWidth

4. Identify whether DataTable itself, any parent, or the page-level container is causing width shrink/overflow.

5. Verify whether the horizontal scrollbar visible in the screenshot belongs to:
   A. card grid wrapper
   B. DataTable wrapper
   C. DataTable itself
   D. another parent

Do NOT infer this from JSX. Verify it from the browser DOM/computed layout.

--------------------------------------------------
REQUIRED FINAL STRUCTURE
--------------------------------------------------

For SELECT SAMPLE:

<Card>
   card header
   filters

   <div className="... overflow-x-auto ...">
      <div className="... min-w-[appropriate width] ...">

          grid toolbar/header area

          DataTable

          pagination / horizontally aligned grid content

      </div>
   </div>
</Card>

For LOAD SAMPLES:

<Card>
   card header

   <div className="... overflow-x-auto ...">
      <div className="... min-w-[appropriate width] ...">

          grid toolbar/header area

          DataTable

          pagination / horizontally aligned grid content

      </div>
   </div>
</Card>

There must be exactly ONE horizontal overflow owner for each card's grid area.

--------------------------------------------------
CRITICAL WIDTH REQUIREMENT
--------------------------------------------------

Do NOT blindly use min-w-max.

The inner wrapper must have a width that is actually larger than the viewport when the grid requires horizontal scrolling.

Use the smallest appropriate explicit/min-content width that preserves the existing DataTable's natural column widths.

For example, if inspection proves necessary:

min-w-[1100px]

or another justified value.

Do NOT arbitrarily choose a number.

Explain why the chosen width is required.

--------------------------------------------------
VERY IMPORTANT
--------------------------------------------------

The current implementation:

<div className="overflow-x-auto">
    <div className="min-w-max">
        ...
        <DataTable ... />
    </div>
</div>

has already been tried and did NOT solve the UI problem.

Therefore, do not simply recreate the same structure with different formatting.

Find the actual reason the browser is still rendering the scrollbar at the wrong scope.

--------------------------------------------------
PAGINATION
--------------------------------------------------

Pay special attention to pagination.

The horizontal scroll container should contain the horizontally aligned grid content consistently.

Do not create a second horizontal scroll container for pagination.

If pagination should visually remain outside the horizontal scroll area, explain why and make that decision only after checking the requirement and actual UI.

--------------------------------------------------
SAFETY CONSTRAINTS
--------------------------------------------------

Only modify:

frontend/src/app/load-samples/page.tsx

Do NOT change:

- DataTable component
- DataTable props
- column definitions
- render functions
- state
- event handlers
- sorting
- pagination logic
- dropdown logic
- date picker logic
- filter logic
- business logic
- card headers
- vertical layout
- spacing
- any other file

Only restructure the relevant horizontal-scroll wrapper/layout classes.

--------------------------------------------------
AFTER IMPLEMENTATION
--------------------------------------------------

DO NOT simply say "fixed".

Verify in the browser.

For Select Sample:

- create/edit a sample so the grid becomes wide
- verify exactly ONE horizontal scrollbar
- verify the scrollbar belongs to the intended grid/card wrapper
- verify scrolling horizontally moves ALL grid content together
- verify there is no nested DataTable scrollbar

For Load Samples:

- verify its horizontal scrollbar is independent
- scrolling Select Sample must NOT scroll Load Samples
- scrolling Load Samples must NOT scroll Select Sample

Also verify normal non-wide view.

--------------------------------------------------
FINAL RESPONSE MUST INCLUDE
--------------------------------------------------

1. Root cause based on actual browser DOM measurements.

2. Before DOM hierarchy.

3. After DOM hierarchy.

4. FULL git diff of frontend/src/app/load-samples/page.tsx.

5. Exact number of horizontal scroll containers in Select Sample.

6. Exact number of horizontal scroll containers in Load Samples.

7. Confirmation that no DataTable props/columns/renderers/business logic changed.

8. Browser verification results.

Do NOT claim completion unless the browser verification confirms the required behavior.
