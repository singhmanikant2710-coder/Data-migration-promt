SELECT 
    KU.COLUMN_NAME
FROM INFORMATION_SCHEMA.TABLE_CONSTRAINTS AS TC
INNER JOIN INFORMATION_SCHEMA.KEY_COLUMN_USAGE AS KU
    ON TC.CONSTRAINT_NAME = KU.CONSTRAINT_NAME
    AND TC.TABLE_SCHEMA = KU.TABLE_SCHEMA
WHERE TC.TABLE_NAME = 'tblMain'
  AND TC.CONSTRAINT_TYPE = 'PRIMARY KEY';

  SELECT 
    annMain,
    intFiscalYear,
    datFiscalYearStart,
    datFirstOfMonth,
    intFiscalYearMonthStart,
    strFiscalYearMonthStart,
    intFiscalMonth
FROM tblMain
WHERE intFiscalYear = 2025
  AND YEAR(datFiscalYearStart) = 2026;
