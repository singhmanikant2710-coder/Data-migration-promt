Bug in ReviewSummaryForManagementPDF.tsx — the "Satisfactory" ratings table (RISK RECOGNITION | SCORECARD MANAGEMENT | UNDERWRITING | CREDIT SERVICING | LOAN ADMINISTRATION) has its header row overlapping with the data row below it — the navy header bar text is clipped/cut off at the top, and the "Satisfactory" values render overlapping the header bar instead of below it. See attached screenshot (page 3 of the generated PDF).

READ-ONLY first, then fix:
1. Find this specific table's header <View> and data <View> in the file (the 5-column "UNSATISFACTORY CRM RATINGS" style table). Paste both blocks exactly.
2. Compare against how the SAME kind of header+data row pair is styled in an existing WORKING report (e.g. CrmSummaryPDF.tsx's equivalent Unsatisfactory CRM Ratings table, which we already fixed for page-break issues in Bug 225) — is there a missing marginBottom, a wrong position: absolute, incorrect height, or a missing wrap={false} causing this overlap?
3. Identify the exact styling difference causing the overlap.

Fix it to match the working pattern from CrmSummaryPDF.tsx exactly for this table type. Show diff. Rebuild. Do NOT commit — I'll re-generate the PDF and check this table (and re-verify the Scorecard Assessment table right above it, which rendered correctly, isn't affected by the fix).
