Decision: B + C.

B: Remove d.[Segment], the Unit CASE, d.[Market], d.[CustomLOB], d.[LOBSub] from the Reviews GROUP BY (SqlSampleLoadRepository.cs:894-898). Take those five attributes from the customer's dominant Data Mart row via OUTER APPLY: highest numeric commitment, tie-breaker ACCT_NUM. Note: Data Mart Commitment is TEXT — order by TRY_CONVERT(decimal(19,2), <cleaned Commitment>) (strip $ and commas if present), NULL/unparseable last. The OUTER APPLY must apply the same filters as the main query (same CUST_NUM trim match AND the existing Unit <> 'Core Regions Cnsmr' exclusion at line 885), so the picked row is always one that the review aggregates. Do not change any other aggregated column or the Accounts insert.

C: Replace SELECT MIN(id) FROM @rid (line 922) with a count check: if more than one Review_id was inserted for the customer, throw so the whole commit transaction rolls back, and return a clear message naming the customer number ("Customer <n> would create more than one review — contact support"). Log it.

DB scripts:
- NO unique index for now.
- YES: scripts/sql/find-duplicate-reviews-per-sample-customer.sql — read-only report of (Sample_id, Customer_number) groups with >1 review incl. AccountCount; include a fully COMMENTED-OUT delete block that targets only explicit Review_ids with 0 accounts (and their child rows: checklists/findings etc. — list every child table), for the DBA to run manually after review. Never auto-delete.

Do NOT fix the other issues you listed (Core Regions Cnsmr accounts exclusion, checklist insert without sample filter, global Customer_number uniqueness, EF connection, placeholder row count) — keep them in the report for the client.

Verify: 84935024 (QA data) → exactly 1 review, 32 accounts, Market = Winston-Salem; 2899 → identical to today (1 review, 4 accounts, same values); a single-market customer → byte-identical insert; tie on commitment → deterministic; commitment text with $/commas/blank. Then PHASE 2-4 and the full report. Do not commit or push.
