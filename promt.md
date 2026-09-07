SELECT r.[Sample_id], c.[Review_id], r.[Customer_name], r.[Completed_date],
       c.[Covenant_type], c.[Covenant_category],
       '[' + ISNULL(c.[Covenant_last_eval_status],'<NULL>') + ']' AS Status,
       '[' + ISNULL(c.[Covenant_financial_result],'<NULL>') + ']' AS Result
FROM dbo.[02_CORE_05_Covenants] c WITH (NOLOCK)
INNER JOIN dbo.[02_CORE_02_Reviews] r WITH (NOLOCK) ON r.[Review_id] = c.[Review_id]
WHERE r.[Sample_id] = 354
  AND r.[Completed_date] IS NOT NULL
  AND (r.[Cancelled] IS NULL OR r.[Cancelled] = 0)
  AND (
    REPLACE(REPLACE(UPPER(LTRIM(RTRIM(ISNULL(c.[Covenant_last_eval_status], '')))), '-', ''), ' ', '') IN ('NONCOMPLIANT','NOTCOMPLIANT','PASTDUE')
    OR REPLACE(REPLACE(UPPER(LTRIM(RTRIM(ISNULL(c.[Covenant_financial_result], '')))), '-', ''), ' ', '') IN ('NONCOMPLIANT','NOTCOMPLIANT')
  );
