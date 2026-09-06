Bug 192 — completeness cross-check. READ-ONLY, no edits. Answer, STOP.

We identified 5 PDF components leaking raw HTML in Comments cells: FinalMemoPDF, InitialMemoPDF, CrmFindingsObservationsPDF, ChecklistQuestionnairePDF, ReviewPDF.

Verify NO other component is missed:
1. List ALL PDF components in frontend/src/components/pdf/ that render any rich-text / HTML-bearing field from the DB (comments, information, description, justification, guidance, rationale, key_risks, mitigation, etc. — the fields written by RichTextEditor as innerHTML).
2. For EACH such field render site across ALL PDF components, report whether it: (a) routes through HtmlRichText, (b) applies stripHtml/stripHtmlToText, or (c) passes raw through out()/plain Text (LEAKS).
3. Specifically check these components too (not just the 5): CrmSummaryPDF, CrmSummaryTablePDF, CrmSamplesSummaryPDF, ScorecardResultsPDF, CovenantsSummaryPDF, CovenantViolationsPDF, NonComplianceCovenantsPDF, ManagementSummaryPDF, PolicyExceptionsPDF, CroProductionSummaryPDF, CrmPdGradeMigrationPDF. Do any of these render an HTML-bearing field raw?
4. Confirm the final complete list of files that need the fix — is it exactly 5, or more?

Report each leaking site (file + line + field). Do NOT fix yet.
