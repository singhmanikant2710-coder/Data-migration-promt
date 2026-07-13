Found it. The 400 is NOT coming from the controller guard — it comes from a SECOND guard inside the service:

  Casrr.Domain.Exceptions.DomainValidationException: No changes were provided.
     at Casrr.Application.Services.ReviewService.SaveAsync(ReviewFormSaveRequest dto, ...) 
        in backend/src/Casrr.Application/Services/ReviewService.cs:line 72

Fix: open backend/src/Casrr.Application/Services/ReviewService.cs, look at the validation around line 72 that throws DomainValidationException("No changes were provided"). It counts/checks which sections were supplied and does NOT include dto.Scorecard.

Add dto.Scorecard to that check, exactly the way dto.RiskRatingJustification is handled there.

Also verify the rest of SaveAsync actually routes the scorecard section to SaveScorecardInfoAsync — if the service only throws but never handles scorecard, wire that up too (mirroring how riskRatingJustification is handled in the same method).

Also REMOVE the temporary Console.WriteLine diagnostic logs from ReviewController.cs.

Show me the diff. STOP after applying.
