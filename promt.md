SELECT TABLE_NAME, COLUMN_NAME, DATA_TYPE, CHARACTER_MAXIMUM_LENGTH, NUMERIC_PRECISION
FROM INFORMATION_SCHEMA.COLUMNS
WHERE TABLE_SCHEMA = 'dbo'
  AND (
        (TABLE_NAME = '03_LIBRARY_10_Distribution Parties' AND COLUMN_NAME = 'Recipient_role')
        OR (TABLE_NAME = '01_DATA_01_Data Mart Trial' AND COLUMN_NAME IN ('OfficerNumber','PM Number'))
        OR (TABLE_NAME = '02_CORE_02_Reviews'
            AND COLUMN_NAME IN ('Relationship_mgr_number','Portfolio_mgr_number'))
      );

      SELECT Recipient_name,
       Recipient_role,
       DATALENGTH(Recipient_role) AS RawByteLength,
       CASE WHEN CONVERT(nvarchar(50), Recipient_role) LIKE '0%'
            THEN 'leading zeros present' ELSE 'no leading zeros' END AS ZeroCheck
FROM dbo.[03_LIBRARY_10_Distribution Parties] WITH (NOLOCK)
WHERE Recipient_name = 'GREER, ANDREW T';

SELECT LEN(LTRIM(RTRIM(CONVERT(nvarchar(50), Recipient_role)))) AS IdLength,
       SUM(CASE WHEN CONVERT(nvarchar(50), Recipient_role) LIKE '0%' THEN 1 ELSE 0 END) AS WithLeadingZero,
       COUNT_BIG(*) AS [Rows]
FROM dbo.[03_LIBRARY_10_Distribution Parties] WITH (NOLOCK)
WHERE NULLIF(LTRIM(RTRIM(CONVERT(nvarchar(50), Recipient_role))), '') IS NOT NULL
GROUP BY LEN(LTRIM(RTRIM(CONVERT(nvarchar(50), Recipient_role))))
ORDER BY IdLength;
