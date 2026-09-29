REGRESSION — new month copies previous month's values.
ATHENS: Add New Month 202604 shows Min Tangible Net Worth 62,297 and
Min Net Income 8,369 (202603 values), and custom fields AMZN % /
Suppressed Availability also carry 202603 values.

STEP 1 — READ-ONLY, report first:
a. SeedFromLatestAsync slot mirror: quote where dblCovenantActual{i} /
   Formatted is written. Does it use sourceRows (latest month's actuals)
   instead of the NEW month's own tblMainCovenants actual?
b. Legacy Add New Month (cmdAddNewMonth_Click + funSave): quote exactly
   which columns are copied into the new tblMain row — especially
   strCustomField1..10 and covenant slots. Does legacy copy custom
   field values or leave them blank?
c. Our Add New Month path: quote which columns it copies.

STEP 2 — FIX, only what STEP 1 proves:
- Covenant slot actuals for a month = that SAME month's
  tblMainCovenants actual (NULL for a new month). Never previous month.
- Custom fields on a new month: exactly what legacy does (copy or blank).
- Nothing else.

SAFETY (mandatory):
- Regression: ATHENS new 202604 before save and after save; ATHENS 202603
  re-save must stay byte-identical (Min TNW 62,297, Min Net Income 8,369,
  custom fields unchanged); one other industry customer add-month.
- STOP if any existing month's value would change.
Build, tests, do not commit.

REPORT FORMAT: STEP 1 quotes, ADDED/REMOVED (file:line), BEHAVIOUR
CHANGE incl. NULL, NOT TOUCHED.
