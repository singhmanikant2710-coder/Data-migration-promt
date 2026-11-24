DECLARE @SampleId int = 387;

SELECT COUNT(*) AS Reviews, COUNT(DISTINCT LTRIM(RTRIM(Customer_number))) AS DistinctCustomers
FROM dbo.[02_CORE_02_Reviews] WITH (NOLOCK) WHERE Sample_id = @SampleId;

SELECT COUNT(*) AS TempRows FROM dbo.[04_TEMP_01_Sample Load] WITH (NOLOCK) WHERE Sample_id = @SampleId;
