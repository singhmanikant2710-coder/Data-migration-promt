Do NOT edit anything yet. I need to decide the correct mapping for 
the missing *_rating columns.

In GetCrmFindingsSectionAsync and GetRiskRatingJustificationSectionAsync, 
show me:
1. The full SQL of the "ratings query" that selects Risk_recognition_rating, 
   Scorecard_mgmt_rating, Underwriting_rating, Credit_servicing_rating, 
   Loan_admin_rating.
2. How each selected value is read by the C# reader (ordinal or name) 
   and which DTO property / field it maps to.
3. How that DTO field is used downstream (what type is it — string? 
   bool? an enum?) and where it surfaces in the API response that the 
   frontend Review detail page consumes.

I want to know whether each "rating" is displayed as free-text 
(→ map to *_comments) or as a satisfactory/unsatisfactory status 
(→ map to *_UNSAT). Report this so I can choose the correct column 
for each of the 5 fields.
