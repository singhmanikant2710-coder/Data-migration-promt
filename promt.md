SELECT
    LTRIM(RTRIM(strCustomerName)) AS customer_name,
    COUNT(*) AS month_count,
    MIN(LTRIM(RTRIM(strMonthKey))) AS oldest,
    MAX(LTRIM(RTRIM(strMonthKey))) AS newest
FROM tblMain
WHERE LTRIM(RTRIM(strMonthKey)) IS NOT NULL
  AND LTRIM(RTRIM(strMonthKey)) <> ''
GROUP BY LTRIM(RTRIM(strCustomerName))
ORDER BY customer_name;


SELECT
    LTRIM(RTRIM(strCustomerName)) AS customer_name,
    COUNT(*) AS month_count,
    MIN(LTRIM(RTRIM(strMonthKey))) AS oldest,
    MAX(LTRIM(RTRIM(strMonthKey))) AS newest
FROM tblMain
WHERE LTRIM(RTRIM(strMonthKey)) IS NOT NULL
  AND LTRIM(RTRIM(strMonthKey)) <> ''
  AND LTRIM(RTRIM(strMonthKey)) <= '202607'
GROUP BY LTRIM(RTRIM(strCustomerName))
ORDER BY customer_name;
