WITH cust AS (
    SELECT LTRIM(RTRIM(d.[CUST_NUM])) AS [CustNum],
        TRY_CONVERT(int, MIN(LTRIM(RTRIM(d.[OfficerNumber])))) AS [RmId],
        TRY_CONVERT(int, MIN(LTRIM(RTRIM(d.[PM Number])))) AS [PmId]
    FROM dbo.[01_DATA_01_Data Mart Trial] AS d WITH (NOLOCK)
    WHERE NULLIF(LTRIM(RTRIM(d.[CUST_NUM])), '') IS NOT NULL
    GROUP BY LTRIM(RTRIM(d.[CUST_NUM]))
)
SELECT
    COUNT_BIG(*) AS [DistinctCustomers],

    SUM(CASE WHEN cust.[RmId] IS NULL THEN 1 ELSE 0 END) AS [Rm_NullOfficerNumber],
    SUM(CASE WHEN cust.[RmId] IS NOT NULL AND rm.[Email] IS NULL THEN 1 ELSE 0 END) AS [Rm_HasNumber_NoDistributionMatch],
    SUM(CASE WHEN rm.[Email] IS NOT NULL THEN 1 ELSE 0 END) AS [Rm_Matched],

    SUM(CASE WHEN cust.[PmId] IS NULL THEN 1 ELSE 0 END) AS [Pm_NullPmNumber],
    SUM(CASE WHEN cust.[PmId] IS NOT NULL AND pm.[Email] IS NULL THEN 1 ELSE 0 END) AS [Pm_HasNumber_NoDistributionMatch],
    SUM(CASE WHEN pm.[Email] IS NOT NULL THEN 1 ELSE 0 END) AS [Pm_Matched]

FROM cust
OUTER APPLY (
    SELECT TOP (1) LTRIM(RTRIM(dp.[Recipient_email])) AS [Email]
    FROM dbo.[03_LIBRARY_10_Distribution Parties] AS dp WITH (NOLOCK)
    WHERE TRY_CONVERT(int, LTRIM(RTRIM(dp.[Recipient_role]))) = cust.[RmId]
      AND NULLIF(LTRIM(RTRIM(dp.[Recipient_email])), '') IS NOT NULL
) AS rm
OUTER APPLY (
    SELECT TOP (1) LTRIM(RTRIM(dp.[Recipient_email])) AS [Email]
    FROM dbo.[03_LIBRARY_10_Distribution Parties] AS dp WITH (NOLOCK)
    WHERE TRY_CONVERT(int, LTRIM(RTRIM(dp.[Recipient_role]))) = cust.[PmId]
      AND NULLIF(LTRIM(RTRIM(dp.[Recipient_email])), '') IS NOT NULL
) AS pm;
