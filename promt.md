SELECT *
FROM tblCovenants   -- (ya jo covenants table ka naam hai)
WHERE LTRIM(RTRIM(strCustomerName)) = 'MDR CONSTRUCTION INC'
ORDER BY strMonthKey DESC;

SELECT TABLE_NAME 
FROM INFORMATION_SCHEMA.TABLES 
WHERE TABLE_NAME LIKE '%ovenant%';
