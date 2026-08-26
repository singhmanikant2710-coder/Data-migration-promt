SELECT TOP 20 strMonthKey, strCustomerName, LEN(strMonthKey) AS key_length
FROM tblMain
WHERE LTRIM(RTRIM(strCustomerName)) = 'AMERICAN CREDIT ACCEPTANCE'
ORDER BY strMonthKey DESC;
