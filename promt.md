Task: On the Customer Info tab, convert the "FHN NAICS INDUSTRY" field from a read-only text field into an editable dropdown (SelectField). Options must be a DISTINCT list sourced from the live database.

Data source (use LIVE DB, ignore columns.csv):
- Table: 01_DATA_01_Data Mart Trial
- Field: IntRepCMLSubCategory
- Dropdown options = unique/distinct values of IntRepCMLSubCategory, sorted alphabetically.

Constraints:
- READ-ONLY diagnostics first. Show me:
  (a) confirmation that field IntRepCMLSubCategory exists on 01_DATA_01_Data Mart Trial (live DB), and a sample of a few distinct values,
  (b) the current "FHN NAICS INDUSTRY" field JSX in CustomerInfoSection.tsx (find its current binding: section/name/value),
  (c) whether an existing API endpoint already returns distinct values from a Data Mart table; if not, propose the smallest backend addition (a GET endpoint returning distinct IntRepCMLSubCategory) following the existing SelectionsController / repository pattern.
- Then propose the full plan BEFORE editing: backend endpoint (if needed) + frontend service helper + SelectField wiring.
- Keep the field binding (section/name/value) EXACTLY as it is now so the existing Customer Info Save path persists the value unchanged.
- Single-file edits, step-by-step. Wait for my confirmation at each file.
- Do NOT hardcode the list; it must come from the live DB.
