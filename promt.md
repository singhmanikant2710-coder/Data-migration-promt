Update ONLY the CRM Findings screen.

Requirements:

1. Use the existing TOP Edit button to control edit mode.

2. Default (Read-Only Mode)
- When the page loads or Edit has not been clicked:
  - Display all values as plain text.
  - Do NOT show dropdowns.
  - Do NOT show textareas.
  - Do NOT show editable controls.
  - The screen should look exactly like the CAS Findings screen in read-only mode.

3. Edit Mode
- When the existing top Edit button is clicked:
  - Switch the CRM Findings section into edit mode.
  - Replace text with editable controls:
      • CRM Component → Dropdown
      • Finding Code → Dropdown
      • Severity → Dropdown
      • Finding Comments → Textarea
      • Info → Existing editable control
  - Pre-populate all controls with the current values.
  - Keep using the existing Save and Cancel functionality.
  - No new Edit buttons should be added.

4. UI Styling
- Match the approved PPT.
- Remove the current green header styling.
- Use a Light Sky Blue background for:
   • Finding Description header
   • Column headers
- Match the PPT's colors, spacing, typography, borders, and padding.

5. Layout
- No horizontal scrolling.
- Entire grid should fit within a single desktop screen.
- Wrap long comments instead of increasing the page width.
- Maintain proper alignment of all controls.

6. Restrictions
- DO NOT modify any business logic.
- DO NOT modify APIs, backend, validation, models, or database.
- DO NOT change any other screen, shared component, or existing functionality.
- Apply changes ONLY to the CRM Findings screen.
