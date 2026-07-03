Modify ONLY this file:
backend/src/Casrr.Infrastructure/SqlServer/SqlReviewRepository.cs

IMPORTANT: Use the LIVE database. IGNORE discovery/backend-schema/columns.csv — it is 
outdated. The live DB HAS these bit columns (confirmed, and SaveCrmRatingsAsync already 
writes to them successfully): Risk_recognition_UNSAT, Scorecard_mgmt_UNSAT, 
Underwriting_UNSAT, Credit_servicing_UNSAT, Loan_admin_UNSAT — plus the *_comments columns.

In GetCrmFindingsSectionAsync, fix the ratings READ:

1. REMOVE the interim line: rr = sm = uw = cs = la = null;

2. Add a query to read the 10 columns for this review:
   SELECT [Risk_recognition_UNSAT], [Risk_recognition_comments],
          [Scorecard_mgmt_UNSAT], [Scorecard_mgmt_comments],
          [Underwriting_UNSAT], [Underwriting_comments],
          [Credit_servicing_UNSAT], [Credit_servicing_comments],
          [Loan_admin_UNSAT], [Loan_admin_comments]
   FROM dbo.[02_CORE_02_Reviews] WITH (NOLOCK)
   WHERE [Review_id] = @reviewId;

3. For each component, read the bit. If the UNSAT bit is true, set the rating string 
   variable (rr/sm/uw/cs/la) to "Unsatisfactory"; otherwise "Satisfactory". These feed 
   the existing NormalizeRatingSimple calls, so the frontend checkboxes will show checked 
   when Unsatisfactory.

4. Also capture the 5 *_comments values so they can be returned for the rationale editors. 
   Look at the CrmRatings model returned by this method — if it has per-component comment 
   fields, populate them. If CrmRatings has NO comment fields and adding them requires 
   editing another file (the DTO/model), STOP and tell me first (for now, still return 
   the UNSAT-based rating strings so at least the checkboxes restore correctly).

Match the existing ADO.NET read style (open connection, SqlCommand, reader) used 
elsewhere in this file. Handle the case where the review row has NULLs (treat null bit 
as false → "Satisfactory").

Modify ONLY SqlReviewRepository.cs. If the CrmRatings DTO needs new comment fields in 
another file, STOP and ask.
