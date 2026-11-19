#260 – Implemented. Tested in Dev and pushed to the develop branch. Not yet deployed to QA.
What changed (CRM Summary report, CRM FINDINGS & OBSERVATIONS table):
- The COMPONENT column was removed from the PDF. The columns are now CODE | SEVERITY | TYPE | DESCRIPTION.
- SEVERITY shows CORE_Findings.Finding_level (Finding or Observation) and fits on one line.
- Findings remain sorted by CRM Component, then Finding Code.
- The Excel export of this report still includes CRM Component (and Severity).
I will confirm here once deployed to QA.



git add frontend/src/components/pdf/CrmSummaryPDF.tsx
git status
git commit -m "UAT #260: CRM Summary findings table shows CODE | SEVERITY | TYPE | DESCRIPTION (Component removed from PDF, kept in Excel)"
git push origin develop
