Implement inline edit functionality for the CRM Findings screen exactly like the CAS Findings screen.

Requirements:

1. When the user clicks the Edit button for a row:
   - Convert ONLY the selected row into edit mode.
   - Do not open any popup or modal.
   - Replace read-only controls with editable controls within the same row.

2. Pre-populate all editable controls with the existing row values:
   - CRM Component -> Dropdown (selected value should be current value)
   - Finding Code -> Dropdown (selected value should be current value)
   - Severity -> Dropdown (selected value should be current value)
   - Finding Comments -> Textarea with existing comment loaded
   - Any other editable field should display its current value.

3. While editing:
   - Display Save and Cancel buttons exactly like the CAS Findings screen.
   - Hide the Edit button for the row being edited.
   - Only one row should be editable at a time.

4. On Save:
   - Persist the updated values using the existing save functionality.
   - Return the row to read-only mode.

5. On Cancel:
   - Discard all unsaved changes.
   - Restore the original values.
   - Return the row to read-only mode.

6. UI
   - Match the inline editing experience of the CAS Findings screen.
   - Maintain existing spacing, alignment, and responsive layout.

Important:
- Do NOT modify any business logic, API, validation, backend, database, or existing functionality.
- Do NOT change any other screen, shared component, or reusable control.
- Implement this ONLY for the CRM Findings screen.
- Existing behavior outside this screen must remain completely unchanged.
