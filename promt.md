SELECT r.[Review_id], r.[Sample_id], r.[Customer_name], r.[Cancelled],
       r.[Completed_date], r.[Review_distributed_date], r.[Review_finalized_date],
       r.[Special_assets], r.[CCL]
FROM dbo.[02_CORE_02_Reviews] r WITH (NOLOCK)
WHERE r.[Review_id] = 21861;

SELECT DISTINCT r.[Sample_id], r.[Sample_name], c.[Review_id], r.[Customer_name], r.[Completed_date]
FROM dbo.[02_CORE_05_Covenants] c WITH (NOLOCK)
INNER JOIN dbo.[02_CORE_02_Reviews] r WITH (NOLOCK) ON r.[Review_id] = c.[Review_id]
WHERE r.[Completed_date] IS NOT NULL
  AND (r.[Cancelled] IS NULL OR r.[Cancelled] = 0)
  AND (
    REPLACE(REPLACE(UPPER(LTRIM(RTRIM(ISNULL(c.[Covenant_last_eval_status], '')))), '-', ''), ' ', '') IN ('NONCOMPLIANT','NOTCOMPLIANT','PASTDUE')
    OR REPLACE(REPLACE(UPPER(LTRIM(RTRIM(ISNULL(c.[Covenant_financial_result], '')))), '-', ''), ' ', '') IN ('NONCOMPLIANT','NOTCOMPLIANT')
  );

  SELECT DISTINCT [Covenant_category], COUNT(*) AS n
FROM dbo.[02_CORE_05_Covenants] WITH (NOLOCK)
GROUP BY [Covenant_category] ORDER BY n DESC;
