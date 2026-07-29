SELECT TOP 5 [Review_id], [eCIF_number], [Customer_number], [Customer_name]
FROM dbo.[02_CORE_02_Reviews] WITH (NOLOCK)
WHERE [Review_finalized_date] IS NOT NULL
ORDER BY [Review_id] DESC;
