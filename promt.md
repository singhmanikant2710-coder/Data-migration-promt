SELECT d.[Market], d.[ACCT_NUM], d.[Commitment]
FROM dbo.[01_DATA_01_Data Mart Trial] d WITH (NOLOCK)
WHERE LTRIM(RTRIM(d.[CUST_NUM])) = '<customer number>'
ORDER BY TRY_CONVERT(decimal(19,2), REPLACE(REPLACE(d.[Commitment],'$',''),',','')) DESC;
