Modify ONLY this file:
backend/src/Casrr.Application/Services/ReviewService.cs

In the CRM Findings block inside SaveAsync, AFTER the line 
"await _repo.SaveCrmFindingsAsync(resolvedReviewId, findingRows, ct);" 
and still INSIDE the try block, add CRM Ratings persistence.

The same "data" JsonElement contains:
  - "ratings": { riskRecognition, scorecardManagement, underwriting, creditServicing, 
    loanAdministration } as strings ("Unsatisfactory" or "Satisfactory")
  - "rationales": { general, riskRecognition, scorecardManagement, underwriting, 
    creditServicing, loanAdministration } as HTML strings

Add this logic (reuse the existing local TryGetPropertyIgnoreCase helper):

1. Read the "ratings" object. For each of the 5 components, compute a bool UNSAT:
   true if the string value (trimmed, case-insensitive) equals "unsatisfactory", else false.

2. Read the "rationales" object. For each of the 5 components, get the comment string 
   (per-component: riskRecognition, scorecardManagement, underwriting, creditServicing, 
   loanAdministration). Ignore "general" for now (no DB column).

3. Call:
   await _repo.SaveCrmRatingsAsync(
       resolvedReviewId,
       rrUnsat, rrComments,
       smUnsat, smComments,
       uwUnsat, uwComments,
       csUnsat, csComments,
       laUnsat, laComments,
       ct);

Helper to read a nested string safely: if the property exists and is a String, use 
GetString(); otherwise null. For UNSAT bool: string.Equals(val?.Trim(), "Unsatisfactory", 
StringComparison.OrdinalIgnoreCase).

Only add this after the findings save; do not change the findings logic. Keep it 
inside the same try/catch.

Modify ONLY ReviewService.cs.


edit 4 ------ 

Modify ONLY this file:
backend/src/Casrr.Infrastructure/SqlServer/SqlReviewRepository.cs

In GetCrmFindingsSectionAsync, the 5 ratings are currently set to null (interim). 
Fix the read to load the actual UNSAT flags and comments from dbo.[02_CORE_02_Reviews], 
and return them so the CRM Ratings tab shows saved state.

1. Remove the interim line: rr = sm = uw = cs = la = null;

2. Add a query to read the 10 columns for this review (LIVE DB column names):
   SELECT [Risk_recognition_UNSAT], [Risk_recognition_comments],
          [Scorecard_mgmt_UNSAT], [Scorecard_mgmt_comments],
          [Underwriting_UNSAT], [Underwriting_comments],
          [Credit_servicing_UNSAT], [Credit_servicing_comments],
          [Loan_admin_UNSAT], [Loan_admin_comments]
   FROM dbo.[02_CORE_02_Reviews] WITH (NOLOCK)
   WHERE [Review_id] = @reviewId;

3. Convert each UNSAT bit to the rating string the frontend expects: 
   if UNSAT bit is true → "Unsatisfactory", else → "Satisfactory". 
   Set rr, sm, uw, cs, la accordingly (these feed NormalizeRatingSimple).

4. Also read the per-component comments and include them in the returned CrmRatings / 
   section so the rationale editors can show saved text. If the CrmRatings model 
   doesn't have comment fields, show me the model — if adding fields requires editing 
   another file (DTO), STOP and tell me first.

Match the existing ADO.NET read style in this file (connection, reader). 
Modify ONLY SqlReviewRepository.cs; if the CrmRatings DTO needs new comment fields 
in another file, STOP and ask.
