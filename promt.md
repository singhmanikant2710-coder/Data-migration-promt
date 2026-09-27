SELECT TOP 10 c.strCustomerName, m.strIndustry,
  COUNT(DISTINCT c.strCovenantName) AS covenants,
  MAX(c.intCovenantOrder) AS max_order
FROM tblMainCovenants c
JOIN tblMain m ON m.strCustomerName = c.strCustomerName AND m.strMonthKey = c.strMonthKey
WHERE c.strMonthKey >= '202501'
  AND c.intCovenantOrder BETWEEN 1 AND 4
  AND m.strIndustry NOT LIKE 'Wholesale%'
GROUP BY c.strCustomerName, m.strIndustry
HAVING MAX(c.intCovenantOrder) >= 3
ORDER BY covenants DESC;


SELECT DISTINCT strCovenantName, intCovenantOrder
FROM tblMainCovenants
WHERE strCustomerName = '<CUSTOMER>' AND strMonthKey >= '202501'
ORDER BY intCovenantOrder;

SELECT strCustomFieldDescription1, strCustomFieldDescription2,
       strCustomFieldDescription3, strCustomFieldDescription4
FROM tblCustomer WHERE strCustomerName = '<CUSTOMER>';
