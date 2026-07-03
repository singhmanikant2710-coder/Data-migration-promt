Modify ONLY this file:
backend/src/Casrr.Domain/ReviewQueue.cs

In the CrmRatings class, ADD 5 per-component comment fields (keep the existing 5 
rating string fields). New class:

public sealed class CrmRatings
{
    public string RiskRecognition { get; init; } = "Satisfactory";
    public string ScorecardManagement { get; init; } = "Satisfactory";
    public string Underwriting { get; init; } = "Satisfactory";
    public string CreditServicing { get; init; } = "Satisfactory";
    public string LoanAdministration { get; init; } = "Satisfactory";

    public string? RiskRecognitionComments { get; init; }
    public string? ScorecardManagementComments { get; init; }
    public string? UnderwritingComments { get; init; }
    public string? CreditServicingComments { get; init; }
    public string? LoanAdministrationComments { get; init; }
}

Modify ONLY ReviewQueue.cs.


EDit 2 

Modify ONLY this file:
backend/src/Casrr.Infrastructure/SqlServer/SqlReviewRepository.cs

In GetCrmFindingsSectionAsync, the ratings read has the *_comments reads commented out. 
Fix it:

1. UNCOMMENT and activate the 5 comment reads (rrComments, smComments, uwComments, 
   csComments, laComments) from the reader — read them as string? (IsDBNull check).
   Declare them OUTSIDE the if-block (like rr/sm/uw/cs/la) so they're in scope for the return.

2. In the returned CrmRatings, populate the new comment fields:
   Ratings = new CrmRatings
   {
       RiskRecognition = NormalizeRatingSimple(rr),
       ScorecardManagement = NormalizeRatingSimple(sm),
       Underwriting = NormalizeRatingSimple(uw),
       CreditServicing = NormalizeRatingSimple(cs),
       LoanAdministration = NormalizeRatingSimple(la),
       RiskRecognitionComments = rrComments,
       ScorecardManagementComments = smComments,
       UnderwritingComments = uwComments,
       CreditServicingComments = csComments,
       LoanAdministrationComments = laComments
   }

Modify ONLY SqlReviewRepository.cs.

EDIT 3 
Modify ONLY these files (frontend CRM Ratings):
1. frontend/src/app/review/[ecif]/review-info/components/sections/hooks/useCrmFindings.ts
2. frontend/src/app/review/[ecif]/review-info/components/sections/CrmRatingsSection.tsx

The backend now returns per-component rating comments in the response at 
form.crmFindings.ratings.{riskRecognitionComments, scorecardManagementComments, 
underwritingComments, creditServicingComments, loanAdministrationComments} 
(camelCase; confirm the exact JSON casing from the response and use it).

FILE 1 (useCrmFindings.ts): In the ratings mapping, also surface the 5 comment values 
so the component can read them. Add them to the ratings object (or a parallel field), e.g.:
  ratings: {
    riskRecognition: normalizeRating(crmFind?.ratings?.riskRecognition),
    ... (existing 5) ...,
    riskRecognitionComments: crmFind?.ratings?.riskRecognitionComments ?? "",
    scorecardManagementComments: crmFind?.ratings?.scorecardManagementComments ?? "",
    underwritingComments: crmFind?.ratings?.underwritingComments ?? "",
    creditServicingComments: crmFind?.ratings?.creditServicingComments ?? "",
    loanAdministrationComments: crmFind?.ratings?.loanAdministrationComments ?? "",
  }
Update the CrmRatings type accordingly.

FILE 2 (CrmRatingsSection.tsx): Initialize the local "rationales" state from the 
backend comments when data loads (e.g. a useEffect that sets rationales from 
state.ratings.*Comments when the section state becomes available), so the rationale 
editors show saved text on reload. Do not break the existing onChange behavior.

Modify ONLY these two files. If the response JSON casing differs, adapt. If the 
CrmRatings frontend type is defined elsewhere and needs editing, STOP and tell me first.
