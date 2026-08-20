Bug #189 is partially fixed.

The color issue is now fixed:

- Yes = RED
- No = GREEN

Do NOT change the working color implementation.

The remaining issue is specifically with the N/A value.

Current behavior

When the user selects "N/A" in any of these three Covenant fields:

1. Are Monitoring Covenants Accurately Tracked and Defined
2. Are Covenants Accurately Calculated and Validated
3. Are Covenant Breaches Adequately Addressed and Mitigated

the UI allows selecting "N/A".

However, after clicking Save:

- the page reloads
- after reload, the field shows "No" instead of "N/A"

Confirmed root cause in current implementation

The current onChange implementation contains logic equivalent to:

"const val = raw === "N/A" ? "No" : raw;"

This explicitly converts "N/A" to "No".

That logic is incorrect for the required behavior.

Required behavior

If the user selects:

- "Yes" → persist "Yes" → after reload display "Yes" → RED
- "No" → persist "No" → after reload display "No" → GREEN
- "N/A" → persist "N/A" → after reload display "N/A"

"N/A" must NOT be converted to "No" anywhere in the frontend.

Important: Fix the complete persistence flow

Do not simply change the dropdown display.

Trace the complete flow for all three fields:

"Dropdown → React state → Save handler → changes.setField → API request → backend → database/persistence → API response → initial state mapping → UI"

First determine whether the backend/API/database already supports the literal value "N/A".

If the backend and persistence layer already support "N/A":

- remove the "N/A → No" normalization
- pass the original selected value through unchanged
- ensure "changes.setField()" receives "N/A"
- ensure the response mapping does not convert "N/A" to "No"

If the backend/API/database does NOT support "N/A", identify exactly where it is being rejected or transformed and make the smallest required change so "N/A" can be persisted and returned correctly.

Do NOT silently convert "N/A" to another value.

Specific frontend fix

For each of the three affected fields, the onChange logic should preserve the raw value:

"const val = e.target.value as "Yes" | "No" | "N/A";"

Then pass "val" directly to:

- the React state setter
- "changes.setField()"

Do not use:

"raw === "N/A" ? "No" : raw"

Validation

Test each of the three fields independently.

For each field:

1. Select "N/A"
2. Click Save
3. Allow the page to reload
4. Verify the field still displays "N/A"
5. Inspect the Save/API request and confirm "N/A" was sent
6. Inspect the API response after reload and confirm it contains "N/A"
7. If possible, verify the persisted/database value is "N/A"

Then verify:

"Yes → Yes → RED"

"No → No → GREEN"

"N/A → N/A"

Do not modify unrelated fields or functionality.

Keep the existing color fix unchanged.

Before editing, tell me:

- exact file/function where N/A is being converted
- whether the API/database supports N/A
- exact reason N/A becomes No after reload

Then make the minimal fix.
