WITH lastm AS (
  SELECT strCustomerName, MAX(strMonthKey) AS mk
  FROM tblMainCovenants GROUP BY strCustomerName
),
per AS (
  SELECT c.strIndustry, v.strCustomerName,
    COUNT(*) AS totalCov,
    SUM(CASE WHEN v.intCovenantOrder BETWEEN 1 AND 5 THEN 1 ELSE 0 END) AS validCov,
    SUM(CASE WHEN v.intCovenantOrder = 0 THEN 1 ELSE 0 END) AS orderZero,
    SUM(CASE WHEN v.intCovenantOrder > 5 THEN 1 ELSE 0 END) AS orderAbove5,
    MAX(v.intCovenantOrder) AS maxOrder
  FROM tblMainCovenants v
  JOIN lastm l ON l.strCustomerName = v.strCustomerName AND l.mk = v.strMonthKey
  JOIN tblCustomer c ON c.strCustomerName = v.strCustomerName
  GROUP BY c.strIndustry, v.strCustomerName
)
SELECT strIndustry, COUNT(*) AS customers,
  MIN(totalCov) AS minTotal, MAX(totalCov) AS maxTotal,
  MIN(validCov) AS minValid, MAX(validCov) AS maxValid,
  MAX(orderZero) AS maxOrderZero, MAX(orderAbove5) AS maxAbove5, MAX(maxOrder) AS maxOrder,
  SUM(CASE WHEN totalCov > 6 THEN 1 ELSE 0 END) AS customersOver6,
  SUM(CASE WHEN validCov > 4 THEN 1 ELSE 0 END) AS customersValidOver4
FROM per
GROUP BY strIndustry
ORDER BY strIndustry;

WITH lastm AS (
  SELECT strCustomerName, MAX(strMonthKey) AS mk
  FROM tblMainCovenants GROUP BY strCustomerName
)
SELECT v.strCustomerName, v.intCovenantOrder, COUNT(*) AS covenantsOnSameOrder
FROM tblMainCovenants v
JOIN lastm l ON l.strCustomerName = v.strCustomerName AND l.mk = v.strMonthKey
WHERE v.intCovenantOrder BETWEEN 1 AND 5
GROUP BY v.strCustomerName, v.intCovenantOrder
HAVING COUNT(*) > 1;
