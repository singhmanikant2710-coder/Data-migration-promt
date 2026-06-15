Create FILE 3:
backend/src/Casrr.Infrastructure/SqlServer/
SqlNaicsRepository.cs

Follow exact same pattern as:
SqlSelectionRepository.cs

Requirements:
- Implement INaicsRepository
- Use same Dapper pattern as existing
- Database table: 
  [dbo].[03_LIBRARY_08_NAICS]

Column mappings:
NAICS_industry_key → NaicsIndustryKey
NAICS_division → NaicsDivision
NAICS_sector → NaicsSector
NAICS_subsector → NaicsSubsector
NAICS_industry_group → NaicsIndustryGroup
NAICS_code → NaicsCode
NAICS_industry_description → 
  NaicsIndustryDescription

SQL Queries:
- GetAllAsync: 
  SELECT * FROM [03_LIBRARY_08_NAICS]
  WHERE (@sector IS NULL 
  OR NAICS_sector = @sector)

- GetDistinctSectorsAsync:
  SELECT DISTINCT NAICS_sector 
  FROM [03_LIBRARY_08_NAICS]
  WHERE NAICS_sector IS NOT NULL 
  ORDER BY NAICS_sector

- GetDistinctDivisionsAsync:
  SELECT DISTINCT NAICS_division 
  FROM [03_LIBRARY_08_NAICS]
  WHERE NAICS_division IS NOT NULL 
  ORDER BY NAICS_division

- GetByKeyAsync:
  SELECT * FROM [03_LIBRARY_08_NAICS]
  WHERE NAICS_industry_key = @key

- CreateAsync:
  INSERT INTO [03_LIBRARY_08_NAICS]
  (NAICS_industry_key, NAICS_division,
  NAICS_sector, NAICS_subsector,
  NAICS_industry_group, NAICS_code,
  NAICS_industry_description)
  VALUES (@NaicsIndustryKey, 
  @NaicsDivision, @NaicsSector,
  @NaicsSubsector, @NaicsIndustryGroup,
  @NaicsCode, @NaicsIndustryDescription)

- UpdateAsync:
  UPDATE [03_LIBRARY_08_NAICS]
  SET NAICS_division = @NaicsDivision,
  NAICS_sector = @NaicsSector,
  NAICS_subsector = @NaicsSubsector,
  NAICS_industry_group = @NaicsIndustryGroup,
  NAICS_code = @NaicsCode,
  NAICS_industry_description = 
    @NaicsIndustryDescription
  WHERE NAICS_industry_key = 
    @NaicsIndustryKey

- DeleteAsync:
  DELETE FROM [03_LIBRARY_08_NAICS]
  WHERE NAICS_industry_key = @key

Confirm file path after creation.
Wait for approval before FILE 4.
