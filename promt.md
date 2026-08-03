Fix the footer issue in `components/pdf/CroProductionSummaryPDF.tsx`.

Reference file:
- `CrmSummaryPDF.tsx` (Footer implementation is correct)

Issue:
The CRO Review Production PDF footer is not matching the CRM Summary PDF. It is missing the report name and proper page numbering.

Expected Footer:
CRO Review Production · Page X of Y
(or use "CRO Production" if that is the official report name used in the application.)

Requirements:
- Compare `CroProductionSummaryPDF.tsx` with `CrmSummaryPDF.tsx`.
- Reuse the same footer implementation, styling, alignment, spacing, and formatting.
- Display only:
  • Report Name
  • Page X of Y
- Footer should appear on every page of the PDF.
- Do not modify the report content, tables, header, or pagination.
- Reuse existing shared footer/page-number logic if available instead of creating new code.

Acceptance Criteria:
✔ Footer matches the CRM Summary PDF exactly.
✔ Report name is displayed on every page.
✔ Page numbering is shown as "Page X of Y".
✔ Footer styling and alignment are identical to CRM Summary.
✔ No layout or pagination regressions.

Please analyze both files, identify why the footer is missing in `CroProductionSummaryPDF.tsx`, and implement the same footer logic used in `CrmSummaryPDF.tsx`.
