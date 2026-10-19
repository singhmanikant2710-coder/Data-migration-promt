SELECT TOP 10 Review_id,
       COUNT(*) AS findings,
       MAX(LEN(CONVERT(nvarchar(max), Finding_Comments))) AS longest_comment
FROM dbo.[02_CORE_07_Findings]
GROUP BY Review_id
ORDER BY longest_comment DESC;
