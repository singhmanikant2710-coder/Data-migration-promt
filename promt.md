READ-ONLY. Diagnostics only. Do NOT change anything.

File: frontend/src/app/load-samples/page.tsx (and its layout if relevant)

The card grids now scroll correctly inside their cards. But the WHOLE page is still slightly wider than the screen — the left edge ("CASRR" logo) and right edge (Search button, pagination) get cut off. This is a PAGE-LEVEL overflow, not the cards.

Show me ONLY (no edits):
1. The outermost page container (line ~2034: <div className="mx-auto max-w-7xl px-4 py-4 flex flex-col gap-4">) and EVERYTHING that wraps it — go UP from line 2034 to the component's top-level return. Show any parent <div>, <main>, <section> and their classNames.
2. Is this page wrapped by a layout file? Check for frontend/src/app/load-samples/layout.tsx OR a parent layout (frontend/src/app/layout.tsx). If a layout wraps this page with a sidebar/shell that has fixed width or padding, show that layout's main content wrapper and its className.
3. Specifically look for: any element with a fixed width, a width larger than the viewport, negative margins, or missing overflow handling that would make the whole page exceed the screen width. Also check if there's a sidebar (the "«" pin icon on the left suggests a collapsible sidebar) whose width isn't accounted for.

Read once. Findings only. No edits. I want to find why the whole page (not the cards) exceeds the viewport width.
