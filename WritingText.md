SELECT TOP 5 r.Review_id, r.Sample_id, r.Completed_date, r.CRO_email, r.CRO_manager_email
FROM dbo.[02_CORE_02_Reviews] r
JOIN dbo.[02_CORE_01_Samples] s ON s.Sample_id = r.Sample_id
WHERE ISNULL(r.Locked, 0) = 0
  AND ISNULL(r.Cancelled, 0) = 0
  AND ISNULL(s.Closed, 0) = 0
  AND r.Completed_date IS NULL
  AND r.Review_approval_date IS NULL
  AND ISNULL(r.CRO_manager_email, '') <> '';

  http://localhost:3100/review/id-<RID>/review-info?section=review-info&reviewId=<RID>&sampleId=<SID>


  
SELECT Review_id, Completed_date FROM dbo.[02_CORE_02_Reviews] WHERE Review_id = <RID>;


SELECT * FROM dbo.[02_CORE_01_Samples] WHERE Sample_id = 354;


SELECT Review_id, Completed_date, Review_approval_date, Locked
FROM dbo.[02_CORE_02_Reviews] WHERE Review_id = 21587;
