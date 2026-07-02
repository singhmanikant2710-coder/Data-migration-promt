READ-ONLY. Do NOT edit. Report only. Use live DB, ignore columns.csv.

The CRM Ratings tab (URL section=crm-ratings) is a SEPARATE component from CRM Findings. 
It renders 5 finding-total tiles, a rationale comment box, and 5 UNSAT checkboxes 
(UNSAT Risk Recognition, UNSAT Scorecard Management, UNSAT Underwriting, 
UNSAT Credit Servicing, UNSAT Loan Administration).

Find the CRM RATINGS component file (NOT CrmFindingsAndRatingsSection.tsx). 
Report ONLY:

1. Exact file path of the CRM Ratings tab component.

2. The state fields for each of the 5 UNSAT checkboxes and the comment box(es). 
   Is there ONE shared rationale comment, or 5 separate comments (one per component)? 
   Show the state/hook.

3. On save, what does it put in setSection / the payload? Does it send the 5 UNSAT 
   booleans + comment(s), or nothing currently? Show the setSection calls.

4. How is the section keyed in the payload — under "crmFindingsAndRatings" (shared 
   with findings) or its own section key? 

5. The 5 finding-total tiles (Risk Recognition Findings count etc.) — are these 
   computed from findings, or read from somewhere? (These are display-only, not saved.)

Report only with exact code and file path. No edits.
