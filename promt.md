Context: CASRR (.NET 8 + Next.js/React/TypeScript + SQL Server). Production banking app: existing behaviour must not break.

TASK (follow-on to UAT #208/#209, approved by Geoff): CRM Summary PDF only — rename two labels (you found them earlier at CrmSummaryPDF.tsx ~:541-542):
- "TTBA APPROVER" → "RCA APPROVER"
- "TTBA"          → "HOUSEHOLD EXPOSURE"
Label text only (keep uppercase style). No change to values, data, variables, layout or other reports.

PHASE 1 (read-only): confirm file:line; check if the longer "HOUSEHOLD EXPOSURE" label fits its field width or wraps/overlaps; list any other place in the CRM Summary report (Excel export, filters) still showing "TTBA" — list only, don't change.
PHASE 2-3: change the two strings.
PHASE 4: build frontend.

Report: CHANGED lines (file:line), PDF before/after, label wrapping result, other TTBA occurrences found (not changed), build result, git status (no harness files). Do not commit or push.
