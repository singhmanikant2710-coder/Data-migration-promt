UI Layout Issue – Review Status page is not using the full available content width

I have attached two screenshots for visual comparison:

1. Review Queue screen – this is the correct/reference layout.
2. Review Status screen – this is the screen with the layout issue.

Issue:
On the Review Status screen, there is unwanted white space/margin on both the left and right sides of the main page content.

The Review Status content appears horizontally constrained or centered inside a fixed/max-width container. Because of this, the page does not use the complete available width between the left sidebar and the right edge of the viewport.

The Review Queue screen correctly expands across the available content area and should be used as the reference for the expected page width and horizontal alignment.

Expected behavior:
- Review Status should use the full available content width, exactly like the Review Queue screen.
- Remove the unwanted white margin/gap from both the left and right sides.
- The main Review Status container should start at the same horizontal position as the Review Queue content area.
- The right edge of the Review Status content should extend/alignment-match the Review Queue screen.
- The REVIEW STATUS header, Select Sample Name section, Sample Start Date, Sample End Date, status summary cards, filter/bucket/page-size toolbar, and data grid should all use the available content width consistently.
- Preserve the existing sidebar and top navigation/header layout.
- Do not introduce horizontal page overflow or an unnecessary browser-level horizontal scrollbar.
- The layout must remain responsive.

Important:
Do not redesign the page.
Do not change colors, fonts, card styling, grid styling, data, business logic, API calls, filtering logic, or component behavior.

Please investigate the actual root cause before making changes.

Compare the Review Status page layout implementation with the Review Queue page and specifically inspect:
- outer page wrapper
- main content wrapper
- width / w-full classes
- max-width / max-w-* constraints
- mx-auto
- margin-left and margin-right
- padding-left and padding-right
- container classes
- flex/grid parent width constraints
- parent layout/sidebar content calculations
- fixed widths
- calc(...) width rules
- responsive breakpoint classes
- inline styles or global CSS affecting page width

Implementation requirement:
Use the Review Queue screen as the source of truth for the main content width and horizontal spacing.

Identify the exact wrapper/container or CSS/Tailwind class causing Review Status to have the left and right white margins.

Apply the minimum required code change so that Review Status follows the same full-width layout pattern as Review Queue.

Do not use arbitrary negative margins, hardcoded viewport widths, absolute positioning, transform/translate hacks, or CSS overrides with !important unless absolutely required by the existing architecture.

Before editing:
1. Inspect the Review Queue page and identify how its outer content container achieves full available width.
2. Inspect the Review Status page and compare its parent/outer wrapper hierarchy.
3. Find the exact difference responsible for the horizontal margins.
4. Check shared layout components before changing shared CSS.

After editing:
1. Verify Review Status fills the same available horizontal space as Review Queue.
2. Verify the left and right alignment against the attached screenshots.
3. Verify the status cards and grid resize correctly.
4. Verify there is no browser-level horizontal overflow.
5. Verify Review Queue and other existing pages are not visually impacted.
6. Run TypeScript/build/lint validation for the modified files.

Please make the code changes directly.

After completing the fix, provide:
- Root cause
- Exact file(s) modified
- Exact class/style/container causing the issue
- Before vs after layout behavior
- Summary of code changes
- Validation performed

Do not make unrelated changes.
