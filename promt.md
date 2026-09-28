WITH lastm AS (
  SELECT strCustomerName, MAX(strMonthKey) AS mk FROM tblMain GROUP BY strCustomerName
),
cand AS (
  SELECT c.strIndustry, c.strCustomerName, l.mk,
    (SELECT COUNT(*) FROM tblMainCovenants v
      WHERE v.strCustomerName = c.strCustomerName AND v.strMonthKey = l.mk
        AND v.intCovenantOrder BETWEEN 1 AND 4 AND v.strCovenantActual IS NOT NULL) AS covWithValue,
    (SELECT COUNT(*) FROM tblMain z
      WHERE z.strCustomerName = c.strCustomerName AND z.strMonthKey > '202501'
        AND ISNULL(z.curRevenueOrSales,0) = 0) AS zeroMonths,
    (SELECT COUNT(DISTINCT ISNULL(z.strIndustry,'')) FROM tblMain z
      WHERE z.strCustomerName = c.strCustomerName) AS industryVariants
  FROM tblCustomer c JOIN lastm l ON l.strCustomerName = c.strCustomerName
  WHERE c.strIndustry IS NOT NULL AND l.mk >= '202603'
    AND c.strCustomerName NOT IN ('ATHENS PAPER COMPANY INC','MARINER FINANCE LLC',
      'NATIONWIDE SPECIALTY FINANCE INC','MARTIN INCORPORATED','GRACELAND PROPERTIES')
)
SELECT strIndustry, strCustomerName, mk, covWithValue, zeroMonths
FROM (SELECT *, ROW_NUMBER() OVER (PARTITION BY strIndustry
        ORDER BY covWithValue DESC, zeroMonths ASC) rn
      FROM cand WHERE industryVariants = 1) x
WHERE rn = 1 ORDER BY strIndustry;
