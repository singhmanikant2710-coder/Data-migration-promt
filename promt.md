Task: On the Customer Info tab, convert the "FHN NAICS INDUSTRY" field from a read-only text field into an editable dropdown (SelectField). Options must be a DISTINCT list from the live database.

Data source (use LIVE DB, ignore columns.csv):
- Table: 01_DATA_01_Data Mart Trial
- Field: IntRepCMLSubCategory
- Options = distinct values, trimmed, sorted alphabetically.
- NOTE: Use "SELECT DISTINCT TOP (n)" ordering, NOT "SELECT TOP (n) DISTINCT" (that is invalid SQL Server syntax).

Constraints:
- READ-ONLY diagnostics first. Show me:
  (a) the current "FHN NAICS INDUSTRY" JSX in CustomerInfoSection.tsx (its section/name/value binding),
  (b) whether an endpoint already returns distinct IntRepCMLSubCategory; if not, propose the smallest backend addition following the existing SelectionsController/repository pattern.
- IMPORTANT (performance): The dropdown options must be fetched ONCE on mount, not on every render. Use a useEffect with an empty dependency array [] and a cancellation guard. Do NOT create any effect that re-fetches in a loop. Confirm in your plan that the fetch runs only once.
- Keep the field binding (section/name/value) unchanged so the existing Save path persists the value.
- Use ensureIncludesSelected to keep the current saved value visible.
- Single-file edits, step-by-step, wait for my confirmation at each file.
