Don't rely on me guessing a selector. You have repo access — read the code and find the actual header element yourself:

1. Open CrmFindingsAndRatingsSection.tsx and trace where the "REVIEW FORM — CRM FINDINGS" toolbar (Edit/Save/Cancel/Email) is rendered. Find its real id/class/data-attribute, or identify the shared component that renders it.

2. Preferred approach: if that header is rendered within reach of this screen's component tree, attach a React ref to the actual header element and measure from the ref — no document.querySelector, no guessed selector that can silently return null.

3. For the height: inspect the toolbar's styling in code. If it has a fixed height (single row, fixed px/rem), use a CSS-only padding-top on this screen's root container equal to that height — no JS at all. Only use ResizeObserver if the height is genuinely dynamic.

Report what you find (the real selector/ref + whether height is fixed) BEFORE writing code. Single-file scoped to CrmFindingsAndRatingsSection.tsx, no shared component edits, no packages. Wait for my approval on the diff.
