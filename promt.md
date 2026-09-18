Context: .NET 8 Clean Architecture backend + Next.js/React/TypeScript frontend,
CASRR project. The backend for "CRM Summary for Management" is already complete
(new service/contract/repository/endpoint, fully additive — ICrmSummaryReportService,
SqlCrmSummaryReportRepository, and all existing endpoints remain untouched).
The Selections row script is already created and will be run separately.

I am attaching the reference prototype PDF ("Review Summary for Management",
23 pages) separately in this conversation — use it as the primary source of truth
for section layout and table columns. Below is a written summary of its structure
to guide you, extracted from pages 1, 22, and 23 of that prototype:

STRUCTURE (one page — or more, if content overflows — PER REVIEW, same pattern as
the existing CrmSummaryDocument):

1. Header navy bar: title "Review Summary for Management" on the left.
   (Override the prototype's header-right content — see "Header/Footer styling"
   section below; do NOT copy the prototype's right-side "SAMPLE_ID - DATE -
   REVIEW_TYPE - SEGMENT" label as-is.)

2. "CUSTOMER NAME" label + customer name (bold, large).

3. Info grid, two rows:
   Row 1: CUSTOMER # | REVIEW ID | UNIT | RELATIONSHIP MANAGER | COMMITTED | BANK PD
   Row 2: PORTFOLIO | MARKET | PORTFOLIO MANAGER | OUTSTANDING | CAS PD

4. "RISK RATING JUSTIFICATION" heading + paragraph (free text).

5. Scorecard table, columns in order:
   SCORECARD ID | DATE | BANK PD | BANK LGD | CAS PD | CAS LGD | SCORECARD TYPE |
   SCORECARD ASSESSMENT

6. Category-ratings table (5 fixed columns, values like "Satisfactory"):
   RISK RECOGNITION | SCORECARD MANAGEMENT | UNDERWRITING | CREDIT SERVICING |
   LOAN ADMINISTRATION

7. Findings table (appears per review, filtered to Finding_level = 'Finding' AND
   Finding_code <> 'CRM-00' — same filter as the backend), columns in order:
   CRM COMPONENT | CODE | SEVERITY | FINDING TYPE | REVIEW COMMENTS
   IMPORTANT: check the full attached prototype (not just the 3 pages I described)
   to confirm whether reviews with zero qualifying findings still render this table
   (e.g. empty/omitted) or are skipped entirely — follow whatever the prototype
   actually does across all its pages, don't guess from a partial sample.

8. Final page: "APPLIED REPORT FILTERS" block — plain text line listing filter
   name/value pairs (REPORT NAME, SAMPLE ID, REVIEW STATUS, START DATE, END DATE,
   SEGMENT, UNIT, MARKET, RELATIONSHIP MANAGER, PORTFOLIO MANAGER, PORTFOLIO
   CLASSIFICATION, PORTFOLIO SEGMENT, PORTFOLIO INDUSTRY, SPECIAL ASSETS,
   CENTRALIZED COMMERCIAL (CCL), REVIEWER) — same style as the existing
   "Applied Report Filters" block used in other CRM reports in this codebase
   (match that existing component/pattern, don't build a new one).

TABLE RENDERING FIX (apply to this new report's tables, and note this is a
recurring issue across all CRM Summary-style reports since DejaVuSans font
metrics differ from the default font):
- No cell's content may wrap into or bleed across an adjacent column — this
  specifically affects long values like SCORECARD ID (a GUID). Fix via fixed/
  explicit column widths (not auto-sizing) and CSS white-space/overflow handling
  (e.g. truncate with ellipsis, or wrap within its own cell only — never into a
  neighboring cell). Check how DejaVuSans is registered/used in this component
  and size columns accordingly so this doesn't regress.

HEADER/FOOTER STYLING (do NOT copy the prototype's old style — use the new
standardized pattern we just implemented on Checklist Questionnaire):
- Header navy bar: white date/time text, right-aligned — this must be the report
  GENERATION/download date/time (not a sample label).
- Footer: centered "Review Summary for Management • Page X of Y" — same pattern
  as Checklist Questionnaire's and Non-Compliant Covenants' footers. Per-page
  numbering runs across the whole document (all reviews + the filters page), not
  reset per customer.

ROUTING (frontend):
- In `frontend/src/app/reports/page.tsx`, the label/id mapper has:
  `if (s.includes("crm summary") && !s.includes("table")) return "crm-summary";`
  "CRM Summary for Management" contains "crm summary" and would incorrectly match
  this existing rule. Add a new, more specific rule BEFORE that line (e.g. matching
  "for management") that returns a new id like "crm-summary-for-management", so it
  is checked first and the existing rule's behavior for the real "CRM Summary"
  report is completely unchanged.
- Add the matching `isCrmSummaryForManagement` guard inside `isCrm` in
  `onGeneratePdf`, following the same pattern used for the existing guards —
  without modifying the existing guards.

Acceptance criteria:
- New report renders with the structure above, filtered findings only, correct
  filename with no numeric prefix ("Review Summary for Management"), white
  header date, centered footer with report name + page number.
- No column's content ever wraps into another column.
- Selecting the existing "CRM Summary" report still routes and behaves exactly as
  before — verify this explicitly after your routing change.
- No other existing report is altered or broken.
