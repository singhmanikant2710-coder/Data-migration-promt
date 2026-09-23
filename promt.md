SELECT
    'RM' AS [Field],
    17436 AS [EmployeeId],
    dp.Recipient_name,
    dp.Recipient_email
FROM dbo.[03_LIBRARY_10_Distribution Parties] AS dp WITH (NOLOCK)
WHERE TRY_CONVERT(int, LTRIM(RTRIM(dp.Recipient_role))) = 17436

UNION ALL

SELECT
    'PM',
    41249,
    dp.Recipient_name,
    dp.Recipient_email
FROM dbo.[03_LIBRARY_10_Distribution Parties] AS dp WITH (NOLOCK)
WHERE TRY_CONVERT(int, LTRIM(RTRIM(dp.Recipient_role))) = 41249;


SELECT TOP 5
    'Matched RM example' AS [Type],
    d.OfficerNumber AS [EmployeeId],
    d.OfficerName AS [DataMartName],
    dp.Recipient_name AS [DistributionPartiesName]
FROM dbo.[01_DATA_01_Data Mart Trial] AS d WITH (NOLOCK)
INNER JOIN dbo.[03_LIBRARY_10_Distribution Parties] AS dp WITH (NOLOCK)
    ON TRY_CONVERT(int, LTRIM(RTRIM(dp.Recipient_role))) = TRY_CONVERT(int, LTRIM(RTRIM(d.OfficerNumber)))
WHERE TRY_CONVERT(int, LTRIM(RTRIM(d.OfficerNumber))) IS NOT NULL;

SELECT Review_id, Relationship_mgr_number, Relationship_mgr_name, Relationship_mgr_email,
       Portfolio_mgr_number, Portfolio_mgr_name, Portfolio_mgr_email
FROM dbo.[02_CORE_02_Reviews]
WHERE Review_id = 21592;
