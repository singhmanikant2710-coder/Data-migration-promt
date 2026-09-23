Decision confirmed by the client: implement items 4-5 exactly as spec'd —
default Relationship_mgr_number/_name/_email and Portfolio_mgr_number/_name/
_email to NULL when there's no Distribution Parties match, for the reviewer to
assign manually via the front-end form. No fallback to Data Mart name/number on
miss — this is a deliberate behavioral change from today's INSERT.

Scope: newly loaded/committed reviews only (via the Sample Load commit flow —
SqlSampleLoadRepository.CommitValidatedToReviewsAsync, the INSERT at line 743).
Do NOT backfill existing reviews — the client's team is handling historical
data separately during next week's production migration (full purge + reload).

IMPORTANT — the fan-out-safety requirement from before still applies:
implement via correlated TOP(1)/OUTER APPLY (exactly the pattern used in the
diagnostic queries), never a plain LEFT JOIN, since duplicate Employee IDs are
possible in Distribution Parties even though none exist today.

Implementation:
1. In the INSERT...SELECT at SqlSampleLoadRepository.cs:743, after computing
   the existing MIN(d.[OfficerNumber]) / MIN(d.[PMName]) etc. per customer,
   resolve each against Distribution Parties via OUTER APPLY with TOP(1),
   matching on TRY_CONVERT(int, ...) = the employee ID, same as the diagnostic
   queries.
2. Relationship_mgr_number, Relationship_mgr_name, Relationship_mgr_email are
   ALL set from the Distribution Parties match if one exists (number and name
   from Distribution Parties, not Data Mart Trial); if no match, all three are
   NULL — even though Data Mart Trial had a number/name available. Same rule
   for Portfolio_mgr_number/_name/_email.
3. Also populate Relationship_mgr_email / Portfolio_mgr_email — currently NOT
   in the INSERT column list at all (confirmed in your investigation). Add
   these columns to the INSERT with the resolved email from Distribution
   Parties.
4. As a defensive measure (from the empty Query 3 result — no duplicates
   today, but the schema permits them), add a deterministic ORDER BY inside
   each OUTER APPLY's TOP(1) subquery (e.g. ORDER BY Recipient_email) so
   behavior stays consistent even if a duplicate ID is introduced later.
   Apply this same ORDER BY fix to the existing review-form resolver
   (ResolveDistributionPartyByEmployeeIdAsync) while you're in this area —
   low priority but cheap to include now.

Acceptance criteria:
- A newly loaded review whose RM's employee ID has no Distribution Parties
  match ends up with Relationship_mgr_number/_name/_email all NULL (not the
  Data Mart Trial values).
- A newly loaded review whose RM's employee ID DOES match gets number/name/
  email all sourced from Distribution Parties.
- Same for Portfolio Manager fields.
- No fan-out: verify row counts before/after against a known customer with a
  duplicate-prone scenario (or reason through the OUTER APPLY TOP(1) guarantee
  explicitly).
- Existing reviews are untouched — this only affects the INSERT path for new
  sample-load commits.
- The review-form's Customer Info save path (canonical-name fix from
  a5b4ffa) is unaffected — that's a separate, already-shipped code path with
  its own (fallback, not NULL) behavior, which stays as-is per the client's
  answer to question 1 in our Teams thread — the two paths are intentionally
  different (batch load forces manual assignment; the review form's manual
  edit path still falls back to Data Mart naming when a match isn't found).


  ;WITH rm_officers AS (
    SELECT DISTINCT
        TRY_CONVERT(int, LTRIM(RTRIM(d.[OfficerNumber]))) AS [EmployeeId],
        LTRIM(RTRIM(d.[OfficerName])) AS [Name]
    FROM dbo.[01_DATA_01_Data Mart Trial] AS d WITH (NOLOCK)
    WHERE TRY_CONVERT(int, LTRIM(RTRIM(d.[OfficerNumber]))) IS NOT NULL
),
pm_officers AS (
    SELECT DISTINCT
        TRY_CONVERT(int, LTRIM(RTRIM(d.[PM Number]))) AS [EmployeeId],
        LTRIM(RTRIM(d.[PMName])) AS [Name]
    FROM dbo.[01_DATA_01_Data Mart Trial] AS d WITH (NOLOCK)
    WHERE TRY_CONVERT(int, LTRIM(RTRIM(d.[PM Number]))) IS NOT NULL
)

SELECT TOP (10)
    'Relationship Manager' AS [Field],
    o.[EmployeeId],
    o.[Name] AS [DataMartName]
FROM rm_officers AS o
WHERE NOT EXISTS (
    SELECT 1 FROM dbo.[03_LIBRARY_10_Distribution Parties] AS dp WITH (NOLOCK)
    WHERE TRY_CONVERT(int, LTRIM(RTRIM(dp.[Recipient_role]))) = o.[EmployeeId]
)
ORDER BY o.[EmployeeId]

UNION ALL

SELECT TOP (10)
    'Portfolio Manager',
    o.[EmployeeId],
    o.[Name]
FROM pm_officers AS o
WHERE NOT EXISTS (
    SELECT 1 FROM dbo.[03_LIBRARY_10_Distribution Parties] AS dp WITH (NOLOCK)
    WHERE TRY_CONVERT(int, LTRIM(RTRIM(dp.[Recipient_role]))) = o.[EmployeeId]
)
ORDER BY o.[EmployeeId];
