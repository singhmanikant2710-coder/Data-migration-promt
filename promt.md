New report: "CRM Findings for Management" (Bug 208/217). Mirror the pattern already established for "CRM Summary for Management" — same file location, same shared modules, same header/footer convention. Show a skeleton first (imports + header/footer + routing) before building the full table logic — I'll confirm before you proceed.

DESIGN SOURCE: Mirrors CrmFindingsAndObservationsPDF.tsx's structure, but filtered to Finding-level rows only: CORE Findings.Finding_level = "Finding" (exclude Observation-level rows).

STRUCTURE (from the prototype, content only — NOT its header/footer):
- Grouped by CRM Component (Risk Recognition, Scorecard Management, Underwriting, Credit Servicing, Loan Administration) as section headings, in that fixed order — same as the other CRM findings-style reports.
- Within each component, grouped by Finding Code + Type as a sub-heading with a count in parens, e.g. "SM-104 - LGD Scorecard Inputs - (2)".
- Each sub-group is a 2-column table: CUSTOMER NAME (REVIEW ID) | COMMENTS (long-form finding comment text, wrapped, via HtmlRichText since these are rich-text fields — same handling as Bug 192's fix).
- Skip any CRM Component / Finding Code group with zero Finding-level rows (do not render empty sections).

HEADER/FOOTER — use the EXISTING standard, NOT what's in the prototype image:
- Header: navy #1F3864 bar, title "CRM Findings for Management" left, generatedOn date (via formatDate, date only, no time — same as the recent Checklist Questionnaire fix) right.
- Footer: centered "CRM Findings for Management • Page X of Y", no logo, no <Image> import — mirror ReviewSummaryForManagementPDF.tsx's ReportFooter exactly.

FILE LOCATION AND SHARED MODULES — identical rules to ReviewSummaryForManagementPDF.tsx:
1. New file: frontend/src/components/pdf/CrmFindingsForManagementPDF.tsx — same folder as every other report component. No separate folder, no separate package.json/build tooling.
2. Import from ./pageSetup: PAGE_SIZE, PAGE_ORIENTATION, MARGINS, colors, crmTypography, fontSizes, formatDate, lineHeights, spacing, buildFilterParagraph, softBreakId (now shared, per the earlier hoist — do NOT duplicate it a third time).
3. Import HtmlRichText, decodeEntities, stripHtmlToText from ./HtmlRichText.
4. Column widths: flexBasis percentages (the dominant convention in this codebase, confirmed — not getPageWidthPts).
5. Table rows: wrap={false} on header AND data rows (Bug 225 lesson — required since COMMENTS can be long/multi-line).

DROPDOWN + FILENAME:
- This report needs a NEW Selection entry added to 03_LIBRARY_09_Selections (Tab='Reporting', Section='Report Selections') — dropdown label "CRM Findings for Management".
- Download filename: "CRM Findings for Management.pdf" — no numeric prefix.

ROUTING: Wire into the EXISTING frontend/src/app/reports/page.tsx exactly like ReviewSummaryForManagementPDF was wired — new entry in toReportId(), new isCrmFindingsForManagement-style guard in onGeneratePdf. Backend call via a new function in the existing services/api/reporting.ts.

BACKEND: New model + repository + service + controller endpoint, following the EXACT same pattern as CrmSummaryForManagement's backend pipeline (which was just built) — same folder conventions, same DI registration pattern. Check whether the existing ManagementSummaryResponse.FindingsOnly collection (mapped from legacy REPORTING_02_Reports_04_CRM Summary_05_Findings Only) can be reused as the data source instead of a brand-new query — confirm this before building a new query from scratch.

STOP after showing: (a) the skeleton (imports + header/footer + routing entries), (b) confirmation of whether FindingsOnly can be reused or a new backend query is needed. Do NOT write the full table/data logic yet — I'll review the skeleton first.

Do NOT commit anything.
