SELECT TOP 20
    LTRIM(RTRIM(d.[CUST_NUM]))  AS Customer_number,
    MIN(d.[CustomerName])       AS Customer_name,
    COUNT(*)                    AS DataMartRows,
    COUNT(DISTINCT CONCAT(
        ISNULL(d.[Segment],N'~'), N'|',
        ISNULL(CASE WHEN d.[Segment] IN ('Specialty Banking','Wholesale')
                    THEN d.[SpecialtyLine] ELSE d.[Segment1] END, N'~'), N'|',
        ISNULL(d.[Market],N'~'), N'|',
        ISNULL(d.[CustomLOB],N'~'), N'|',
        ISNULL(d.[LOBSub],N'~')))  AS ReviewsThatWouldBeCreated
FROM dbo.[01_DATA_01_Data Mart Trial] d WITH (NOLOCK)
GROUP BY LTRIM(RTRIM(d.[CUST_NUM]))
HAVING COUNT(DISTINCT CONCAT(
        ISNULL(d.[Segment],N'~'), N'|',
        ISNULL(CASE WHEN d.[Segment] IN ('Specialty Banking','Wholesale')
                    THEN d.[SpecialtyLine] ELSE d.[Segment1] END, N'~'), N'|',
        ISNULL(d.[Market],N'~'), N'|',
        ISNULL(d.[CustomLOB],N'~'), N'|',
        ISNULL(d.[LOBSub],N'~'))) > 1
ORDER BY DataMartRows;


SELECT r.Review_id, r.Sample_id, r.Market, r.Customer_size,
       (SELECT COUNT(*) FROM dbo.[02_CORE_04_Accounts] a WHERE a.Review_id = r.Review_id) AS AccountCount
FROM dbo.[02_CORE_02_Reviews] r
WHERE LTRIM(RTRIM(r.Customer_number)) = '<CUST>'
ORDER BY r.Review_id DESC;

SELECT d.[Market], COUNT(*) AS Accounts,
       SUM(TRY_CONVERT(decimal(19,2), REPLACE(REPLACE(d.[Commitment],'$',''),',',''))) AS TotalCommitment
FROM dbo.[01_DATA_01_Data Mart Trial] d
WHERE LTRIM(RTRIM(d.[CUST_NUM])) = '<CUST>'
GROUP BY d.[Market];
