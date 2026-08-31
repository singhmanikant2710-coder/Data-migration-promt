SELECT strCustomerName, strMonthKey, strCovenantName, strCovenantReported
FROM tblMainCovenants
WHERE LTRIM(RTRIM(strCustomerName)) = 'MDR CONSTRUCTION INC'
ORDER BY strMonthKey DESC, intCovenantOrder;

-- Customer ke covenants kitne unique "sets" mein hain (relationship column ho to)
SELECT COLUMN_NAME
FROM INFORMATION_SCHEMA.COLUMNS
WHERE TABLE_NAME = 'tblMainCovenants'
  AND (COLUMN_NAME LIKE '%elationship%' OR COLUMN_NAME LIKE '%acility%' OR COLUMN_NAME LIKE '%ntity%');
