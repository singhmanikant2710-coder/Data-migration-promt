You need to fix ONLY the UI layout/responsiveness issue on the Samples page. 
Do NOT change or break any existing business logic, functionality, API calls, state management, validation, sorting, filtering, pagination, CRUD behavior, or existing component behavior.

I have attached screenshots for reference:
1. Samples main page in normal mode.
2. Samples page after clicking "Create Sample".
3. Review Queue screen, which is the desired layout/scrolling reference.

CURRENT ISSUE:
When I click "Create Sample" on the Samples page, the editable row makes some fields/columns unnecessarily wide. This causes the overall page to expand horizontally and the page/layout appears pushed or broken.

Also, on the main Samples page, the top filter boxes are unnecessarily large and some controls are getting too close/overlapping instead of maintaining a clean layout.

EXPECTED UI BEHAVIOR:

1. MAIN PAGE MUST REMAIN FIXED
- The overall Samples page/container should remain within the viewport.
- Do NOT introduce horizontal scrolling on the entire page.
- Header, navigation, filters, buttons and surrounding layout must remain stable.
- The page should not shift/push sideways when Create Sample is clicked.

2. TOP FILTER SECTION
Keep these controls compact and properly aligned:
- Sample Start Date
- Sample End Date
- Sample Year
- Sample Name
- Closed
- Search
- Clear

Reduce unnecessary widths where appropriate.
Maintain consistent spacing between controls.
No controls should overlap each other.
No text, dropdown, calendar icon or button should be clipped.
Do not unnecessarily increase the height of the filter section.

If the available viewport becomes smaller, the controls may wrap naturally, but they must never overlap or break the page.

3. SAMPLE GRID
The Samples grid contains many columns, so the GRID itself should handle horizontal scrolling.

Use the Review Queue screenshot as the visual/reference pattern:
- Main page remains fixed.
- Grid/table has its own horizontal scrollbar.
- Wide columns stay inside the grid.
- Horizontal scrolling must happen ONLY inside the grid.

The horizontal scrollbar should remain available/visible for the grid when the content is wider than the available grid area.

4. CREATE SAMPLE MODE
When "Create Sample" is clicked:
- Editable inputs should remain fully usable.
- Do not allow their width to expand the entire page.
- Any extra width required by editable columns must remain contained inside the grid's scrollable area.
- The overall page width must NOT increase.
- The page must look structurally the same as the normal Samples page.

5. IMPORTANT: DO NOT COPY THE REVIEW QUEUE UI
Use Review Queue ONLY as a reference for the layout/scroll behavior.
Do not change Samples page functionality or styling unnecessarily.

6. PRESERVE EXISTING CODE
Before making changes:
- Inspect the existing layout and identify the actual cause of the overflow/width issue.
- Make the smallest possible change.
- Reuse existing components, classes and patterns wherever possible.
- Do not rewrite the page.
- Do not refactor unrelated code.
- Do not remove existing functionality.
- Do not change data structures or APIs.
- Do not change business logic.
- Do not modify unrelated screens/components.

7. VALIDATION
After implementing the fix, verify BOTH states:

A. Normal Samples page:
- Top filters are aligned and compact.
- No overlap.
- No unnecessary page-level horizontal scroll.
- Grid scroll works independently.

B. Create Sample mode:
- Editable row works correctly.
- All fields remain accessible.
- Page does not expand or shift horizontally.
- Grid can scroll horizontally when required.
- No existing functionality is affected.

IMPORTANT:
Do not make assumptions or make broad UI changes.
First identify the specific CSS/layout/container causing the page-level overflow, then apply the minimal targeted fix.

The final result should follow this principle:

MAIN PAGE = FIXED / STABLE
TOP FILTERS = COMPACT / ALIGNED
GRID = INDEPENDENT HORIZONTAL SCROLL
CREATE MODE = DOES NOT EXPAND THE PAGE
EXISTING FUNCTIONALITY = 100% PRESERVED

Before finishing, check for unintended changes and ensure the existing application still builds successfully.
