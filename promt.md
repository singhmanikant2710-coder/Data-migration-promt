SELECT 
    r.[Sample_id],
    r.[Sample_name],
    c.[Review_id],
    r.[Customer_name],
    c.[Covenant_type],
    c.[Covenant_category],
    c.[Covenant_last_eval_status] AS Status,
    c.[Covenant_financial_result] AS Result
FROM dbo.[02_CORE_05_Covenants] c WITH (NOLOCK)
INNER JOIN dbo.[02_CORE_02_Reviews] r WITH (NOLOCK) ON r.[Review_id] = c.[Review_id]
WHERE REPLACE(REPLACE(UPPER(LTRIM(RTRIM(ISNULL(c.[Covenant_last_eval_status], '')))), '-', ''), ' ', '') IN ('NONCOMPLIANT','NOTCOMPLIANT','PASTDUE')
   OR REPLACE(REPLACE(UPPER(LTRIM(RTRIM(ISNULL(c.[Covenant_financial_result], '')))), '-', ''), ' ', '') IN ('NONCOMPLIANT','NOTCOMPLIANT');
