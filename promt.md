READ-ONLY. Do NOT edit. Report only.

CRM Ratings save is not implemented. The 5 ratings are riskRecognition, 
scorecardManagement, underwriting, creditServicing, loanAdministration.

Report ONLY:

1. Where should CRM Ratings be stored? Check dbo.[02_CORE_02_Reviews] columns — do 
   columns exist for these 5 ratings (e.g. Risk_recognition_rating, Scorecard_mgmt_rating, 
   Underwriting_rating, Credit_servicing_rating, Loan_admin_rating, or similar)? 
   List any matching columns from the schema.

2. In the READ path (GetCrmFindingsSectionAsync / wherever ratings are read), how are 
   these 5 ratings currently read? Earlier there was an interim line setting them to 
   null (rr = sm = uw = cs = la = null). Is that still there? Show it.

3. In the payload, dto.CrmFindingsAndRatings.Data contains a "ratings" object with 
   these 5 fields. Confirm the exact property names sent from frontend.

4. Is there any UNSAT-related fields too (the CRM Ratings tab also had Unsat checkboxes 
   and a rationale)? Where would those be stored?

Report only with exact columns, code, and SQL. No edits.
