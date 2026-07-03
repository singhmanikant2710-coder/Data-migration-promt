Modify ONLY this file:
backend/src/Casrr.Application/IReviewRepository.cs

Add a declaration for SaveCrmRatingsAsync matching the style of the existing 
declarations (like SaveKeyRisksAsync):

// Persist CRM Ratings (5 per-component UNSAT flags + comments) into dbo.[02_CORE_02_Reviews]
Task SaveCrmRatingsAsync(
    int reviewId,
    bool riskRecognitionUnsat, string? riskRecognitionComments,
    bool scorecardMgmtUnsat, string? scorecardMgmtComments,
    bool underwritingUnsat, string? underwritingComments,
    bool creditServicingUnsat, string? creditServicingComments,
    bool loanAdminUnsat, string? loanAdminComments,
    CancellationToken ct);

Modify ONLY IReviewRepository.cs.


read only after build 
READ-ONLY. Do NOT edit. Report only.

In ReviewService.SaveAsync, I need to add CRM Ratings saving inside the existing 
CRM Findings block (section key crmFindingsAndRatings). The frontend sends, in 
dto.CrmFindingsAndRatings.Data:
  - ratings: { riskRecognition, scorecardManagement, underwriting, creditServicing, 
    loanAdministration } as strings ("Unsatisfactory"/"Satisfactory")
  - rationales: { general, riskRecognition, scorecardManagement, underwriting, 
    creditServicing, loanAdministration } as HTML strings

Report ONLY:
1. Show the current CRM Findings block in SaveAsync exactly (where findings are parsed 
   and SaveCrmFindingsAsync is called), so I know where to add the ratings parsing 
   and the SaveCrmRatingsAsync call.
2. Confirm how to read nested objects "ratings" and "rationales" from the same 
   dto.CrmFindingsAndRatings.Data JsonElement (the block already has a 
   TryGetPropertyIgnoreCase helper — show it).

Report only. No edits.
