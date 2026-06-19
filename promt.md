Create new backend files for Review History. Do NOT modify any 
existing file (ReviewController.cs, ReviewService.cs, 
IReviewRepository.cs, SqlReviewRepository.cs must remain untouched).

CREATE these 2 new files only:

1. backend/src/Casrr.Application/IReviewHistoryRepository.cs

Interface with one method:
Task<IReadOnlyList<ReviewHistoryRow>> GetHistoryRowsAsync(
  string? sampleName, string? borrowerName, CancellationToken ct);

Also define this DTO in the same file or a new file 
backend/src/Casrr.Application/ReviewHistoryRow.cs:

public class ReviewHistoryRow
{
    public int ReviewId { get; set; }
    public int SampleId { get; set; }
    public string? SampleName { get; set; }
    public string? ECifNumber { get; set; }
    public string? CustomerName { get; set; }
    public string? ReviewerName { get; set; }
    public decimal? Exposure { get; set; }
    public int? BankPD { get; set; }
    public int? CasPD { get; set; }
    public DateTime? CompletedDate { get; set; }
    public DateTime? ReviewFinalizedDate { get; set; }
}

Note: I need the Sample table to have a "Sample_name" or similar 
column to join and get SampleName — check the Samples table 
(referenced in SqlReviewRepository.cs as db.Samples) for the 
exact column name before finalizing this DTO. If a clean join 
isn't available, leave SampleName mapping as a TODO comment 
and I will confirm separately.

2. backend/src/Casrr.Infrastructure/SqlServer/SqlReviewHistoryRepository.cs

Implements IReviewHistoryRepository.GetHistoryRowsAsync using 
raw SQL via SqlCommand/Dapper, following the EXACT same pattern 
as GetQueueRowsAsync in SqlReviewRepository.cs (same connection 
handling, same WITH (NOLOCK), same error handling/logging style).

SQL query:

SELECT r.[Review_id], r.[Sample_id], r.[eCIF_number], 
       r.[Customer_name], r.[Relationship_mgr_name], 
       r.[TTBA_exposure], r.[Bank_PD], r.[CAS_PD], 
       r.[Completed_date], r.[Review_finalized_date]
FROM dbo.[02_CORE_02_Reviews] AS r WITH (NOLOCK)
WHERE r.[Review_finalized_date] IS NOT NULL
  AND (r.[Cancelled] IS NULL OR r.[Cancelled] = 0)
  AND (@borrowerName IS NULL OR r.[Customer_name] LIKE '%' + @borrowerName + '%')
ORDER BY r.[Review_finalized_date] DESC, r.[Review_id] DESC;

For sampleName filter: check if a join to the Samples table is 
needed to filter/display Sample_name (since Sample_name isn't a 
column on the Reviews table itself, based on what we've seen). 
Add the appropriate JOIN to dbo.Samples (or whatever the actual 
table name is, check db.Samples mapping) ON r.[Sample_id] = 
Samples.Sample_id, and select Samples.Sample_name AS SampleName.
Apply sampleName filter in WHERE if provided.

IMPORTANT RULES:
- Do not modify ReviewController.cs, ReviewService.cs, 
  IReviewRepository.cs, or SqlReviewRepository.cs
- Do not modify StartupExtensions.cs yet (DI registration is 
  a separate step)
- Follow the exact same connection/error-handling/logging 
  pattern as SqlReviewRepository.cs

Show me both files when done, and tell me what you found about 
the Samples table column name for sample name.
