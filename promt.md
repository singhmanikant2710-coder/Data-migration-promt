Context: .NET 8 Clean Architecture backend + Next.js/React/TypeScript frontend,
CASRR project. Found a pagination bug in the "Review Summary for Management" PDF
(CRM Summary for Management report) — attaching a screenshot showing the issue:
on one customer's review, page 3 ends with the Scorecard table + the 5-column
category-ratings table (Risk Recognition/Scorecard Management/Underwriting/
Credit Servicing/Loan Administration), and page 4 is then completely blank
(only the footer "Review Summary for Management • Page 4 of 141" renders, no
content) before the next section/review continues.

IMPORTANT: Only fix the page-break/pagination logic in this component (and the
underlying page-break primitive it uses, if shared). Do not change any table
data, column layout, or styling unrelated to pagination. Do not touch other
reports unless you confirm they share the exact same page-break primitive/component
and are provably affected by the same root cause.

Investigate:
- This is likely a page-break threshold issue — when a section's remaining content
  is just slightly more than what fits on the current page, the renderer pushes
  ALL of it to the next page instead of only the overflow, or a fixed-height
  section/table wrapper is forcing a break even when there's little/no content
  left to place, leaving a mostly-empty page behind.
- Check how page breaks are computed for this document (react-pdf, or whatever
  PDF engine this project uses — check ChecklistQuestionnairePDF.tsx /
  CrmSummaryDocument's existing pattern for the same mechanism) and whether table
  rows/sections are allowed to split across a page boundary or are treated as
  atomic blocks that must fit entirely on one page — an all-or-nothing atomic
  block is likely triggering the empty page here.

Fix requirement:
- No review's content should ever produce a fully blank page (only a footer, no
  actual content) anywhere in the generated document.
- Content should flow to fill each page as fully as possible before breaking —
  don't force a break to the next page unless the current page truly has no room
  left for the next atomic element.

Testing requirement (important — this needs to be verified across variable content
lengths, not just one sample):
- Regenerate the full report across MULTIPLE reviews with different content sizes
  (short risk-rating-justification paragraphs, long ones, reviews with many
  findings rows, reviews with zero findings, single-scorecard vs
  multiple-scorecard reviews) and confirm no blank pages appear anywhere in the
  output, not just on the review shown in the attached screenshot.
- Also check whether this same page-break primitive is reused by any other CRM
  report (e.g. CRM Findings and Observations, CRM Summary) — if so, confirm
  whether they exhibit the same blank-page issue; if they do, flag it back to me
  before fixing them (since that would mean touching a shared component used by
  reports outside this task's scope).

Acceptance criteria:
- No blank/near-empty pages anywhere in a freshly generated "Review Summary for
  Management" PDF across at least 5-10 different sample reviews with varying
  content lengths.
- No existing report's pagination is altered unless you've confirmed and flagged
  it shares the same root cause.
