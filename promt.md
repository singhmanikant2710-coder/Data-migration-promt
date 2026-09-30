Hi Geoff,

Sounds good — thank you for reconciling the list against the full FHN
Associate list. Agreed, NULL is the right outcome for RMs/PMs no longer with
the bank, and the 565 additions should close most of the remaining gap once
the tables migrate Friday.

Your note about Employee IDs mapping to 2+ names is worth guarding against
going forward, since our email/name resolution relies on Employee ID being
unique in Distribution Parties. I'll ask Ashok about adding a uniqueness
check on Recipient_role as part of Friday's migration.

We also verified the application end-to-end against Ashok's schema change
(Employee IDs now stored as zero-padded text). Everything works on our side,
with no code changes needed. I'm sending Ashok a few small trigger fixes to
make sure the zero-padding stays consistent.

Thanks,
Manikant


Hi Ashok,

We verified the app against your schema change (nvarchar(10) + zero-padding
trigger). Everything works on our side. A few trigger issues to fix before
Friday's Prod migration:

1. Comparison type: the trigger uses [Relationship_mgr_number] <> 0 (int
   comparison against an nvarchar column). If a non-numeric value ever lands
   in the column, this throws a conversion error (Msg 245) and blocks EVERY
   update to that review row, including unrelated saves. Please change it to
   <> '0' (string comparison).

2. Employee ID 0 / default '0': the trigger skips '0', so it stays '0' while
   existing rows hold '00000', which gives two encodings for the same value.
   The column DEFAULT '0' has the same gap. Suggest defaulting to NULL
   instead of '0', or padding '0' to '00000'.

3. Trailing spaces: LEN() ignores trailing spaces, so a value like '30  '
   is never padded. Please use RTRIM() in the SET (or check DATALENGTH()/2).
   No such rows exist today; this is preventive.

4. Performance: the triggers fire on every UPDATE to 02_CORE_02_Reviews
   (about 20 different save statements). Adding
   IF NOT UPDATE([Relationship_mgr_number]) AND NOT UPDATE([Portfolio_mgr_number]) RETURN;
   would skip the unnecessary work.

5. Unique constraint on Recipient_role: Geoff found Employee IDs mapped to
   2+ names in the source list. Our resolver depends on the ID being unique,
   so a unique constraint/index on Recipient_role in Distribution Parties
   would prevent this from recurring.

6. Prod: please confirm the same schema change + trigger will be applied in
   Prod as part of Friday's migration, BEFORE the Distribution Parties data
   is loaded, so the new 1,495 rows get padded correctly.

Data cleanup (1 row): one review has Portfolio_mgr_number = '00000' with a
non-NULL Portfolio_mgr_name. It displays as "0 - NAME" in the UI. Can you
set both to NULL to match the sample-load NULL-on-miss behavior?

Thanks,
Manikant


Context: CASRR. Employee IDs are now nvarchar(10), zero-padded to 5 chars by
a DB trigger. Real IDs are at most 5 digits.

Fix (generic, must not change behaviour for valid 1–5 digit IDs):
1. frontend/src/app/maintenance/distribution-parties/page.tsx:32-35:
   lower EMPLOYEE_ID_MAX to 99999 and maxLength to 5, so a 6+ digit ID is
   rejected with a validation message instead of silently saved unpadded.
2. SqlDistributionPartiesRepository.cs:120,144: bind @role as
   NVarChar(10) instead of NVarChar(255), and correct the doc comment at
   line 18.
3. Do NOT change SqlDbType.Int bindings in SqlReviewRepository.cs:1344/1351
   (works correctly via trigger; optimization only, out of scope).
4. Do NOT touch the dead lookup endpoint GET /api/v1/lookups/
   distribution-party-names; just list it in your report.

Report back:
- ADDED: every line added (file:line)
- REMOVED: every line removed/changed (file:line)
- On-screen behaviour change (what the user sees for a 6-digit ID now vs
  before, and for a valid 5-digit ID — should be unchanged)
- NULL/empty case: what happens when Employee ID is left blank on
  Add/Edit (must behave exactly as before)
- Build result. Do not commit/push until I review.
