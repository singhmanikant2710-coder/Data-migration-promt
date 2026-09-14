SELECT TABLE_NAME, COLUMN_NAME, DATA_TYPE
FROM INFORMATION_SCHEMA.COLUMNS
WHERE TABLE_SCHEMA = 'dbo'
  AND TABLE_NAME IN ('02_CORE_01_Samples', '02_CORE_02_Reviews', '02_CORE_04_Accounts', '02_CORE_05_Covenants')
  AND DATA_TYPE IN ('date','datetime','datetime2','smalldatetime','datetimeoffset','time')
ORDER BY TABLE_NAME, ORDINAL_POSITION;

SELECT COUNT(*) AS rows_with_time
FROM dbo.[02_CORE_01_Samples] WITH (NOLOCK)
WHERE Created_date IS NOT NULL AND CAST(Created_date AS time) <> '00:00:00';


Hi Geoff, quick confirmation before I make all date fields in these 4 exports Date-Only:
A couple of date columns (like Sample's Created_date, Closed_Date) actually store a real time-of-day, not just midnight. Formatting them as MM-DD-YYYY would drop that time from the export. Is that OK, or should time-bearing columns keep their time?
MM-DD-YYYY is ambiguous in Excel if opened on a non-US regional setting (03-04-2026 could read as March 4 or April 3). Is MM-DD-YYYY specifically required, or would MM/DD/YYYY or a clearer format work? (Just confirming since it affects how other teams might read the file.)
Once confirmed I'll implement — it's a single shared function, so easy to change once we're aligned on the exact format.
