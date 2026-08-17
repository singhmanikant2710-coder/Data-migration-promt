READ-ONLY. Diagnostics only. Do NOT change anything. Do NOT edit any file.

I need to find where the "CRM Findings and Observations" EXCEL export is generated (not the PDF). This is the Excel/xlsx export, likely in a service, API, or export utility — not the PDF component.

STEP 1 — Locate files (run commands, show output paths ONLY, do not open/read):
grep -rl "CRM Findings" frontend/src backend/src --include=*.ts --include=*.tsx --include=*.cs
grep -rl "findings-observations" frontend/src backend/src --include=*.ts --include=*.tsx --include=*.cs
grep -rl "xlsx\|ExcelExport\|export.*excel\|SheetJS\|EPPlus\|ClosedXML" frontend/src backend/src --include=*.ts --include=*.tsx --include=*.cs

Show only file paths. Do NOT open or read any file yet.

STEP 2 — Once paths are known, tell me (paths + one-line role each):
- Which file(s) build the CRM Findings and Observations Excel export specifically.
- Whether the export is built on the FRONTEND (JS/SheetJS) or BACKEND (C#/EPPlus/ClosedXML) or via a SQL query/stored procedure.

Read once. Findings only. No edits.
