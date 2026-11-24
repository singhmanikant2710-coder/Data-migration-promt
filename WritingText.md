DECLARE @SampleId int = <NEW_SAMPLE_ID>;

SELECT COUNT(*) AS Reviews, COUNT(DISTINCT LTRIM(RTRIM(Customer_number))) AS DistinctCustomers
FROM dbo.[02_CORE_02_Reviews] WITH (NOLOCK) WHERE Sample_id = @SampleId;

SELECT COUNT(*) AS TempRows FROM dbo.[04_TEMP_01_Sample Load] WITH (NOLOCK) WHERE Sample_id = @SampleId;

SELECT r.Review_id FROM dbo.[02_CORE_02_Reviews] r WITH (NOLOCK)
WHERE r.Sample_id = @SampleId
  AND NOT EXISTS (SELECT 1 FROM dbo.[02_CORE_04_Accounts] a WITH (NOLOCK) WHERE a.Review_id = r.Review_id);
