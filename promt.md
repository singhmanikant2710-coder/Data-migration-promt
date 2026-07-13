Apply this edit now. Two files only.

1. backend/src/Casrr.Application/Reviews/Contracts/ReviewFormSaveModels.cs
   Add a save DTO for the scorecard section, carrying the Comments string (HTML). Mirror exactly how the Risk Rating Justification save DTO is defined in this same file.

2. The review save controller (backend/src/Casrr.Api/Controllers/ — the controller that handles the review save endpoint)
   - Accept a "scorecard" section in the save request body.
   - Route it to the service method that saves scorecard comments.
   Mirror exactly how the "riskRatingJustification" section is accepted and routed in the same controller.

Apply and show me the diff. Do not touch any other file.
