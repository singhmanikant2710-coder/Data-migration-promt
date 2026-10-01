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

Hi Geoff,

Before we build the monthly Distribution Parties ingestion (spec item 3),
a few decisions are needed from you:

1. File format and columns: will John Halsrud's CLA extract be CSV or
   xlsx, and with exactly which columns? Our proposal is a fixed 3-column
   template: Email, Name, Employee ID. If xlsx, the Employee ID column must
   be formatted as Text, otherwise Excel drops the leading zeros before we
   receive the file.

2. Manual entries: each monthly load fully replaces the table, so anyone
   added or edited through the Distribution Parties maintenance screen
   would be wiped. Options:
   a) Keep manual entries: add a "Source" column (CLA / Manual), and the
      monthly load only replaces CLA rows (needs a small DBA change).
   b) CLA file is the single source of truth: the maintenance screen
      becomes read-only.
   c) Upsert only: add/update from the file, never delete (people who
      leave the bank stay in the list until removed manually).
   Which do you prefer?

3. Leavers: if someone is in the table but not in the new file, should
   they be deleted, or kept and marked inactive?

4. Who uploads and when: an admin uploading manually each month (same as
   the Data Mart upload), or something automated?

5. Timing: since the CLA extract (item 2) isn't available yet, should we
   build the upload against the agreed template now, so it's ready when
   John's extract is?

How it will work: the whole file is validated first (blank/duplicate
emails, missing or duplicate Employee IDs, etc.), and you'll see a preview
of rows added / removed / changed before confirming. The load runs as one
transaction, so if anything fails, the existing table stays exactly as it
was.

Separately, FYI: the existing monthly Data Mart upload clears the table
before loading, in separate steps. If an upload fails midway, the Data
Mart table can be left empty or partially loaded until it's re-uploaded.
Not urgent, but worth knowing, and we can make it atomic the same way if
you'd like.

Thanks,
Manikant


Context: CASRR. Review form → "Email" button opens the "Share via Email"
modal (To, CC, Subject, Document Type: Initial Memo / Final Memo, Body,
attachment, Send). Client requirement (item 4):
 i.   Email button opens the modal (already works — don't rebuild).
 ii.  "To" = Relationship_mgr_email + Portfolio_mgr_email.
 iii. "CC" = Portfolio_mgr_lead_email + SCO_email + ECO_email.
 iv.  Document Type dropdown defaults to "Select", not "Initial".
 v.   Bug: from the default, selecting "Initial" does not update Subject and
      Body; the user must switch to "Final" and back. First selection of
      either option must update Subject and Body immediately.

All five email columns exist on dbo.[02_CORE_02_Reviews]
(Portfolio_mgr_lead_email confirmed). RM/PM emails are populated from
Distribution Parties on save / sample load; legacy reviews may have NULLs.

Requirements (generic, don't break anything else):
1. Pre-fill To and CC from the review's latest saved values when the modal
   opens (refetch or use the freshest loaded state, not a stale snapshot).
   Report how unsaved RM/PM changes on screen are handled.
2. Format: "a@x.com; b@x.com". Skip NULL/blank emails, trim, de-duplicate
   case-insensitively (also across To and CC: if an address is in To,
   don't repeat it in CC). No stray or double semicolons.
3. If no email is available for To or CC, leave the field empty with the
   existing placeholder.
4. User can still edit To/CC freely after pre-fill. Pre-fill must not
   overwrite what the user has typed while the modal is open.
5. Document Type opens on "Select"; Subject/Body stay empty (or the current
   neutral default) until a type is chosen. Fix the root cause of the
   Initial bug (likely no value change because the initial state already
   equals "Initial"); don't add a toggle workaround. Selecting Initial then
   Final then Initial must update each time.
6. Send must be blocked (clear message) while Document Type is "Select".
7. Do NOT change the send/attachment logic, memo templates' wording, or
   any other part of the review form.

Report:
- Root cause of the Initial bug (file:line)
- ADDED / REMOVED lines (file:line)
- On-screen behaviour: modal open (To/CC filled), Document Type default,
  first selection of Initial, first selection of Final, switching back and
  forth, Send with "Select"
- NULL cases: review with all five emails NULL; RM email NULL but PM
  present; same address in To and CC
- Build results
Do not commit.
