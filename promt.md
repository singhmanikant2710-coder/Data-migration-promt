Task: On the Customer Info tab, convert the "FHN PORTFOLIO SEGMENT" field from a read-only text field into an editable dropdown (SelectField). The dropdown options must be sourced from the database, not hardcoded.

Data source (use LIVE DB, ignore columns.csv):
- Table: 03_LIBRARY_09_Selections
- Filter: Tab = "Customer Info" AND Section = "FHN Portfolio Segment"
- Use the appropriate value column from that table as the dropdown option list (inspect the table's columns on the live DB first and show me which column holds the selection text).

Constraints:
- READ-ONLY diagnostics first. Show me:
  (a) the live DB columns of 03_LIBRARY_09_Selections and a sample of rows where Tab='Customer Info' AND Section='FHN Portfolio Segment',
  (b) the current "FHN PORTFOLIO SEGMENT" field JSX in CustomerInfoSection.tsx,
  (c) how the existing library-backed dropdowns (if any) in this project fetch options (API route + frontend service), so we follow the same pattern.
- Then propose the full plan (backend API endpoint if needed + frontend service + SelectField wiring) BEFORE editing.
- Keep the binding (section/name/value) same as the current field so the existing Save path persists the selected value.
- Single-file edits, step-by-step. Wait for my confirmation at each file.
