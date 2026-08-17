READ-ONLY. Diagnostics only. Do NOT change anything. Do NOT edit any file.

File: frontend/src/app/reports/page.tsx

Find the onExport() function's isCrmFindings branch (the ExcelJS block that builds crm-findings-observations.xlsx). Show me ONLY (no edits):

1. The column definitions / worksheet header setup for this export — specifically the columns that map to "CRM Component" (Column L), "Code" (Column M), and "Severity" (Column N). Show how each cell value is assigned (the source field from the data + any formatting/mapping).

2. For CRM Component: show exactly what value is written. Is there any existing mapping (e.g. code -> label like "00-CRM Admin"), or is it written raw from the data field?

3. For Code: same — is "CRM-00" style value written raw or mapped?

4. For Severity: show exactly what is written. Is there any default/fallback applied when the value is empty/null (e.g. defaulting to "Observation" or "N/A" or left blank)?

5. Show the raw data field names these three columns read from (e.g. row.crmComponent, row.code, row.severity).

Read once. Findings only. No edits. I want to distinguish: (a) simple display formatting we can fix, vs (b) data default-value/migration behaviour we should NOT change.
