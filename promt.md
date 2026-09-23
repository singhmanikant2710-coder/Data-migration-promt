;WITH cust AS (
    SELECT
        LTRIM(RTRIM(d.[CUST_NUM])) AS [CustNum],
        TRY_CONVERT(int, MIN(LTRIM(RTRIM(d.[OfficerNumber])))) AS [RmId],
        MIN(LTRIM(RTRIM(d.[OfficerName]))) AS [RmName],
        TRY_CONVERT(int, MIN(LTRIM(RTRIM(d.[PM Number])))) AS [PmId],
        MIN(LTRIM(RTRIM(d.[PMName]))) AS [PmName],
        SUM(TRY_CONVERT(decimal(38,10), NULLIF(d.[Commitment], ''))) AS [TotalCommitted]
    FROM dbo.[01_DATA_01_Data Mart Trial] AS d WITH (NOLOCK)
    WHERE NULLIF(LTRIM(RTRIM(d.[CUST_NUM])), '') IS NOT NULL
      AND d.[SourceSystem] IN ('ACBS', 'MWS', 'IFL')
    GROUP BY LTRIM(RTRIM(d.[CUST_NUM]))
)

-- RM: unmatched, grouped, with count + exposure subtotal
SELECT
    'Relationship Manager' AS [Field],
    cust.[RmId] AS [EmployeeId],
    cust.[RmName] AS [DataMartName],
    COUNT_BIG(*) AS [CustomerCount],
    SUM(cust.[TotalCommitted]) AS [CommittedExposure]
FROM cust
WHERE cust.[RmId] IS NOT NULL
  AND NOT EXISTS (
      SELECT 1 FROM dbo.[03_LIBRARY_10_Distribution Parties] AS dp WITH (NOLOCK)
      WHERE TRY_CONVERT(int, LTRIM(RTRIM(dp.[Recipient_role]))) = cust.[RmId]
  )
GROUP BY cust.[RmId], cust.[RmName]

UNION ALL

-- PM: unmatched, grouped, with count + exposure subtotal
SELECT
    'Portfolio Manager',
    cust.[PmId],
    cust.[PmName],
    COUNT_BIG(*),
    SUM(cust.[TotalCommitted])
FROM cust
WHERE cust.[PmId] IS NOT NULL
  AND NOT EXISTS (
      SELECT 1 FROM dbo.[03_LIBRARY_10_Distribution Parties] AS dp WITH (NOLOCK)
      WHERE TRY_CONVERT(int, LTRIM(RTRIM(dp.[Recipient_role]))) = cust.[PmId]
  )
GROUP BY cust.[PmId], cust.[PmName]

ORDER BY [Field], [CommittedExposure] DESC;



;WITH cust AS (
    SELECT
        LTRIM(RTRIM(d.[CUST_NUM])) AS [CustNum],
        TRY_CONVERT(int, MIN(LTRIM(RTRIM(d.[OfficerNumber])))) AS [RmId],
        TRY_CONVERT(int, MIN(LTRIM(RTRIM(d.[PM Number])))) AS [PmId],
        SUM(TRY_CONVERT(decimal(38,10), NULLIF(d.[Commitment], ''))) AS [TotalCommitted]
    FROM dbo.[01_DATA_01_Data Mart Trial] AS d WITH (NOLOCK)
    WHERE NULLIF(LTRIM(RTRIM(d.[CUST_NUM])), '') IS NOT NULL
      AND d.[SourceSystem] IN ('ACBS', 'MWS', 'IFL')
    GROUP BY LTRIM(RTRIM(d.[CUST_NUM]))
)
SELECT
    SUM(CASE WHEN cust.[RmId] IS NOT NULL AND NOT EXISTS (
            SELECT 1 FROM dbo.[03_LIBRARY_10_Distribution Parties] AS dp WITH (NOLOCK)
            WHERE TRY_CONVERT(int, LTRIM(RTRIM(dp.[Recipient_role]))) = cust.[RmId]
        ) THEN 1 ELSE 0 END) AS [Rm_UnmatchedCustomers],
    SUM(CASE WHEN cust.[RmId] IS NOT NULL AND NOT EXISTS (
            SELECT 1 FROM dbo.[03_LIBRARY_10_Distribution Parties] AS dp WITH (NOLOCK)
            WHERE TRY_CONVERT(int, LTRIM(RTRIM(dp.[Recipient_role]))) = cust.[RmId]
        ) THEN cust.[TotalCommitted] ELSE 0 END) AS [Rm_GapExposure],

    SUM(CASE WHEN cust.[PmId] IS NOT NULL AND NOT EXISTS (
            SELECT 1 FROM dbo.[03_LIBRARY_10_Distribution Parties] AS dp WITH (NOLOCK)
            WHERE TRY_CONVERT(int, LTRIM(RTRIM(dp.[Recipient_role]))) = cust.[PmId]
        ) THEN 1 ELSE 0 END) AS [Pm_UnmatchedCustomers],
    SUM(CASE WHEN cust.[PmId] IS NOT NULL AND NOT EXISTS (
            SELECT 1 FROM dbo.[03_LIBRARY_10_Distribution Parties] AS dp WITH (NOLOCK)
            WHERE TRY_CONVERT(int, LTRIM(RTRIM(dp.[Recipient_role]))) = cust.[PmId]
        ) THEN cust.[TotalCommitted] ELSE 0 END) AS [Pm_GapExposure]
FROM cust;
