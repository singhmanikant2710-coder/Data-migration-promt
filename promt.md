Single-file edit: frontend/src/app/reports/page.tsx

Fix: The CRM Summary Table report's header shows the old name because the reports page passes meta.title: "CRM Summary Table" to the PDF component. Geoff wants this report titled "CRM Findings Summary Table". Update the title string at this source (the CrmSummaryTablePDF invocation), so the header matches the footer/report-name which already say "CRM Findings Summary Table".

Locate the CrmSummaryTablePDF invocation inside the isCrmSummaryTable branch of onGeneratePdf.

BEFORE:
<CrmSummaryTablePDF
  data={data as any}
  meta={{ title: "CRM Summary Table", filters: filtersEcho }}
/>

AFTER:
<CrmSummaryTablePDF
  data={data as any}
  meta={{ title: "CRM Findings Summary Table", filters: filtersEcho }}
/>

CONSTRAINTS:
- ONLY change the title string in this ONE meta object, inside the isCrmSummaryTable branch.
- Do NOT change filters, data, or any other report's invocation.
- Do NOT change the dropdown/selection label in reporting.ts (getReportNames) — that's the report picker label, a separate concern; changing it could affect report selection/matching logic. Leave it as-is unless Geoff asks.
- Do NOT touch CrmSummaryTablePDF.tsx.
- Do NOT touch any other file.
- Only edit this one file. Show the diff.
