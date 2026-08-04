-- Reviews jo "Completed" hain PAR approval_date NULL (pehle report inhe drop kar deta tha)
SELECT COUNT(*) AS CompletedButNotApproved
FROM dbo.[02_CORE_02_Reviews] r
INNER JOIN dbo.[02_CORE_04_Accounts] a ON a.[Review_id] = r.[Review_id]
WHERE (r.[Cancelled] IS NULL OR r.[Cancelled] = 0)
  AND r.[Sample_id] = @sampleId   -- koi sample jisme aise reviews hon
  AND r.[Completed_date] IS NOT NULL
  AND r.[Review_approval_date] IS NULL
  AND a.[Bank_PD] BETWEEN 1 AND 16
  AND a.[CAS_PD] BETWEEN 1 AND 16;


  -- "Completed" status pe kitne reviews aane chahiye (PD Grade Migration ki new logic)
-- Ye Review Status screen ke "Completed/Draft" bucket se match karna chahiye
SELECT COUNT(*) AS StatusDrivenCount
FROM dbo.[02_CORE_02_Reviews] r
INNER JOIN dbo.[02_CORE_04_Accounts] a ON a.[Review_id] = r.[Review_id]
WHERE (r.[Cancelled] IS NULL OR r.[Cancelled] = 0)
  AND r.[Sample_id] = @sampleId
  AND r.[Completed_date] IS NOT NULL   -- Completed status predicate
  AND a.[Bank_PD] BETWEEN 1 AND 16
  AND a.[CAS_PD] BETWEEN 1 AND 16;
