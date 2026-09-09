SELECT '[' + strMonthKey + ']' AS mk_exact, LEN(strMonthKey) AS len
FROM tblMain
WHERE LTRIM(RTRIM(strCustomerName)) LIKE '%BANKERS HEALTHCARE%'
  AND strMonthKey LIKE '2027%';
