SELECT CONVERT(VARCHAR(MAX), CAST(CAST(Help_tip AS NVARCHAR(MAX)) AS VARBINARY(MAX)), 1) AS HexTip
FROM dbo.[03_LIBRARY_06_Help Tips] WHERE Help_tip_topic = 'Checklist Questions';


Fix for #211 script only (do not commit): in scripts/sql/insert-help-tip-checklist-questions.sql replace the help tip text with the exact QA value, as DECLARE @tip NVARCHAR(MAX) = CAST(<hex> AS NVARCHAR(MAX)); keep IF NOT EXISTS / identity handling unchanged. No other file. Hex:
<paste 0x... here>
