Context: This is a .NET 8 Clean Architecture backend + Next.js/React/TypeScript frontend
project (CASRR — Credit Analysis and Risk Review System). I need to complete an
already-scaffolded report called "Checklist Questionnaire" (Selection_id 10).

IMPORTANT: Do not modify, remove, or refactor any existing working report pipeline,
routing logic, or shared services. Only touch the files listed below (plus any file
you must inspect to confirm existing patterns — but do not alter them unless it is
the specific file described).

What already exists (do not rebuild):
- Frontend PDF component: ChecklistQuestionnairePDF.tsx (~408 lines) — layout is already built.
- DTO: ChecklistQuestionnaireModels.cs — shape is already defined.
- Routing is already wired (dropdown → PDF download flow works end-to-end).
- Backend endpoint exists, but the repository SqlChecklistQuestionnaireReportRepository.cs
  (41 lines) is a stub — it currently just returns an empty array.

Task 1 — Backend (main work):
Implement the real SQL query inside SqlChecklistQuestionnaireReportRepository.cs.
- Primary data source table: 02_CORE_08_Checklists
  Columns needed: Sample_id, Review_id, Checklist_category, Checklist_question,
  Checklist_guidance, Checklist_answer, Checklist_comments
- Join to Samples / Reviews / Accounts (follow the same join pattern used by other
  existing CRM report repositories in this codebase) to pull client-info fields:
  SampleName, CustomerName, Unit, Market, RM, PM, Portfolio attributes.
- Apply the same standard filter-parameter pattern used by the other CRM reports in
  this repo (maximum flexibility filters — match existing convention, don't invent a
  new one).
- Return data shaped to match ChecklistQuestionnaireModels.cs exactly — do not change
  the DTO shape.

Task 2 — Frontend (small changes to ChecklistQuestionnairePDF.tsx):
- Remove the "Section" column from the table (the DB has "Category", not "Section" —
  there is no Section data to show).
- Remove the "Guidance" column from the report output (Checklist_guidance is in-app
  help text only — it should NOT appear in the generated PDF, even though it exists
  in the source table).
- Final table columns must be exactly: CATEGORY | QUESTION | RESPONSE | COMMENTS
- Wire the filter payload: the `meta` object currently does not pass `filters` to the
  backend — add that wiring so filters selected in the UI are sent through, matching
  how other working CRM reports do it.

Acceptance criteria:
- Selecting "Checklist Questionnaire" from the report dropdown and clicking Generate
  produces a real PDF with live data from 02_CORE_08_Checklists, filtered correctly,
  showing only CATEGORY / QUESTION / RESPONSE / COMMENTS columns.
- No other report's behavior changes.
