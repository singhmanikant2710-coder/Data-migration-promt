Before implementing, two things:

1. First check (read-only) whether the sticky header has a known/fixed CSS height. If it does, prefer a pure-CSS fix on this screen's content container (padding-top equal to the header height, or scroll-margin-top) instead of JS measurement — it's more reliable than querySelector + resize listeners. Report what you find.

2. If the height is genuinely dynamic and JS measurement is needed, do NOT use a guessed selector. The header element on this screen is the "REVIEW FORM — CRM FINDINGS" black bar containing Edit/Save/Cancel/Email. Inspect the actual rendered DOM/component for this screen and use the real selector or, better, a React ref to the actual header element if it's rendered within this same component — not a global document.querySelector that can silently return null.

Either way: single-file, scoped to CrmFindingsAndRatingsSection.tsx, no shared component edits, no new packages. Report your finding from step 1 before writing any code, and wait for my approval on the chosen approach.
