New report: "Unsatisfactory Transactional Ratings" (Selection_id 5, already in dropdown). Mirror the exact pattern established for CrmFindingsForManagementPDF.tsx and ReviewSummaryForManagementPDF.tsx — same file location, same shared modules, same header/footer convention. Show a skeleton first before building the full logic — I'll confirm before you proceed.

DATA SOURCE: The 5 boolean UNSAT columns + their comment fields on dbo.[02_CORE_02_Reviews] (Risk recognition UNSAT, Scorecard mgmt UNSAT, Underwriting UNSAT, Credit servicing UNSAT, Loan admin UNSAT — each with a paired *_comments field), joined with standard review/customer info (Customer_name, Customer_number, Review_id, Unit, Market, RM Name, PM Name). This is the same 10-column pattern already consumed by 6 existing report repositories (CrmSummary, CrmSummaryTable, CrmSummaryForManagement, InitialMemo, FinalMemo, CrmFinalMemos) — reuse the established query/filter pattern from one of those (closest: CrmSummaryForManagement's per-review structure), do NOT invent a new query shape.

CONTENT STRUCTURE (from the prototype "05_CRM Unsatisfactory Ratings.pdf" — content only, NOT its header/footer/logo):
- One block per REVIEW that has at least one of the 5 UNSAT flags = true. Reviews with ALL 5 flags false are skipped entirely (do not render an empty block).
- Each block starts with a customer-info row: CUSTOMER NAME, CUSTOMER #, REVIEW ID, Unit, Market, RM Name, PM Name.
- Below that, all 5 components are listed as checkboxes in a fixed vertical list (Risk Recognition, Scorecard Management, Underwriting, Credit Servicing, Loan Administration) — checked/unchecked state reflects that review's actual flag values (all 5 shown regardless of true/false, matching the prototype).
- For each CHECKED component only, render its rationale/comment text directly below that checkbox line (rich-text via HtmlRichText, since these are RichTextEditor-sourced fields — same handling as other CRM reports' comment fields).
- Unchecked components show no comment text (prototype shows this — UNSAT Scorecard Management etc. have no text beneath when unchecked).

HEADER/FOOTER — use the EXISTING standard (NOT what's in the prototype):
- Header: navy #1F3864 bar, title "Unsatisfactory Transactional Ratings" (or "CRM Unsatisfactory Ratings" — confirm which matches the dropdown Selection label exactly) left, generatedOn date (via formatDate, date only) right.
- Footer: centered "<Report Title> • Page X of Y", no logo, no <Image> import — mirror CrmFindingsForManagementPDF.tsx's ReportFooter exactly.
- Applied Report Filters trailing page via buildFilterParagraph — same pattern as the other new reports.

FILE LOCATION AND SHARED MODULES — identical rules to the last two new reports:
1. New file: frontend/src/components/pdf/UnsatisfactoryTransactionalRatingsPDF.tsx (or a name matching the dropdown label convention) — same folder as every other report component. No separate folder, no separate build tooling.
2. Import from ./pageSetup: PAGE_SIZE, PAGE_ORIENTATION, MARGINS, colors, crmTypography, fontSizes, formatDate, lineHeights, spacing, buildFilterParagraph, softBreakId (only if a GUID-style ID column is used — confirm, likely not needed here since there's no Scorecard ID).
3. Import HtmlRichText, decodeEntities, stripHtmlToText from ./HtmlRichText.
4. flexBasis percentage widths where a table format applies (though this content is more of a checkbox-list + text layout than a table — design accordingly, matching the prototype's visual structure, not forcing it into a table grid if it doesn't naturally fit).

DROPDOWN + FILENAME: 
- Selection already exists (confirm exact current label text from the DB/dropdown — it showed as "Unsatisfactory Transactional Ratings" in the screenshot).
- Download filename: match the label, no numeric prefix (e.g. "Unsatisfactory Transactional Ratings.pdf" or "CRM Unsatisfactory Ratings.pdf" — whichever matches the confirmed dropdown label).

ROUTING: Wire into the EXISTING reports/page.tsx exactly like the other two new reports were wired — new toReportId() rule (placed carefully to avoid collision with existing "unsatisfactory" or "ratings" substring checks — check for any near-miss guards first, e.g. isScorecardResults needing "scorecard" won't collide, but verify no other guard partially matches "unsatisfactory" or "transactional" or "ratings"), new is*() guard, dispatch entry.

BACKEND: New model + repository + service + controller endpoint, following the EXACT same pattern as CrmFindingsForManagement's backend pipeline (just built) — same folder conventions, same DI registration pattern, same ReportFilters parameter envelope and review-status predicate reuse.

STOP after showing: (a) the skeleton (imports + header/footer + routing entries + confirmed report title/filename), (b) the backend query design (confirm it reuses the established 5-flag + comments pattern from an existing repository rather than inventing new logic). Do NOT write the full block-rendering logic yet — I'll review the skeleton first.

Do NOT commit anything.
