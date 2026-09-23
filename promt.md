;WITH rm_officers AS (
    SELECT DISTINCT
        TRY_CONVERT(int, LTRIM(RTRIM(d.[OfficerNumber]))) AS [EmployeeId],
        LTRIM(RTRIM(d.[OfficerName])) AS [Name],
        d.[CUST_NUM]
    FROM dbo.[01_DATA_01_Data Mart Trial] AS d WITH (NOLOCK)
    WHERE TRY_CONVERT(int, LTRIM(RTRIM(d.[OfficerNumber]))) IS NOT NULL
)
SELECT TOP 5
    o.[EmployeeId], o.[Name], o.[CUST_NUM]
FROM rm_officers AS o
WHERE NOT EXISTS (
    SELECT 1 FROM dbo.[03_LIBRARY_10_Distribution Parties] AS dp WITH (NOLOCK)
    WHERE TRY_CONVERT(int, LTRIM(RTRIM(dp.[Recipient_role]))) = o.[EmployeeId]
);
