SELECT Review_id, Finding_code, LEN(CONVERT(nvarchar(max), Finding_Comments)) AS comment_len
FROM dbo.[02_CORE_07_Findings]
WHERE Review_id = 21795;
