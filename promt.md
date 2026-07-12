Approved with two conditions — apply the plan as described, plus:

1. CRITICAL — confirm and guarantee: the derived `activeDesc` is DISPLAY ONLY. It must never be written back into state.findingDescription, never included in any save/PUT payload, and Save must continue to persist exactly the same value it does today. State this explicitly in your summary after applying.

2. Row selection must not interfere with editing: clicking inside a row's dropdown, text input, or rich-text comment editor should still work normally (the onClick/onFocusCapture on the <tr> must not steal focus, swallow events, or close the dropdowns). Verify the Finding Code searchable dropdown and the Finding Comments rich-text editor still function when the row is clicked.

Apply the single-file change to CrmFindingsAndRatingsSection.tsx. Show me the diff. STOP if any other file needs changing.
