Task: Change CRM Findings "Follow-Up" column from text (Yes/No) to a checkbox.

In the CRM Findings tab of the review-info screen, the Follow-Up column currently renders text values "Yes" / "No". Instead, render it as a checkbox:
- Yes → checked (ticked) checkbox
- No → unchecked (empty) checkbox

Workflow (strict — follow exactly):
1. Read-only diagnostics first. Locate the component/file that renders the CRM Findings table and the Follow-Up column cell. Identify the field backing this value (likely a followUp boolean or a "Yes"/"No" string) and confirm its data type before changing anything. Report what you find.
2. Single-file edit only. Change only the rendering of the Follow-Up cell to a checkbox reflecting the value. Do NOT alter data-fetching, the API contract, or the underlying field type — if the value is a "Yes"/"No" string, map it to checked/unchecked in the render layer rather than changing the data model.
3. Keep the checkbox read-only / disabled, since this column is display-only (the existing text was not editable). Preserve existing alignment, styling, and column header.
4. No new npm/NuGet packages. Use a native <input type="checkbox"> or the existing checkbox component already used in the add-row UI for FOLLOW-UP.
5. STOP and ask before proceeding if the change requires touching any other file (shared types, API response, DTO, mapper, etc.).
6. Wait for my manual approval before applying the edit.

Acceptance: Each row in the Follow-Up column shows a checkbox — ticked when the value is Yes, empty when No — matching the style of the existing follow-up checkbox in the add-row form.
