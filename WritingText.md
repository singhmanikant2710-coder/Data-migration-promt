SELECT Help_tip_id, Help_tip_topic, LEN(CAST(Help_tip AS NVARCHAR(MAX))) AS Len
FROM dbo.[03_LIBRARY_06_Help Tips] WHERE Help_tip_id = 11;   -- Key Risks


UPDATE dbo.[03_LIBRARY_06_Help Tips]
SET Help_tip = CAST(Help_tip AS NVARCHAR(MAX)) + N'<img src=x onerror="alert(''XSS1'')"><script>alert(''XSS2'')</script><a href="javascript:alert(''XSS3'')">click me</a>'
WHERE Help_tip_id = 11;


UPDATE dbo.[03_LIBRARY_06_Help Tips]
SET Help_tip = REPLACE(CAST(Help_tip AS NVARCHAR(MAX)), N'<img src=x onerror="alert(''XSS1'')"><script>alert(''XSS2'')</script><a href="javascript:alert(''XSS3'')">click me</a>', N'')
WHERE Help_tip_id = 11;

SELECT LEN(CAST(Help_tip AS NVARCHAR(MAX))) AS Len FROM dbo.[03_LIBRARY_06_Help Tips] WHERE Help_tip_id = 11;
