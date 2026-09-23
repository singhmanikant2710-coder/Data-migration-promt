Context: .NET 8 Clean Architecture backend + Next.js/React/TypeScript frontend,
CASRR project. Client flagged a potential data issue: Employee IDs with
leading zeros (e.g., "00030" for GREER, ANDREW T) are displaying as "30" on
the Distribution Parties maintenance screen.

Investigate — do NOT change anything yet, just report back:

1. Query the raw stored value in dbo.[03_LIBRARY_10_Distribution Parties] for
   this specific record:
   SELECT Recipient_name, Recipient_role, DATALENGTH(Recipient_role) AS
   [RawByteLength] FROM dbo.[03_LIBRARY_10_Distribution Parties] WITH (NOLOCK)
   WHERE Recipient_name = 'GREER, ANDREW T';
   Tell me whether Recipient_role is stored as "00030" (zeros preserved in the
   actual column) or "30" (zeros already stripped at the data level).

2. Check the maintenance screen's Add/Edit form and list-display code: is it
   parsing Recipient_role as a number anywhere (int/number type) before
   displaying or storing it? If so, that's what's stripping the leading
   zeros — it should be treated as a fixed-width text/string field throughout
   the UI, not converted to a number.

3. Note for context (not urgent, just background): our RM/PM email-resolution
   join already does TRY_CONVERT(int, ...) on both sides when matching, so
   numeric join correctness (30 = 030 = 00030) is not affected by this issue
   regardless of what you find. This is purely a display/storage-fidelity
   concern, not a join-correctness bug.

Report back: (a) is the DB value itself missing the zeros, or only the
display, and (b) if it's a UI/parsing issue, what needs to change to preserve
and display leading zeros consistently (e.g., always treat as a
zero-padded-5-digit string, or whatever width the source data actually uses —
check a sample of IDs to confirm they're consistently 5 digits before
assuming a fixed width).
