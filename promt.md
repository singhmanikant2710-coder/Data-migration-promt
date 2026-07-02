READ-ONLY. Do NOT edit. Report only.

CRM Ratings save: DB columns exist in dbo.[02_CORE_02_Reviews]:
Risk_recognition_UNSAT (bit) + Risk_recognition_comments,
Scorecard_mgmt_UNSAT + Scorecard_mgmt_comments,
Underwriting_UNSAT + Underwriting_comments,
Credit_servicing_UNSAT + Credit_servicing_comments,
Loan_admin_UNSAT + Loan_admin_comments.

Report ONLY:

1. In the frontend CRM Ratings tab component and the save payload 
   (dto.CrmFindingsAndRatings.Data), what EXACTLY is sent for ratings now? Is it 
   5 UNSAT booleans + 5 comment strings (matching the checkbox + rationale design), 
   or still the old 5 rating strings (riskRecognition, etc.)? Show the exact payload 
   shape and property names for the ratings/UNSAT section.

2. In the READ path (GetCrmFindingsSectionAsync), the 5 ratings are currently set to 
   null (interim). To display UNSAT + comments correctly, the read needs to SELECT 
   these 10 columns. Show the current read code around the ratings so I know what to 
   change.

3. What is the exact frontend field/state for each UNSAT checkbox and its comment 
   in the CRM Ratings tab component?

Report only with exact code and property names. No edits.
