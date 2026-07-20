Backend only. Add two new lookups following the EXACT existing pattern of GetRelationshipSegmentsAsync (LookupsController → ReportingService → IReportingRepository → SqlReportingRepository). Use LIVE DB, ignore columns.csv. Single file per edit. Do NOT modify or revert any existing method or any code authored by anyone else (including Jothi) — only ADD new methods/endpoints.

Add these two lookups, returning IReadOnlyList<string> where each string is formatted "Number - Name":

1) Repository interface: backend/src/Casrr.Application/IReportingRepository.cs
   ADD (do not touch existing signatures):
     Task<IReadOnlyList<string>> GetDataMartRelationshipManagersAsync(int topN, CancellationToken ct);
     Task<IReadOnlyList<string>> GetDataMartPortfolioManagersAsync(int topN, CancellationToken ct);

2) Repository impl: backend/src/Casrr.Infrastructure/SqlServer/SqlReportingRepository.cs
   ADD two methods copying the exact style of GetRelationshipSegmentsAsync (SqlConnectionFactory, @TopN parameter, WITH (NOLOCK), reader loop, trim).

   RM SQL:
     SELECT DISTINCT TOP (@TopN)
            CAST([OfficerNumber] AS NVARCHAR(50)) + ' - ' + LTRIM(RTRIM([OfficerName])) AS [Val]
     FROM dbo.[01_DATA_01_Data Mart Trial] WITH (NOLOCK)
     WHERE [OfficerNumber] IS NOT NULL
       AND [OfficerName] IS NOT NULL
       AND LTRIM(RTRIM([OfficerName])) <> ''
     ORDER BY [Val];

   PM SQL:
     SELECT DISTINCT TOP (@TopN)
            CAST([PM Number] AS NVARCHAR(50)) + ' - ' + LTRIM(RTRIM([PMName])) AS [Val]
     FROM dbo.[01_DATA_01_Data Mart Trial] WITH (NOLOCK)
     WHERE [PM Number] IS NOT NULL
       AND [PMName] IS NOT NULL
       AND LTRIM(RTRIM([PMName])) <> ''
     ORDER BY [Val];

3) Service: backend/src/Casrr.Application/Services/ReportingService.cs
   ADD two methods mirroring GetRelationshipSegmentsAsync (const int topN = 5000 for both, since RM has ~2356 distinct values):
     GetDataMartRelationshipManagersAsync(ct)
     GetDataMartPortfolioManagersAsync(ct)
   Also add the matching signatures to the service interface if one exists.

4) Controller: backend/src/Casrr.Api/Controllers/LookupsController.cs
   ADD two GET endpoints copying the exact try/catch/telemetry/ProblemDetails style of GetRelationshipSegments:
     [HttpGet("data-mart/relationship-managers")]  -> GetDataMartRelationshipManagers
     [HttpGet("data-mart/portfolio-managers")]     -> GetDataMartPortfolioManagers

Do not change the frontend in this step. Do not modify any existing method. Report the files changed.
