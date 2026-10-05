DECLARE @CUST NVARCHAR(50);

-- multi-market customers mein se sabse chhota (kam rows) wala, jiska abhi koi review nahi hai
SELECT TOP 1 @CUST = x.Customer_number
FROM (
    SELECT LTRIM(RTRIM(d.[CUST_NUM])) AS Customer_number, COUNT(*) AS DataMartRows
    FROM dbo.[01_DATA_01_Data Mart Trial] d WITH (NOLOCK)
    GROUP BY LTRIM(RTRIM(d.[CUST_NUM]))
    HAVING COUNT(DISTINCT CONCAT(
        ISNULL(d.[Segment],N'~'), N'|',
        ISNULL(CASE WHEN d.[Segment] IN ('Specialty Banking','Wholesale')
                    THEN d.[SpecialtyLine] ELSE d.[Segment1] END, N'~'), N'|',
        ISNULL(d.[Market],N'~'), N'|',
        ISNULL(d.[CustomLOB],N'~'), N'|',
        ISNULL(d.[LOBSub],N'~'))) > 1
) x
WHERE NOT EXISTS (SELECT 1 FROM dbo.[02_CORE_02_Reviews] r
                  WHERE LTRIM(RTRIM(r.Customer_number)) = x.Customer_number)
ORDER BY x.DataMartRows;

SELECT @CUST AS Customer_number_to_test;

-- Expected answer: market-wise accounts + biggest commitment
SELECT d.[Market], d.[CustomerName], d.[ACCT_NUM], d.[Commitment]
FROM dbo.[01_DATA_01_Data Mart Trial] d WITH (NOLOCK)
WHERE LTRIM(RTRIM(d.[CUST_NUM])) = @CUST
ORDER BY TRY_CONVERT(decimal(19,2), REPLACE(REPLACE(d.[Commitment],'$',''),',','')) DESC;


SELECT r.Review_id, r.Sample_id, r.Market,
       (SELECT COUNT(*) FROM dbo.[02_CORE_04_Accounts] a WHERE a.Review_id = r.Review_id) AS AccountCount
FROM dbo.[02_CORE_02_Reviews] r
WHERE LTRIM(RTRIM(r.Customer_number)) = '<CUST>';
