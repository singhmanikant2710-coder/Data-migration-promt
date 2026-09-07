SELECT 
    REPLACE(REPLACE(UPPER(LTRIM(RTRIM(ISNULL(c.[Covenant_last_eval_status], '')))), '-', ''), ' ', '') AS NormStatus,
    REPLACE(REPLACE(UPPER(LTRIM(RTRIM(ISNULL(c.[Covenant_financial_result], '')))), '-', ''), ' ', '') AS NormResult,
    COUNT(*) AS n
FROM dbo.[02_CORE_05_Covenants] c WITH (NOLOCK)
WHERE REPLACE(REPLACE(UPPER(LTRIM(RTRIM(ISNULL(c.[Covenant_last_eval_status], '')))), '-', ''), ' ', '') IN ('NONCOMPLIANT','NOTCOMPLIANT','PASTDUE')
   OR REPLACE(REPLACE(UPPER(LTRIM(RTRIM(ISNULL(c.[Covenant_financial_result], '')))), '-', ''), ' ', '') IN ('NONCOMPLIANT','NOTCOMPLIANT')
GROUP BY 
    REPLACE(REPLACE(UPPER(LTRIM(RTRIM(ISNULL(c.[Covenant_last_eval_status], '')))), '-', ''), ' ', ''),
    REPLACE(REPLACE(UPPER(LTRIM(RTRIM(ISNULL(c.[Covenant_financial_result], '')))), '-', ''), ' ', '');
