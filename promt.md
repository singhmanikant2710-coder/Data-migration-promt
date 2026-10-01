Hi Geoff / John,

The CompCallCode fix is done and on QA. Root cause: the upload was
expanding anything that looked like scientific notation in every column,
including text columns, so "01E1" became "010". It now only does that for
numeric columns. Values like 01E0, 01E1, 01E2 are stored exactly as in the
file, and DelinquentID and amount columns are unaffected.

On your question: yes, the upload accepts both .csv and .xlsx. One caveat
for xlsx: if Excel has already converted the cell to a number (e.g. 01E0
saved as 1), the original text is lost in the file itself. So for xlsx,
please format the CompCallCode column as Text before saving. CSV avoids
this.

John, could you re-upload the 8/31 file to QA and confirm CompCallCode
values like 01E0 / 01E1 / 01E2 come through as-is? Once confirmed, the
same re-upload will be needed wherever the 8/31 data was already loaded.

FYI: the upload also strips leading zeros from OfficerNumber / PM Number.
This doesn't affect RM/PM matching or display in CASRR (we match on the
numeric value and pad the display), so no action needed. Just flagging it
in case those IDs are used elsewhere.

Thanks,
Manikant


Context: CASRR. Spec item 3: "Create a monthly ingestion process that
re-populates the entire Distribution Parties table, similar to the Data Mart
Trial staging table ingestion process." Item 2 (a separate team, John
Halsrud) will produce a monthly extract from the Commercial Lending
Authority (CLA) database; this process ingests that file.

Table: dbo.[03_LIBRARY_10_Distribution Parties]
- Recipient_email (PK), Recipient_name, Recipient_role (nvarchar(10),
  holds the Employee ID, zero-padded to 5 chars by a DB trigger; DBA is
  adding a unique index on it).
- Read by: RM/PM/PML/ECO/SCO dropdowns, the RM/PM email/name resolver,
  sample-load NULL-on-miss, Reports RM/PM filter labels, and the
  Distribution Parties maintenance screen.

INVESTIGATE ONLY — do not write code yet. Report back:

1. How the existing monthly Data Mart upload works end to end
   (/admin/monthly-upload → api/monthly-upload/{parse,save}): file types,
   parse/preview step, validation, how rows are written (truncate+insert?
   transaction? batch size?), rollback on failure, who can access it, audit
   or logging.
2. Which parts can be reused for a Distribution Parties upload, and which
   would need to be new.
3. Proposed design for a DP upload, covering:
   - Expected columns: Email, Name, Employee ID (anything else?)
   - Full replace done atomically (one transaction; on any failure the old
     table stays intact)
   - Validation that rejects the WHOLE file before anything is written:
     blank/invalid email, duplicate email, blank or non-numeric Employee ID,
     ID longer than 5 digits, duplicate Employee ID (incl. "30" vs "00030"),
     blank name. Show row-level errors on screen.
   - Leading zeros: Employee ID must be read as text (same lesson as the
     CompCallCode fix), and stay compatible with the padding trigger.
   - A preview/diff before commit: rows added / removed / changed vs the
     current table.
   - Access: same admin restriction as the Data Mart upload.
4. Risks:
   - Manual edits made through the Distribution Parties maintenance screen
     will be overwritten by each monthly full replace. Options?
   - What happens to existing reviews whose stored RM/PM ID is no longer in
     the new file (nothing should change on saved reviews; confirm).
   - Effect on the unique index and the padding trigger during a bulk load.
5. Open questions I need to ask the client (file format/source, column
   names, schedule, who uploads, keep or drop manual entries).

Report findings only, with file:line references. No code changes, nothing
committed.
