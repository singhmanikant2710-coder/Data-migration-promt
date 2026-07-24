Frontend only. Single file: frontend/src/app/review-queue/page.tsx
Do NOT modify any other file. Do not plan. Read the current controls-row JSX first and report what the current layout classes are, then apply the fix.

Problem: The "My View" and "Sample Name" filter controls at the top of the Review Queue page are misaligned. The "My View" label wraps onto two lines and its select is squeezed to almost zero width (only the dropdown arrow is visible), while the "Sample Name" select stretches too wide. They previously rendered as two evenly sized, bordered controls sitting side by side.

Fix the layout so that:

1) Each control sits in its own container with a fixed, comfortable width — "My View" around w-48, "Sample Name" around w-80. Neither should collapse or stretch to fill.
2) Each label ("My View" and "Sample Name") sits on a SINGLE line above its select — add whitespace-nowrap to the labels so they cannot wrap.
3) Both selects keep the identical bordered box styling: same border width, border colour, border radius, padding and background. Read the classes currently on the two selects and make them match each other exactly.
4) The two controls are laid out side by side in the controls row with sensible spacing, with "Sample Name" positioned on the right side of the row (as it was before).

Do NOT change either select's value, options, or onChange handler. Do NOT touch the status tiles, the search box, page size, the grid, or anything else on the page.

After applying, report the before/after of the changed container and label classes, and run read-only TypeScript diagnostics on this file only.
