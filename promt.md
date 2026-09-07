SELECT r.[Sample_id], c.[Review_id], r.[Customer_name], r.[Completed_date], r.[Cancelled],
       c.[Covenant_type], c.[Covenant_category],
       '[' + ISNULL(c.[Covenant_last_eval_status],'<NULL>') + ']' AS Status,
       '[' + ISNULL(c.[Covenant_financial_result],'<NULL>') + ']' AS Result
FROM dbo.[02_CORE_05_Covenants] c WITH (NOLOCK)
INNER JOIN dbo.[02_CORE_02_Reviews] r WITH (NOLOCK) ON r.[Review_id] = c.[Review_id]
WHERE r.[Sample_id] = 357
ORDER BY c.[Review_id];
