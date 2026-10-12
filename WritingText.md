SELECT Help_tip_id, Help_tip_topic, LEN(CAST(Help_tip AS NVARCHAR(MAX))) AS Len
FROM dbo.[03_LIBRARY_06_Help Tips] WHERE Help_tip_id = 11;   -- Key Risks
