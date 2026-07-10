Task: On the CRM Findings tab, the "Finding Code" dropdown should display the finding description alongside the finding code, to help the user select the correct finding. (UAT #53)

Currently the dropdown shows only the Finding Code. We want each option to show "CODE - Description" so the user can identify the right finding.

Data source (use LIVE DB, ignore columns.csv):
- The findings master is 03_LIBRARY_01_CAS Findings (Finding_code -> category, description, guidance).
- Confirm the exact table/columns that hold Finding_code and its description on the live DB.

Constraints:
- READ-ONLY diagnostics first. Show me:
  (a) the current CRM Findings component and how the Finding Code dropdown is populated — what data source/endpoint provides the codes now? Show the JSX and the fetch.
  (b) whether the description is already available in that same data (so we can display "code - description" without a new query), or if we need to fetch it.
  (c) the exact columns on 03_LIBRARY_01_CAS Findings for code and description.
- Then propose the smallest change: display "Finding_code - Finding_description" as the dropdown option text, while the SAVED value remains Finding_code only (do NOT change what gets saved — only the display label).
- Keep binding and save path unchanged.
- Single-file edits, step-by-step, wait for confirmation. Do not edit yet.
