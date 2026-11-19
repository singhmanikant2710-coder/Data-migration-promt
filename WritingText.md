UAT #256 – Excel Export for six reports (DISCOVERY + PLAN first)

REQUIREMENT (Geoff)
Add Excel Export to: CRM Summary for Management, CRM Findings for Management, Scorecard Results, Policy Exceptions, Non-Compliant Covenants, CRO Production.

PHASE 1 – READ-ONLY DISCOVERY (no edits)
- How Export Report works today in reports/page.tsx (onExport, ExcelJS usage, which reports already export and how; file:line). Note: CRO Production currently falls through to a generic "accepted" call and produces no file (#252 finding).
- Existing export helpers to reuse: lib/reviewQueueExport.ts (#237 – formula-injection guard, dates, numbers, text keys), lib/reportFileName.ts (file-name rule: Proper Case, no date suffix), any other.
- For each of the six reports: data source used by the PDF (service/API/type), the sections/tables the PDF shows, and whether the same data is already available in the browser after Generate PDF or must be fetched again.
- How filters are applied, and how the PDF shows "Applied Report Filters".

PHASE 2 – PLAN (smallest generic change)
- One small shared Excel builder (reuse #237 helpers: formula-injection guard, real dates MM/DD/YYYY, numbers/currency as numbers, IDs/customer numbers as text, NULL as empty) used by all six; no new npm package.
- Same data and same filters as the PDF; nothing extra, nothing missing.
- Workbook layout per report: propose sheets (e.g. one sheet per PDF section/table + a "Filters" sheet), headers matching the PDF, frozen header row, autofilter, sensible widths. List the proposed sheets/columns per report for my approval.
- CRO Production: export the monthly matrix as shown in the PDF (#252 month/year columns and 12-month blocks with totals).
- File name: reportFileName rule (e.g. "CRM Summary for Management.xlsx").
- The Export button must work for these six without changing PDF generation or existing exports of other reports.
- Do NOT decide silently – list decisions with recommendations (sheet layout, detail vs summary only, 0-row behaviour, very large row counts).
- No backend change unless a report's data is not available in the browser – ask first.

STOP after Phase 2 and wait for my answers. Report in chat only.
