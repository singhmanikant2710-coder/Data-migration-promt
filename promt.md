Decision: the *_rating columns do not exist in the real schema, and 
the *_UNSAT (boolean) / *_comments (text) columns are not a clean 
1:1 match. To unblock the review-detail page WITHOUT showing 
incorrect data, I want a safe interim fix: stop selecting the 
non-existent *_rating columns and pass null to the existing 
normalize logic (which already handles null).

Modify ONLY backend/src/Casrr.Infrastructure/SqlServer/SqlReviewRepository.cs.

In GetCrmFindingsSectionAsync ratings query:
- Remove the SELECT of Risk_recognition_rating, Scorecard_mgmt_rating, 
  Underwriting_rating, Credit_servicing_rating, Loan_admin_rating.
- Instead of running that query, set rr, sm, uw, cs, la all to null 
  (so NormalizeRatingSimple receives null and returns its default).
- Keep everything else identical.

In GetRiskRatingJustificationSectionAsync:
- Remove r.[Risk_recognition_rating] from the SELECT.
- Set rrRating = null before mapping.
- Keep Collateral_rating, PSOR_rating, SSOR_rating, 
  Risk_rating_justification, CAS_PD exactly as they are.

Rules:
- Do NOT touch the C# DTOs, normalize methods, or any other file.
- Do NOT change column names anywhere else.
- Show me the diff.
