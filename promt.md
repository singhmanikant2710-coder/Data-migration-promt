Context: .NET 8 Clean Architecture backend + Next.js/React/TypeScript frontend,
CASRR project. I'm fixing the header/footer styling of the
ChecklistQuestionnairePDF.tsx report component (Selection_id 10) so it matches the
standard CRM report header/footer pattern already used by other working reports in
this codebase (e.g. the "Non-Compliant Covenants" PDF component — inspect that
component/style first and use it as the reference pattern).

IMPORTANT: Only touch header/footer styling in ChecklistQuestionnairePDF.tsx (and,
if the header/footer is a shared component used across multiple reports, only the
specific style props/classes relevant to this fix). Do not change table data logic,
columns, or any other report's styling/behavior.

Issues to fix:
1. Header (top navy bar): the date/time text on the right side (currently rendering
   in a dark/default color, hard to read against the navy background) must be white
   — same as the date text color in the reference "Non-Compliant Covenants" report
   header.
2. Footer: currently shows only "Page X of Y" in the bottom-right corner. It must
   instead follow the standard pattern used by other CRM reports: the report name
   + page number, centered at the bottom — e.g. "Checklist Questionnaire • Page 1 of 2"
   centered, exactly matching the format/style seen in "Non-Compliant Covenants"
   report's footer ("Non-Compliant Covenants • Page 1 of 3").

Acceptance criteria:
- Generated Checklist Questionnaire PDF shows white date/time text in the header,
  right-aligned, on the navy background.
- Footer shows "Checklist Questionnaire • Page X of Y" centered at the bottom,
  matching the visual style of other CRM reports' footers.
- No other report's header/footer is affected.
