Files (READ-ONLY, do NOT edit): 
- backend ReviewStatusController / ReviewStatusService / SqlReviewStatusRepository.cs
- frontend/src/app/review-status/page.tsx

Report in plain text only:

1. When a sample is selected in review-status, what exact value does the frontend 
   send as sampleId to GET /api/v1/reviews/status? Is it the Samples.Sample_id 
   (small internal id like 136) or the leading number from Sample_name (like 357)?

2. In SqlReviewStatusRepository.cs, show how sampleId is used to filter:
   a) the count buckets (the EF Core/LINQ over Reviews), and
   b) the "Completed Draft Reviews" ADO.NET query.
   Specifically: does it filter by Reviews.Sample_id = @sampleId (numeric match), 
   and is @sampleId the small id (136) that won't match Reviews (which store 357)?

3. Does the review-status sample dropdown get its id from Samples.Sample_id, or 
   does it (or could it) use the leading number parsed from Sample_name?

4. Does 02_CORE_02_Reviews have a Sample_name column usable for filtering (like 
   review-history now uses), so we could filter by parsed sample number or by 
   Sample_name instead of the mismatched Sample_id?

Report only. No edits.
