Implement the CRM Findings screen to match the CAS Findings inline editing behavior and the approved PPT design.

Requirements:

1. Inline Edit
- When the user clicks the Edit button, ONLY the selected row should switch to edit mode.
- Display editable controls inside the same row (no popup or modal).
- Pre-populate all fields with the existing values:
  - CRM Component -> Dropdown
  - Finding Code -> Dropdown
  - Severity -> Dropdown
  - Finding Comments -> Textarea
  - Any other editable field should display its current value.
- Show Save and Cancel buttons while editing.
- Only one row can be edited at a time.

2. Save / Cancel
- Save should use the existing save functionality and return the row to read-only mode.
- Cancel should discard unsaved changes and restore the original values.

3. Layout (Very Important)
- The CRM Findings page should fit within a single screen like the PPT/CAS Findings screen.
- Remove the need for horizontal scrolling.
- Users should not have to scroll left or right to view or edit the row.
- Adjust column widths, spacing, and responsive layout so all columns are visible on a standard desktop resolution.
- Finding Comments should wrap text appropriately instead of increasing the page width.
- Maintain proper alignment for all dropdowns, buttons, and text areas.

4. UI
- Match the CAS Findings edit experience and the approved PPT styling.
- Keep the premium header colors and section styling.
- Maintain consistent spacing, padding, typography, and alignment.

5. Important Restrictions
- DO NOT change any business logic.
- DO NOT modify APIs, backend, database, validation, models, or data binding.
- DO NOT change any existing functionality.
- DO NOT modify any other screen, tab, shared component, or stylesheet.
- Implement these changes ONLY for the CRM Findings screen.

Acceptance Criteria:
- CRM Findings visually matches the PPT.
- Clicking Edit opens inline editable controls with pre-filled values.
- Save and Cancel behave exactly like CAS Findings.
- No horizontal scrolling is required.
- All columns are visible within a single screen.
- No existing functionality outside the CRM Findings screen is affected.
