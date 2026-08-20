Apply the minimal fix identified in the READ-ONLY investigation for Bug #189.

Root cause is confirmed

Only modify:

"frontend/src/app/review/[ecif]/review-info/components/sections/CovenantsSection.tsx"

The regression was introduced by these three onChange handlers:

- accuratelyDefinedTracked
- accuratelyCalculated
- breachesMitigated

They currently contain logic equivalent to:

"const val = raw === "N/A" ? "No" : raw;"

This is incorrectly converting "N/A" to "No".

Required change

Remove ONLY the N/A → No coercion.

The selected value must flow through unchanged:

"const val = e.target.value as "Yes" | "No" | "N/A";"

Then pass "val" directly to:

- setAccuratelyDefinedTracked(val)
- setAccuratelyCalculated(val)
- setBreachesMitigated(val)

And, where applicable, pass the same "val" to "changes.setField()".

VERY IMPORTANT

Do NOT modify:

- useCovenants.ts
- backend/API
- database
- other components
- other fields
- existing color mapping

The color fix is already working and MUST remain unchanged:

- Yes → RED
- No → GREEN

The purpose of this change is ONLY to restore the previously working N/A behavior.

Expected final behavior

For all three Covenant fields:

"Yes → Save → reload → Yes + RED"

"No → Save → reload → No + GREEN"

"N/A → Save → reload → N/A"

Validation

After applying the change:

1. Select N/A in "Are Monitoring Covenants Accurately Tracked and Defined".
2. Click Save.
3. Wait for page reload.
4. Verify it displays N/A.
5. Repeat for the other two Covenant fields.
6. Verify Yes remains RED.
7. Verify No remains GREEN.

Also verify that the Save payload contains "N/A" when N/A is selected.

Do not make any additional refactoring or unrelated changes.

After editing, report:

- exact file changed
- exact lines changed
- confirmation that N/A is now passed unchanged
- confirmation that the existing Yes/No color fix was preserved
