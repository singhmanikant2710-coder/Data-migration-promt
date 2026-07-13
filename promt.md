UAT #54 follow-up — single word change.

In CrmFindingsAndRatingsSection.tsx, change the findings table column header from "Info" to "Finding Type".

Nothing else changes — the cell content (Finding Category text) stays exactly as it is.

Apply and show the diff.

SELECT COLUMN_NAME, DATA_TYPE
FROM INFORMATION_SCHEMA.COLUMNS
WHERE TABLE_SCHEMA = 'dbo'
  AND TABLE_NAME = '03_LIBRARY_01_CAS Findings';
