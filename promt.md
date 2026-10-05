Subject: CASRR – UAT items completed and pushed; DB scripts to review and execute before testing

Hi Team,

All new functionality and fixes from the current UAT list have been implemented as per the business requirements, tested locally/Dev, and the changes are pushed. Please review and execute the database scripts below in QA and Prod, and then we can start testing.

Completed items
- #178  Review Form return button goes back to the originating list (Queue / Progress / History) with filters kept
- #193 / #194  Rich text tables and pasted images keep their size in PDFs; pasted Word tables/images stay inside the field on screen
- #200 / #201  Customer # column added to Review Queue and Review Progress
- #203  Application renamed to "Credit Assurance Services RiskReview (CASRR)" and new logo
- #208 / #209  TTBA labels renamed (Household Exposure, RCA Approver, RCA Approval Authority, Approval Reason) on CAS Linesheet, Review Form and CRM Summary PDF
- #210  CRM Findings "Add Row" renamed to "+ Add Finding"
- #211  Checklist Questions help tip linked
- #212  Distributed / Finalized dates locked until Mgr Approval date is entered (plus a fix for date-only fields showing one day earlier)
- #215  Commitment column added to CRM Findings and Observations and CRM Findings for Management
- #217  PD Grade Migration matrix font size, row height and alignment
- #224  Loan Codes – Code column font matches Category
- #226  Samples: Type list from Selections library, "Target" label, Selection ID order, one-time "+ Add Target"
- #229  Load Samples: one review per customer when Data Mart attributes vary (fixes the duplicate/empty review issue)
- #231  Covenant accuracy fields: Green for Yes, Red for No

Scripts to execute (location: scripts/sql/ in the repo)
Please review each script before running. Each one ends with a verification SELECT. Some have already been executed in Dev; none have been executed in Prod, and some are pending in QA.

Run in this order (QA, then Prod):
1. create-distribution-parties-upload-objects.sql
   – staging and audit tables for the Distribution Parties upload (Admin)
2. insert-crm-summary-for-management-selection.sql
3. insert-crm-findings-for-management-selection.sql
   – adds these reports to the Reports dropdown
4. insert-samples-type-selections.sql
   – Samples "Type" options (Continuous, Examination, CCL, Co-Source, Other) in the Selections library
5. insert-help-tip-checklist-questions.sql
   – Checklist Questions help tip (exact copy of the QA row; inserts only if missing, so it is a no-op in QA)

Report only – review the output, do not delete automatically:
6. find-duplicate-reviews-per-sample-customer.sql
   – lists duplicate reviews created by the old Load Samples issue (#229). In QA we expect review IDs 21957 and 21942 (0 accounts). The clean-up section is commented out and should only be run manually after the output has been reviewed and agreed.

Do NOT run in QA / Prod:
- truncate-all-tables-safe.sql (Dev utility only)
- check-customer-info-people-columns.sql, check-distribution-parties-id-match.sql (read-only diagnostics, not required)

Please also confirm:
- alter-data-mart-trial-identifiers.sql is already applied in QA/Prod (shared earlier).
- The unique index on Recipient_role (Distribution Parties) exists in Prod.

Data Mart
- John – after the new build is deployed, please re-upload the 8/31 Data Mart file so the CompCallCode values are loaded correctly.

Once the scripts are executed, please reply with the verification results (or any errors) and we will start testing.

Thanks,
Manikant
