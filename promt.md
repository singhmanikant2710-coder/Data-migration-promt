Confirmed schema state (ran INFORMATION_SCHEMA.COLUMNS myself):

TABLE                              COLUMN                    TYPE            DEFAULT
02_CORE_02_Reviews                 Relationship_mgr_number   nvarchar(10)    '0'
02_CORE_02_Reviews                 Portfolio_mgr_number      nvarchar(10)    '0'
03_LIBRARY_10_Distribution Parties Recipient_role            nvarchar(10)    '0'

All three moved from int to nvarchar(10). Ashok's team also: backfilled
existing values to be zero-padded to 5 characters, and added a trigger
enforcing 5-character zero-padded values on future inserts/updates.

Do NOT implement any .NET-side padding — not needed, DB handles it now.
This is a verification task: confirm existing code still works correctly
against this new schema. Read-only investigation first; only fix what's
actually broken.

Check each of these for type-mismatch issues now that these columns are
nvarchar instead of int:

1. RM/PM Distribution Parties resolver
   (ResolveDistributionPartyByEmployeeIdAsync) — the matching logic used
   TRY_CONVERT(int, LTRIM(RTRIM(dp.[Recipient_role]))) = @EmployeeId style
   comparisons. Since Recipient_role is now nvarchar and zero-padded (e.g.
   "00030"), does the join/comparison still work correctly? Specifically:
   what type/format is @EmployeeId being passed as from the C# side — an int,
   or a string? If it's an int being compared against a zero-padded nvarchar
   via TRY_CONVERT(int, ...), this should still technically work (TRY_CONVERT
   strips leading zeros back to a number), but confirm this explicitly rather
   than assuming.

2. Sample-load NULL-on-miss INSERT (SqlSampleLoadRepository, commit
   ed366c3) — this INSERT wrote Relationship_mgr_number / Portfolio_mgr_number
   as int-typed SQL parameters/expressions. Now that the destination column is
   nvarchar(10), will this INSERT still succeed (implicit conversion), or does
   it need to be updated to pass a zero-padded string instead? If it still
   inserts successfully but WITHOUT zero-padding (e.g. writes "30" instead of
   "00030"), that violates the new trigger's intent even if it doesn't error —
   check whether the trigger would catch and reformat this, or whether it only
   applies to values already in string form.

3. Customer Info page save path (SqlReviewRepository, canonical-name fix,
   commit a5b4ffa) — same check: are Relationship_mgr_number / Portfolio_mgr_
   number being written as int parameters that now target an nvarchar column?
   Does the save still work, and does the saved value end up correctly
   zero-padded, or does it need explicit formatting before the UPDATE?

4. Response DTOs / API contracts — find every C# model/DTO property typed as
   int or int? for RelationshipManagerNumber / PortfolioManagerNumber /
   EmployeeId (Distribution Parties) equivalents. If EF or Dapper is mapping
   an nvarchar column into an int property, will that throw a runtime
   InvalidCastException, or does it handle it gracefully? This is the
   highest-risk item — a hard type mismatch here would break the API
   entirely, not just misformat data.

5. Distribution Parties maintenance screen — Employee ID field/save logic.
   Same check as #4: is the frontend/backend expecting a number here, and
   would receiving "00030" from the API instead of 30 break parsing,
   validation, or display anywhere?

6. Frontend — anywhere the RM/PM/Employee ID display currently does numeric
   formatting, sorting, or comparison (e.g. Number(value), parseInt, sorting
   columns numerically) on these fields — that logic will now receive
   zero-padded strings like "00030" and needs to handle them as strings, not
   numbers, or sorting/comparison could behave incorrectly (e.g. "00030"
   sorting before "17436" alphabetically is fine, but "9000" vs "00030" as
   strings sorts wrong vs as numbers).

Report back exactly what's broken (with the specific error or wrong behavior)
vs what still works fine as-is. Only fix items that are actually broken —
don't touch anything working correctly via implicit conversion.
