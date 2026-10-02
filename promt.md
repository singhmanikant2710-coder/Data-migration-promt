Reports page: PDF download file names are inconsistent. Some are Proper
Case ("CRM Summary Table_9.21.2026"), most are lower-case-hyphenated
("cro-review-production_9.21.2026"). Make ALL reports use Proper Case with
spaces, no hyphens, keeping the existing date suffix format
"_M.D.YYYY". Use the report's display title as the name, e.g.:
  cro-review-production       → CRO Review Production_9.21.2026
  non-compliant-covenants     → Non-Compliant Covenants_9.21.2026
  policy-exceptions           → Credit Policy Exceptions_9.21.2026
  crm-pd-grade-migration      → CRM PD Grade Migration_9.21.2026
  crm-scorecard-results       → CRM Scorecard Results_9.21.2026
  crm-findings-observations   → CRM Findings and Observations_9.21.2026
  crm-summary                 → CRM Summary_9.21.2026
  checklist-questionnaire     → Checklist Questionnaire_9.21.2026
(Keep the hyphen inside proper names like "Non-Compliant".)

Fix generically: one shared helper that builds the file name from the
report's display title + date, used by every report download. Reports that
are already correct must keep their exact current name. Strip characters
that are invalid in Windows file names.

Constraints: only the download file name changes. Don't change report
content, routes or report IDs.

Report ADDED/REMOVED (file:line), a table of every report's file name
before/after, and the case where a title contains an invalid character.
Build result. Do not commit.
