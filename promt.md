SELECT r.Review_id, r.Sample_id, r.Customer_number, r.eCIF_number, r.Customer_name,
       (SELECT COUNT(*) FROM dbo.[02_CORE_04_Accounts] a WHERE a.Review_id = r.Review_id) AS AccountCount
FROM dbo.[02_CORE_02_Reviews] r
WHERE r.Customer_number IN ('84935024', '2899') OR r.Customer_name LIKE '%DAIRIO%'
ORDER BY r.Sample_id, r.Review_id;

SELECT *
FROM dbo.[01_DATA_01_Data Mart Trial]
WHERE LTRIM(RTRIM(CUST_NUM)) IN ('84935024', '2899');

Context: CASRR (.NET 8 Clean Architecture backend + Next.js/React/TypeScript
frontend + SQL Server). Production banking app: existing behaviour must not
break.

TASK (UAT #229, Geoff — HIGH PRIORITY, investigate first):
In the Test (QA) environment, loading Customer # 84935024 "D-2/DAIRIO LLC"
into sample 364 (Load Samples → Add Customer → Validate Selection → Load
Samples) created TWO review records for this customer: one with all its
accounts and one with no accounts. Loading Customer # 2899 "D-2/DAIRIO, LLC"
(same company, leasing system, name contains a comma) worked fine. After
the load the customer grid showed "Showing 0-0 of 0" with an empty row.
Expected: one customer = exactly one review with all its accounts.

QA evidence (I ran these in QA; Dev may not reproduce):
Query 1 (reviews + account count): <paste result>
Query 2 (Data Mart rows for 84935024 and 2899): <paste result>

PHASE 1: READ-ONLY DISCOVERY (no edits) — then STOP and report.
- Trace the full Load Samples path with file:line: Add Customer (manual +
  CSV/XLSX), Validate Selection, Load/commit (the SQL that inserts into
  02_CORE_02_Reviews and 02_CORE_04_Accounts), and the grid refresh.
- Find exactly how reviews are grouped/created per customer and how
  accounts are matched to a review (keys used: CUST_NUM, eCIF, name,
  SourceSystem, trimming, data types, leading zeros, case).
- Using the QA evidence, explain precisely why 84935024 produced two
  reviews (e.g. multiple Data Mart rows with different name/eCIF/
  whitespace/source system, duplicate staging row, double Add, validation
  inserting a row, '/' or ',' in the name, retry/double-click on Load).
- Check: can the same customer be added twice to one sample (UI and
  backend)? Is the Load action idempotent / protected against double
  submit? Does the commit run in a single transaction?
- Check whether Dev has the same customer data to reproduce; if yes,
  reproduce read-only and show the result.
- Report other customers in the Data Mart that could hit the same root
  cause (give me the SQL; don't run against Prod).
- STOP: give me the root cause with file:line, and fix options (A/B/C)
  with trade-offs and risk to existing behaviour. Do not edit yet.

After I choose:
PHASE 2: smallest generic fix; no schema change from the app (if a DB
change/cleanup is needed, give an idempotent script under scripts/sql/ for
the DBA, plus a separate script to find/clean the existing duplicate review
in QA — never auto-delete).
PHASE 3: implement.
PHASE 4: build backend + frontend; verify with real functions.

Report:
1. Root cause (file:line) + other issues noticed
2. ADDED lines (file:line)
3. REMOVED / CHANGED lines (file:line)
4. On-screen behaviour before vs after (Add Customer, Validate, Load, grid)
5. Edge cases: name with comma/slash/apostrophe, same company under two
   customer numbers, customer with no accounts, multiple source systems,
   leading zeros/whitespace, double-click Load, CSV vs XLSX vs manual add
6. Other screens/reports unaffected
7. Build results
8. git status (test-data/ and artifacts flagged; no harness files left)
Do not commit or push.
