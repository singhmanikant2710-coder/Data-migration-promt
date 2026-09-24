Option 2. Hunk B is APPROVED — apply it now, exactly as you diffed it:
comment on 207, remove the hasNonZeroYtd line, if (yNum === null),
comment on 212. Lines 213-223, alias lists, TTM-then-sum ordering and
braces byte-for-byte unchanged. Preserve line endings/indentation,
no auto-format, prettier, or eslint --fix.

Do NOT commit, stage, or push. Check-in stays manual.

Report (raw terminal output, copied not retyped):
1. git diff -- frontend/src/blackbook/mappings/manufacturing.ts
2. git status   (expect exactly 2 modified files)
3. npx tsc --noEmit and lint on manufacturing.ts: error counts before
   vs after. Must not increase.
4. Frontend build pass/fail.


SELECT Review_id, Customer_name,
       Relationship_mgr_number, Relationship_mgr_name, Relationship_mgr_email,
       Portfolio_mgr_number, Portfolio_mgr_name, Portfolio_mgr_email
FROM dbo.[02_CORE_02_Reviews]
WHERE Review_id = 21888;

SELECT TOP 5 Review_id, Customer_name, Sample_id
FROM dbo.[02_CORE_02_Reviews] WITH (NOLOCK)
ORDER BY Review_id DESC;

SELECT Review_id, Customer_name,
       Relationship_mgr_number, Relationship_mgr_name, Relationship_mgr_email,
       Portfolio_mgr_number, Portfolio_mgr_name, Portfolio_mgr_email
FROM dbo.[02_CORE_02_Reviews]
WHERE Review_id = <naya Review_id>;
