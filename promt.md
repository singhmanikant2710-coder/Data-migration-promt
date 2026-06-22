SELECT TABLE_NAME, COLUMN_NAME, ORDINAL_POSITION
FROM INFORMATION_SCHEMA.COLUMNS
WHERE TABLE_NAME IN (
    '02_CORE_01_Samples',
    '02_CORE_02_Reviews',
    '02_CORE_03_Scorecards',
    '02_CORE_04_Accounts',
    '02_CORE_05_Covenants',
    '02_CORE_06_Policy Exceptions',
    '02_CORE_07_Findings',
    '02_CORE_08_Checklists',
    '03_LIBRARY_01_CAS Findings',
    '03_LIBRARY_03_Policy Exceptions',
    '03_LIBRARY_09_Selections'
)
ORDER BY TABLE_NAME, ORDINAL_POSITION;
