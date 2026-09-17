Context: .NET 8 Clean Architecture backend + Next.js/React/TypeScript frontend,
CASRR project. I need to add a brand-new report entry called "CRM Summary for
Management" that does not currently exist in the report dropdown.

IMPORTANT: Do not modify, remove, or refactor any existing working report pipeline,
routing logic, or shared services other than the specific additions described below.

Requirements:
1. Add a new Selection entry in 03_LIBRARY_09_Selections so "CRM Summary for
   Management" appears as a new option in the report dropdown (follow the exact
   pattern used by existing Selection entries in that table).
2. This report must mirror the structure/layout of the existing "CRM Summary" and
   "CRM Findings and Observations" reports (reuse their existing PDF layout/component
   patterns as the visual/structural reference).
3. Data filter: only include records where CORE Findings.Finding_level = "Finding"
   (i.e., this is a Findings-only filtered version of the CRM Summary report).
4. I have a reference prototype PDF titled "Review Summary for Management"
   (original filename `10_Review Summary for Management`) — I will attach/paste its
   structure separately; use it as the exact layout/column spec once I share it.
   If I haven't shared it yet in this conversation, ask me for it before finalizing
   column layout — don't guess the structure.
5. Filename rule: the downloaded file name must be "Review Summary for Management"
   — strip any numeric prefix (do NOT include "10_" in the output filename).
6. Implementation approach: prefer deriving/reusing the existing CRM Summary
   report's backend pipeline (repository/service/controller) with an added
   Finding_level = "Finding" filter, rather than building a fully new pipeline from
   scratch — but check existing code first and follow whichever pattern keeps this
   consistent with how other CRM reports are structured in this codebase.
7. Header/footer must follow the same styling pattern as other CRM reports in this
   codebase (white date/time text on the navy header bar, footer centered showing
   report name + page number — e.g. "Review Summary for Management • Page 1 of X").

Acceptance criteria:
- New "CRM Summary for Management" option appears in the report dropdown.
- Generates a PDF matching the Review Summary for Management prototype's structure,
  filtered to Finding-level records only, with correct filename (no number prefix).
- Header/footer styling matches the rest of the CRM reports.
- No existing report is altered or broken.
