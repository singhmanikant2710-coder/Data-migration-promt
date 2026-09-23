;WITH rm_officers AS (
    SELECT DISTINCT
        TRY_CONVERT(int, LTRIM(RTRIM(d.[OfficerNumber]))) AS [EmployeeId],
        LTRIM(RTRIM(d.[OfficerName])) AS [Name]
    FROM dbo.[01_DATA_01_Data Mart Trial] AS d WITH (NOLOCK)
    WHERE TRY_CONVERT(int, LTRIM(RTRIM(d.[OfficerNumber]))) IS NOT NULL
),
pm_officers AS (
    SELECT DISTINCT
        TRY_CONVERT(int, LTRIM(RTRIM(d.[PM Number]))) AS [EmployeeId],
        LTRIM(RTRIM(d.[PMName])) AS [Name]
    FROM dbo.[01_DATA_01_Data Mart Trial] AS d WITH (NOLOCK)
    WHERE TRY_CONVERT(int, LTRIM(RTRIM(d.[PM Number]))) IS NOT NULL
)

SELECT * FROM (
    SELECT TOP (10)
        'Relationship Manager' AS [Field],
        o.[EmployeeId],
        o.[Name] AS [DataMartName]
    FROM rm_officers AS o
    WHERE NOT EXISTS (
        SELECT 1 FROM dbo.[03_LIBRARY_10_Distribution Parties] AS dp WITH (NOLOCK)
        WHERE TRY_CONVERT(int, LTRIM(RTRIM(dp.[Recipient_role]))) = o.[EmployeeId]
    )
    ORDER BY o.[EmployeeId]
) AS RmMismatches

UNION ALL

SELECT * FROM (
    SELECT TOP (10)
        'Portfolio Manager',
        o.[EmployeeId],
        o.[Name]
    FROM pm_officers AS o
    WHERE NOT EXISTS (
        SELECT 1 FROM dbo.[03_LIBRARY_10_Distribution Parties] AS dp WITH (NOLOCK)
        WHERE TRY_CONVERT(int, LTRIM(RTRIM(dp.[Recipient_role]))) = o.[EmployeeId]
    )
    ORDER BY o.[EmployeeId]
) AS PmMismatches;
