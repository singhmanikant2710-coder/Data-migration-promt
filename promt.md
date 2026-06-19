Create 2 new backend files for Review History. Do NOT modify any 
existing controller, service, or repository file.

CREATE these 2 new files only:

1. backend/src/Casrr.Application/Services/ReviewHistoryService.cs

- Define interface IReviewHistoryService in the same file or a 
  separate IReviewHistoryService.cs:
  Task<IReadOnlyList<ReviewHistoryRow>> GetHistoryAsync(
    string? sampleName, string? borrowerName, CancellationToken ct);

- Implementation class ReviewHistoryService : IReviewHistoryService
  - Constructor injects IReviewHistoryRepository
  - GetHistoryAsync simply calls 
    _repository.GetHistoryRowsAsync(sampleName, borrowerName, ct) 
    and returns the result
  - Follow the same simple delegation pattern as ReviewService.cs 
    uses for calling IReviewRepository

2. backend/src/Casrr.Api/Controllers/ReviewHistoryController.cs

- [ApiController]
- [Route("api/v1/review-history")]
- [Authorize(Policy = "RequireActiveUser")]
- Inherits BaseTemplateController (same as ReviewController.cs)
- Constructor injects IReviewHistoryService and IGraphUserInfoProvider 
  (pass graphUserInfoProvider to base, same pattern as ReviewController.cs)
- One GET endpoint:
  [HttpGet]
  [ProducesResponseType(StatusCodes.Status200OK, Type = typeof(IReadOnlyList<ReviewHistoryRow>))]
  [ProducesResponseType(StatusCodes.Status500InternalServerError, Type = typeof(ProblemDetails))]
  public async Task<ActionResult<IReadOnlyList<ReviewHistoryRow>>> GetReviewHistory(
    [FromQuery] string? sampleName, 
    [FromQuery] string? borrowerName, 
    CancellationToken ct)
  {
      var response = await _svc.GetHistoryAsync(sampleName, borrowerName, ct);
      return Ok(response);
  }

Follow the exact same error handling pattern as ReviewController.cs 
(if it uses try/catch with ProblemDetails, use the same here).

IMPORTANT RULES:
- Do not modify ReviewController.cs, ReviewService.cs, 
  IReviewRepository.cs, SqlReviewRepository.cs, or 
  IReviewHistoryRepository.cs/SqlReviewHistoryRepository.cs
- Do not modify StartupExtensions.cs yet (next step)

Show me both files when done.
