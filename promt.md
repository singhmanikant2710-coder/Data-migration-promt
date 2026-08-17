READ-ONLY. Diagnostics only. Do NOT change anything. Do NOT edit any file.

I need to find where the report title "CRM Summary Table" is defined/passed to the CrmSummaryTablePDF component, so the name can be changed at its source (not hardcoded per-report).

STEP 1 — Find the caller (paths only, no file reads yet):
STOP reading files. Run ONE command, show output paths only:
grep -rl "CrmSummaryTablePDF\|CRM Summary Table" frontend/src --include=*.ts --include=*.tsx

STEP 2 — Once paths are known, for the file(s) that render/invoke CrmSummaryTablePDF or set its meta.title, show me ONLY (no edits):
1. Where meta.title (or the title value) is set to "CRM Summary Table" — the exact line and surrounding object/config.
2. Is there a central place (a report config, registry, map, or constants file) where ALL report titles/names are defined? If so, show that structure — I want to see if there's one source of truth for report names.
3. Is the same title string used for BOTH the header AND the footer/report-name across reports, or set separately per spot?

Read once each. Findings only. No edits.
