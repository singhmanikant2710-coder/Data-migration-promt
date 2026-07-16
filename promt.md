Backend only. Add a read-only "Distribution Parties" maintenance list, mirroring the Loan Codes pattern exactly. Use LIVE DB, ignore columns.csv. Read-only diagnostics before edits. Single file per edit. Do not add POST/PUT/DELETE — read-only list only.

Table: dbo.[03_LIBRARY_10_Distribution Parties]
Columns: [Recipient_name] nvarchar, [Recipient_email] nvarchar, [Recipient_role] nvarchar. No primary key column. 5 rows.

Create these files, following the exact patterns already used by Loan Codes (LoanCodesController.cs, ILoanCodeRepository.cs, SqlLoanCodeRepository.cs, and its domain entity):

1) Domain entity: backend/src/Casrr.Domain/Entities/DistributionParty.cs
   Properties (trimmed strings, same style as other entities):
     string RecipientName
     string RecipientEmail
     string RecipientRole

2) Repository interface: backend/src/Casrr.Application/IDistributionPartiesRepository.cs
   Method (read-only):
     Task<IReadOnlyList<DistributionParty>> GetAllAsync(CancellationToken ct);

3) Repository impl: backend/src/Casrr.Infrastructure/SqlServer/SqlDistributionPartiesRepository.cs
   Use the same ADO.NET pattern as SqlLoanCodeRepository (SqlConnectionFactory, parameterized, WITH (NOLOCK), LTRIM/RTRIM, DBNull-safe reads). SQL:
     SELECT LTRIM(RTRIM([Recipient_name]))  AS [Recipient_name],
            LTRIM(RTRIM([Recipient_email])) AS [Recipient_email],
            LTRIM(RTRIM([Recipient_role]))  AS [Recipient_role]
     FROM dbo.[03_LIBRARY_10_Distribution Parties] WITH (NOLOCK)
     ORDER BY [Recipient_role], [Recipient_name];

4) Controller: backend/src/Casrr.Api/Controllers/DistributionPartiesController.cs
   [Route("api/v1/distribution-parties")], [Authorize(Policy = "RequireActiveUser")] — same auth/telemetry style as LoanCodesController.
   Single endpoint: GET /api/v1/distribution-parties → returns IReadOnlyList of a DistributionPartyContract { recipientName, recipientEmail, recipientRole }. Include the ToContract mapping like LoanCodesController does.

5) DI wiring: edit backend/src/Casrr.Api/Extensions/StartupExtensions.cs
   Register IDistributionPartiesRepository → SqlDistributionPartiesRepository inside AddDataProviders() (same place Loan Codes is registered).

Do not create any frontend files in this step. After edits, report the files changed.
