-- Ek row mein test value daalo (dev DB, transaction ke saath)
BEGIN TRAN;
UPDATE TOP (1) dbo.[01_DATA_01_Data Mart Trial]
SET [IntRepCMLSubCategory] = 'TEST INDUSTRY'
WHERE [IntRepCMLSubCategory] IS NULL;

-- endpoint dobara hit karo -> "TEST INDUSTRY" aana chahiye dropdown mein
-- confirm hone ke baad:
ROLLBACK;  -- test data hata do
