SELECT c.[Review_id],
       c.[Covenant_type],
       c.[Covenant_category],
       '[' + ISNULL(c.[Covenant_last_eval_status], '<NULL>') + ']' AS RawStatus,
       '[' + ISNULL(c.[Covenant_financial_result], '<NULL>') + ']' AS RawResult,
       REPLACE(REPLACE(UPPER(LTRIM(RTRIM(ISNULL(c.[Covenant_last_eval_status], '')))), '-', ''), ' ', '') AS NormStatus,
       REPLACE(REPLACE(UPPER(LTRIM(RTRIM(ISNULL(c.[Covenant_financial_result], '')))), '-', ''), ' ', '') AS NormResult
FROM dbo.[02_CORE_05_Covenants] c WITH (NOLOCK)
WHERE c.[Review_id] IN (21760, 21761, 21762);
