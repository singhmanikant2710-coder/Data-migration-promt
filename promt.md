Frontend only. Single file: frontend/src/app/review-queue/page.tsx. Do not plan. Just apply.

The "Sample Name" dropdown has no visible box border, while the "My View" dropdown on the left renders as a bordered box. The client (UAT #141) asked for the Sample Name control to use the same border as My View.

1) Read the exact className currently applied to the "My View" <select> element in this file.
2) Apply the identical border-related classes (border width, border colour, border radius, background, padding) to the "Sample Name" <select> so both controls look visually identical as bordered boxes.
3) Do NOT change the My View control itself. Do NOT change the Sample Name control's width, its label, its options, or its onChange handler.

If the Sample Name select is currently rendered by a different component (e.g. SearchableSelect) than My View (a plain <select>), report that instead of guessing, and state what would be needed to match them.

Run read-only TypeScript diagnostics on this file only.
