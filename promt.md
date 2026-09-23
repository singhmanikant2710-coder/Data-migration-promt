;WITH cust AS (
    SELECT
        LTRIM(RTRIM(d.[CUST_NUM])) AS [CustNum],
        TRY_CONVERT(int, MIN(LTRIM(RTRIM(d.[OfficerNumber])))) AS [RmId],
        TRY_CONVERT(int, MIN(LTRIM(RTRIM(d.[PM Number])))) AS [PmId],
        SUM(TRY_CONVERT(decimal(38,10), NULLIF(d.[Commitment], ''))) AS [TotalCommitted]
    FROM dbo.[01_DATA_01_Data Mart Trial] AS d WITH (NOLOCK)
    WHERE NULLIF(LTRIM(RTRIM(d.[CUST_NUM])), '') IS NOT NULL
      AND d.[SourceSystem] IN ('ACBS', 'MWS', 'IFL')
    GROUP BY LTRIM(RTRIM(d.[CUST_NUM]))
),
flagged AS (
    SELECT
        cust.[CustNum],
        cust.[TotalCommitted],
        CASE WHEN cust.[RmId] IS NOT NULL AND NOT EXISTS (
            SELECT 1 FROM dbo.[03_LIBRARY_10_Distribution Parties] AS dp WITH (NOLOCK)
            WHERE TRY_CONVERT(int, LTRIM(RTRIM(dp.[Recipient_role]))) = cust.[RmId]
        ) THEN 1 ELSE 0 END AS [RmUnmatched],
        CASE WHEN cust.[PmId] IS NOT NULL AND NOT EXISTS (
            SELECT 1 FROM dbo.[03_LIBRARY_10_Distribution Parties] AS dp WITH (NOLOCK)
            WHERE TRY_CONVERT(int, LTRIM(RTRIM(dp.[Recipient_role]))) = cust.[PmId]
        ) THEN 1 ELSE 0 END AS [PmUnmatched]
    FROM cust
)
SELECT
    SUM(f.[RmUnmatched]) AS [Rm_UnmatchedCustomers],
    SUM(CASE WHEN f.[RmUnmatched] = 1 THEN f.[TotalCommitted] ELSE 0 END) AS [Rm_GapExposure],
    SUM(f.[PmUnmatched]) AS [Pm_UnmatchedCustomers],
    SUM(CASE WHEN f.[PmUnmatched] = 1 THEN f.[TotalCommitted] ELSE 0 END) AS [Pm_GapExposure]
FROM flagged AS f;


Attached is the output of a SQL query showing, for Data Mart Trial customers
filtered to SourceSystem IN ('ACBS','MWS','IFL'), the Relationship Managers
and Portfolio Managers whose Employee ID has no matching row in
[03_LIBRARY_10_Distribution Parties]. Columns: Field (RM/PM), EmployeeId,
DataMartName, CustomerCount, CommittedExposure.

Please analyze this data and produce a summary for the client (Geoff), who
asked: "I want to get a feel for how many and how much exposure may fall
within the data gap when cleaned up for relevant universe bank systems."

Specifically:
1. Total unmatched RM count and total unmatched PM count (distinct officers).
2. Total CommittedExposure summed across all unmatched RMs, and separately
   across all unmatched PMs (careful not to double-count if the same customer
   appears under both an unmatched RM and unmatched PM row).
3. Top 10 unmatched RMs and top 10 unmatched PMs by CommittedExposure —
   these represent the biggest-dollar gaps and are most worth the client's
   attention first.
4. Any officers with a very high CustomerCount but negligible
   CommittedExposure (Geoff mentioned some of the original 10 RM examples
   were commercial credit card account managers with "negligible counts and
   dollars, many from non-universe bank systems" — flag anything that looks
   similar, i.e. many customers but very low total exposure, since these are
   likely NOT genuine gaps worth chasing).
5. A short plain-English summary paragraph suitable for pasting into a Teams
   message to a client, covering the overall scale of the gap (customer count
   and dollar exposure) for RM and PM separately.

Don't modify any code — this is a data-analysis task only. Present the
findings as a summary I can share directly with the client.
