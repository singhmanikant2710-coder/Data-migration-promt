TASK: CASRR UAT #244 – Record Retention. READ-ONLY DISCOVERY ONLY. Do NOT edit, create or delete any source code, config or SQL object. Do NOT run any INSERT/UPDATE/DELETE/DDL.

OUTPUT RULE (MANDATORY):
Write your COMPLETE findings into a plain text file:
  devin-output/244-phase1-discovery.txt
(create the devin-output/ folder if missing). This is the ONLY file you may create. Do not just give me a SQL query – I need the actual findings in the file. In chat, only reply with the file path and a 5-line summary. Never commit or push this file.

If you have database access to CAS_RISKREVIEW_DEV, RUN the read-only SELECT queries yourself and paste the RESULTS (not just the query) into the file. If you do NOT have DB access, say so clearly at the top of the file, then (a) derive everything you can from the code, EF mappings, repositories and scripts/sql/, and (b) include the read-only query at the end for me to run.

Put these sections in the file, each with file:line references where applicable:

1. DOWNSTREAM TABLES
   Every table that references dbo.[02_CORE_01_Samples] or dbo.[02_CORE_02_Reviews] – via FOREIGN KEY (sys.foreign_keys) OR via a Sample_id / Review_id (or similar) column (INFORMATION_SCHEMA.COLUMNS), including 02_CORE_04_Accounts, 02_CORE_05_Covenants, 02_CORE_06_Policy Exceptions, 02_CORE_07_Findings, 02_CORE_08_Checklists and any history/email/unlock/attachment/audit/temp tables.
   Table format: Table | Key column | Links to (Samples/Reviews/other child) | FK yes/no | ON DELETE rule | Where used in code (file:line).
   Then the required delete order (children first).

2. TRIGGERS on all tables above (sys.triggers) – name, table, event, what it does.

3. ROW COUNTS (if DB access): per table total rows, and how many rows belong to Samples where ALL reviews have Review_finalized_date <= DATEADD(year,-7, CAST(GETDATE() AS date)). Also counts of: Samples with any review where Review_finalized_date IS NULL; Samples with Cancelled reviews; Samples with zero reviews; Samples with mixed in/out-of-retention dates.

4. EXISTING ADMIN PATTERN
   Full path for Distribution Parties Upload (preview -> confirm -> atomic replace -> audit row): UI page -> src/services/api/admin.ts -> AdminController.cs -> service -> repository -> SQL, with file:line for each step. Note how transactions, timeouts and errors are handled.

5. AUTHORIZATION
   How RequireAdmin is defined and applied server-side (file:line) and how app/admin/layout.tsx gates the UI.

6. ERROR HANDLING
   How errors are returned today; confirm AdminController.cs:176 returning raw SQL error in ProblemDetails.Detail and any similar places.

7. CALLERS / IMPACT
   Anything that would need to change for this feature, with every caller listed (file:line). Expected answer: no shared helper/repository changes needed – confirm or explain.

8. OPEN QUESTIONS / RISKS you found (unclear table relationships, orphan risks, large-volume/timeout risks).

At the end of the file: git status output (should show only devin-output/244-phase1-discovery.txt as new). Do not commit or push.
