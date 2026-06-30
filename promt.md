The current implementation is incorrect. Please revert the broken layout and apply the following corrections ONLY for the CRM Findings screen.

Issues to Fix:

1. Do NOT collapse the Finding Comments column.
2. Do NOT allow text to render vertically (letter-by-letter).
3. Maintain a proper responsive table layout where all columns remain horizontally aligned.
4. Remove unnecessary horizontal scrolling caused by incorrect column widths.
5. The table should occupy the available width and distribute column widths proportionally.
6. Finding Comments should wrap words normally within the cell (word-wrap), not character-wrap.
7. Header and body columns must remain perfectly aligned.
8. Preserve the existing scrollbar behavior only for long content inside the comments area if required, but never because of incorrect column sizing.

Edit Mode Requirements:
- By default, show all values as read-only text.
- Only when the existing TOP Edit button is clicked should the fields become editable.
- Show dropdowns and textareas only in Edit mode.
- Pre-populate all controls with existing values.
- Save and Cancel should continue using the existing functionality.

UI Requirements:
- Match the approved PPT.
- Use the Light Sky Blue for the Finding Description section and column headers (not green).
- Keep spacing, typography, and borders consistent with the PPT.

Restrictions:
- Do NOT modify business logic, APIs, backend, validation, models, or existing functionality.
- Do NOT modify any other screen or shared component.
- Only fix the CRM Findings UI and edit-mode behavior.
