SELECT strMonthKey, strCovenantName, strCovenantReported
FROM tblMainCovenants
WHERE LTRIM(RTRIM(strCustomerName)) = 'MDR CONSTRUCTION INC'
ORDER BY strCovenantName, strMonthKey DESC;
