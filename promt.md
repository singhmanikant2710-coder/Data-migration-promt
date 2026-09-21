SELECT strMonthKey, dblCovenantActual1, dblCovenantActual2
FROM tblMain
WHERE strCustomerName = 'ATHENS PAPER COMPANY INC'
  AND (dblCovenantActual1 IS NOT NULL OR dblCovenantActual2 IS NOT NULL)
ORDER BY strMonthKey;
