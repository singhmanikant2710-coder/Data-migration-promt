Fix ONLY the following UI bug on the Samples page.

TARGET FILE:
frontend/src/app/load-samples/page.tsx

IMPORTANT SCOPE RESTRICTION:
- Make changes ONLY in:
  frontend/src/app/load-samples/page.tsx
- Do NOT modify any other file.
- Do NOT modify backend/API files, shared components, global CSS, layout files, configuration files, or unrelated components.
- Do NOT change existing business logic or functionality.
- Do NOT refactor unrelated code.

I have attached screenshots showing the issue.

BUG:
When the user creates a new Sample on the Samples page, the page becomes vertically cut off. Part of the Samples/Load Samples content goes below the visible viewport, and there is no usable vertical scrollbar to access the hidden content.

EXPECTED RESULT:
- The Samples page should remain fully accessible after creating a Sample.
- The page should have proper vertical scrolling when the content height exceeds the viewport.
- The page should maintain the same alignment and overall layout as the existing Samples page.
- No section should be clipped or hidden at the bottom.
- The existing header, sidebar, Select Sample section, Load Samples section, buttons, tables, filters, pagination, and other UI elements should remain unchanged.
- Do not unnecessarily change heights, widths, spacing, or visual styling.
- Fix the root cause of the page cut-off/overflow issue only.

IMPORTANT:
Before making any change, inspect ONLY this file and identify the container causing the page to be clipped or preventing vertical scrolling.

Prefer a minimal targeted fix such as correcting the relevant height/overflow/flex behavior in this file, if that is the actual root cause.

DO NOT:
- Add a hardcoded fixed height workaround.
- Hide any content.
- Reduce the size of existing UI.
- Remove existing functionality.
- Change API calls or data handling.
- Modify any other file.

VALIDATION:
1. Verify the Samples page before creating a Sample remains exactly as it currently works.
2. Create a Sample.
3. Verify the complete page is accessible.
4. Verify vertical scrolling works when required.
5. Verify no content is clipped at the bottom.
6. Verify existing Sample creation, tables, filters, pagination, buttons, and other functionality still work.
7. Ensure ONLY frontend/src/app/load-samples/page.tsx was modified.

After completing the fix, provide:
1. Root cause of the issue.
2. Exact change made in page.tsx.
3. Confirmation that no other files were modified.
4. Confirmation that existing functionality was preserved.
