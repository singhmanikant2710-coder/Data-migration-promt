SELECT COLUMN_NAME
FROM INFORMATION_SCHEMA.COLUMNS
WHERE TABLE_NAME = 'tblMain'
  AND (COLUMN_NAME LIKE '%Note%' OR COLUMN_NAME LIKE '%Internal%');

  SELECT TOP 20
    strCustomerName,
    strMonthKey,
    strInternalNotes
FROM tblMain
WHERE strInternalNotes LIKE '%&quot;%'
   OR strInternalNotes LIKE '%&amp;%'
   OR strInternalNotes LIKE '%&lt;%'
   OR strInternalNotes LIKE '%&gt;%'
   OR strInternalNotes LIKE '%&#%'
ORDER BY LEN(strInternalNotes) DESC;

SELECT TOP 20
    strCustomerName,
    strMonthKey,
    LEN(strInternalNotes) AS notes_length,
    strInternalNotes
FROM tblMain
WHERE strInternalNotes IS NOT NULL
  AND LEN(LTRIM(RTRIM(strInternalNotes))) > 20
ORDER BY LEN(strInternalNotes) DESC;

SELECT strMonthKey, strInternalNotes
FROM tblMain
WHERE LTRIM(RTRIM(strCustomerName)) = '1ST FRANKLIN FINANCIAL CORPORATION'
ORDER BY strMonthKey DESC;
