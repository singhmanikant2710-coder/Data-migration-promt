READ-ONLY. Diagnostics only. Do NOT change anything.

Files: frontend/src/app/reports/page.tsx and frontend/src/components/pdf/

Geoff clarified these 4 reports may NOT have been fully built by Krishnan before departure. I need to verify each one's ACTUAL status. Show me ONLY (no edits):

1. UNSATISFACTORY TRANSACTIONAL RATINGS:
   - Is there a PDF component for it (search components/pdf/ for "Unsatisfactory")?
   - In reports/page.tsx, is it in the dropdown, and does onGeneratePdf have a detector + run for it, or does it fall through to "not implemented"?

2. NON-COMPLIANT COVENANTS (aka Covenant Violations):
   - There are imports for BOTH NonCompliantCovenantsDocument AND CovenantViolationsPDF. Show both components' status — are they real implementations or stubs/placeholders? Show the first ~30 lines of each file.
   - How does the dropdown route to them?

3. CRM SUMMARY FOR MANAGEMENT (Findings Only) / "Review Summary" (10_Review Summary):
   - Is there any "management summary" report? (ManagementSummaryPDF was imported.) Is it built or stub? Show its status and whether it's a findings-only variant.

4. CRM FINDINGS FOR MANAGEMENT (Findings Only) (04_CRM Findings):
   - Is there a findings-only / "for management" variant of CRM Findings, separate from the standard CRM Findings and Observations? Or only the standard one?

For each: BUILT (real component + wired + data), PARTIAL (component exists but stub/not wired), or NOT BUILT (missing). Show evidence (file existence, first lines, routing).

Read once. Findings only. No edits. I need accurate per-report status to respond to Geoff about what still needs building.
