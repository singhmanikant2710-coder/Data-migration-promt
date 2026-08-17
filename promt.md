READ-ONLY. Diagnostics only. Do NOT change anything. Do NOT edit any file.

Find where the "CRM Findings and Observations" EXCEL export is generated (the .xlsx export, NOT the PDF component).

Run these commands, show output paths ONLY (do not open/read files):
grep -rl "findings" frontend/src backend/src --include=*.ts --include=*.tsx --include=*.cs
grep -rl "xlsx\|ExcelExport\|SheetJS\|EPPlus\|ClosedXML\|worksheet\|Worksheet" frontend/src backend/src --include=*.ts --include=*.tsx --include=*.cs
grep -rl "crmComponent\|CrmComponent\|00-CRM\|CRM-00" frontend/src backend/src --include=*.ts --include=*.tsx --include=*.cs

Show only file paths. Then tell me (one line each) which file most likely builds the CRM Findings and Observations Excel export, and whether it's frontend (JS) or backend (C#).

Read once. Findings only. No edits.
