SELECT TABLE_NAME, COLUMN_NAME, DATA_TYPE, CHARACTER_MAXIMUM_LENGTH,
       NUMERIC_PRECISION, COLUMN_DEFAULT
FROM INFORMATION_SCHEMA.COLUMNS
WHERE TABLE_SCHEMA = 'dbo'
  AND (
        (TABLE_NAME = '03_LIBRARY_10_Distribution Parties' AND COLUMN_NAME = 'Recipient_role')
        OR (TABLE_NAME = '02_CORE_02_Reviews'
            AND COLUMN_NAME IN ('Relationship_mgr_number','Portfolio_mgr_number'))
      );
