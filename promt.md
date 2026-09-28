SELECT DISTINCT strCustomerName, strCovenantName, intCovenantOrder
FROM tblMainCovenants
WHERE intCovenantOrder = 0 OR intCovenantOrder > 4
ORDER BY strCustomerName;


SELECT DISTINCT c.strCustomerName, c.strCovenantName
FROM tblMainCovenants c
JOIN tblMain m ON m.strCustomerName = c.strCustomerName AND m.strMonthKey = c.strMonthKey
WHERE c.intCovenantOrder BETWEEN 1 AND 4
  AND c.strCovenantName NOT IN (ISNULL(m.strCovenantName1,''), ISNULL(m.strCovenantName2,''),
      ISNULL(m.strCovenantName3,''), ISNULL(m.strCovenantName4,''), ISNULL(m.strCovenantName5,''))
ORDER BY c.strCustomerName;

SELECT strCustomerName, strIndustry,
  strCustomFieldDescription1, strCustomFieldDescription2,
  strCustomFieldDescription3, strCustomFieldDescription4
FROM tblCustomer
WHERE COALESCE(strCustomFieldDescription1, strCustomFieldDescription2,
               strCustomFieldDescription3, strCustomFieldDescription4) IS NOT NULL
ORDER BY strIndustry;

SELECT DISTINCT TOP 20 strCustomerName, strIndustry
FROM tblMain WHERE ISNULL(curCPLTDTTM,0) <> 0 AND strMonthKey >= '202601';
