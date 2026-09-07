SELECT c.[Covenant_last_eval_status], c.[Covenant_financial_result], COUNT(*) AS n
FROM dbo.[02_CORE_05_Covenants] c WITH (NOLOCK)
INNER JOIN dbo.[02_CORE_02_Reviews] r WITH (NOLOCK) ON r.[Review_id] = c.[Review_id]
WHERE r.[Sample_id] = <apna sample id>
GROUP BY c.[Covenant_last_eval_status], c.[Covenant_financial_result]
ORDER BY n DESC;
