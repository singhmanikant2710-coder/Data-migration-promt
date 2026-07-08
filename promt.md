Task: In the Customer Info tab, hide the "NAICS DESCRIPTION" field completely from the UI.

Constraints:
- Use live DB knowledge only; ignore backend-schema/columns.csv.
- READ-ONLY diagnostics first: locate the Customer Info frontend component and find where the "NAICS DESCRIPTION" label and its input/textarea are rendered. Show me the exact block before editing.
- Single-file edit only.
- Do NOT remove the field from any DTO, API, or backend — only hide it in the frontend render (e.g., remove/comment the JSX block for NAICS DESCRIPTION label + input). Keep FHN NAICS INDUSTRY intact.
- After showing the diagnostic, wait for my confirmation, then make the single edit.
