#256 – Implemented. Tested in Dev and pushed to the develop branch. Not yet deployed to QA.

What changed: "Export Report" now produces an Excel file for all six reports, using exactly the same data and filters as the PDF:
- CRM Summary for Management – Reviews, Scorecards and Findings sheets.
- CRM Findings for Management – Findings and Finding Counts sheets.
- CRM Scorecard Results – Scorecard Totals and Scorecard Details sheets.
- Credit Policy Exceptions – Exception Totals and Exception Details sheets.
- Non-Compliant Covenants – Summary, Monitoring Totals, Performance Totals and Violation Details sheets.
- CRO Review Production – Monthly Summary (same month/year columns and 12-month blocks as the PDF, with totals), Reviewer Details and Reviewer Totals sheets.
Every workbook also has a Filters sheet listing the filters applied. Customer Name and Review ID are in separate columns, dates and amounts are real Excel dates and numbers, headers are frozen with filters turned on, and file names match the report titles (e.g. "Credit Policy Exceptions.xlsx").

Also fixed: previously, Export on CRM Findings for Management downloaded the CRM Findings & Observations data, and Export on Non-Compliant Covenants downloaded the Covenants Summary data. Both now export their own report. The Policy Exceptions export now matches its PDF (totals plus details).

Tested in Dev: all six exports compared against their PDFs (row counts and totals), plus the CRM Findings & Observations export still working as before.

I will confirm here once deployed to QA.


git add frontend/src/lib/reportExcelExport.ts frontend/src/lib/reportExcelSheets.ts frontend/src/app/reports/page.tsx frontend/src/services/api/reporting.ts frontend/src/components/pdf/CrmFindingsForManagementPDF.tsx frontend/src/components/pdf/ScorecardResultsPDF.tsx frontend/src/components/pdf/NonCompliantCovenantsPDF.tsx frontend/src/components/pdf/CroProductionSummaryPDF.tsx frontend/src/components/pdf/ReviewSummaryForManagementPDF.tsx frontend/src/components/pdf/PolicyExceptionsPDF.tsx
git status
git commit -m "UAT #256: Excel export for CRM Summary for Management, CRM Findings for Management, Scorecard Results, Policy Exceptions, Non-Compliant Covenants and CRO Production (same data and filters as the PDF)"
git push origin develop
