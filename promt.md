-- Rolling 24 endpoint jo deta hai, wo dekho — kitne months aate hain 202607 tak
SELECT COUNT(*) AS month_count,
       MIN(strMonthKey) AS oldest,
       MAX(strMonthKey) AS newest
FROM tblMain
WHERE LTRIM(RTRIM(strCustomerName)) = 'AMERICAN CREDIT ACCEPTANCE'
  AND LTRIM(RTRIM(strMonthKey)) <= '202607';
