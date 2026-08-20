Fix Bug #189 in the Covenants section.

Current Issues

In the Covenant and Monitoring Information section, there are three dropdown fields:

1. Are Monitoring Covenants Accurately Tracked and Defined
2. Are Covenants Accurately Calculated and Validated
3. Are Covenant Breaches Adequately Addressed and Mitigated

The dropdown options are:

- Yes
- No
- N/A

Currently, the following problems exist:

1. No is displayed in RED, but it must be GREEN.
2. Yes must be displayed in RED.
3. N/A must remain N/A after clicking Save. Currently, selecting N/A and saving changes the value to No.
4. The issue occurs across all three Covenant fields.

Expected Behavior

The final behavior must be:

- "Yes" → RED
- "No" → GREEN
- "N/A" → N/A and must not be converted to "No"

The selected value must remain unchanged after:

- selecting the value
- clicking Save
- API request/response
- navigating away and coming back
- refreshing/reloading the page

Important Root-Cause Requirement

Do NOT fix this by changing only the displayed UI value or adding a frontend-only workaround.

First trace the complete data flow for all three fields:

"Dropdown selection → component state → Save handler → request payload → API/backend → database/persistence → API response → state initialization → UI rendering"

Find exactly where:

"N/A → No"

conversion is happening.

Also find exactly where the color for "Yes", "No", and "N/A" is determined and why "No" is currently receiving the RED color.

Fix Requirements

1. Fix the actual source of the "N/A → No" conversion.
2. Ensure the literal value "N/A" is preserved end-to-end.
3. Fix the color mapping:
   - Yes = RED
   - No = GREEN
   - N/A = default/neutral color unless an existing requirement specifies otherwise.
4. Apply the fix consistently to all three Covenant fields.
5. Do not change unrelated fields, sections, APIs, or existing functionality.
6. Do not introduce hardcoded display-only values.
7. Do not duplicate logic if an existing shared mapping/helper can be corrected.
8. Keep the implementation minimal and consistent with the existing project architecture.

Validation

After implementing the fix, test all three fields individually.

For each field:

Test 1

- Select "Yes"
- Save
- Reload
- Verify value = "Yes"
- Verify color = RED

Test 2

- Select "No"
- Save
- Reload
- Verify value = "No"
- Verify color = GREEN

Test 3

- Select "N/A"
- Save
- Reload
- Verify value = "N/A"
- Verify it does NOT become "No"

Also verify that the API request payload and response contain "N/A" when "N/A" is selected.

Before Editing

Inspect the relevant files and identify the exact location responsible for:

- Covenant dropdown state
- Save/update logic
- API payload construction
- API response mapping
- Any "N/A"/"No" conversion
- Yes/No/N/A color mapping

Do not assume "CovenantsSection.tsx" is the only file involved. If the bug originates in another file, fix it at the actual source.

After making the changes, provide:

1. Files changed
2. Exact root cause
3. What was changed
4. Confirmation that N/A remains N/A after Save/reload
5. Confirmation that Yes is RED and No is GREEN
