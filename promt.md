Apply this edit now. Two files only.

1. backend/src/Casrr.Application/IReviewRepository.cs
   Add this method signature:
   Task SaveScorecardInfoAsync(int reviewId, string? comments, CancellationToken ct);

2. backend/src/Casrr.Infrastructure/SqlServer/SqlReviewRepository.cs
   Implement it:
   UPDATE dbo.[02_CORE_02_Reviews]
   SET [Scorecard_information] = @comments
   WHERE [Review_id] = @reviewId;

Mirror EXACTLY how SaveRiskRatingJustificationAsync is written in the same file (same null-safety, same parameter handling, same connection/command pattern).

Apply and show me the diff. Do not touch any other file.


Apply this edit now. Two files only.

1. backend/src/Casrr.Application/Services/IReviewService.cs
   Add the service method signature for saving scorecard comments.

2. backend/src/Casrr.Application/Services/ReviewService.cs
   Implement it — it should simply delegate to the repository's SaveScorecardInfoAsync.

Mirror EXACTLY how the Risk Rating Justification save is done in these same two files (same signature style, same delegation pattern).

Apply and show me the diff. Do not touch any other file.
