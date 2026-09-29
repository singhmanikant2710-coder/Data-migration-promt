FIX — Blackbook edit page live preview + one data-loss bug.
Attached: ground-truth legacy calculated-field expressions (from Access
tblMain Field.Expression). Use these, not the "Inferred" doc.

1. M7 (priority, data loss): curTotalOperatingExpensesYTD is a manual
   input in legacy (frm001 enabled; funSave never derives it;
   qryMainYTDCalculations_003 is never called). Stop deriving/overwriting
   it on save (RecomputeRevenueAndGrossProfitYtdAsync targets,
   ~3973 / 4044-4061), and make sure the user's input is saved (check
   IsDerivedColumn blocklist). Quote evidence.

2. Live preview rule (edit page, all industries):
   A. A field updates LIVE only if its legacy expression uses same-row
      inputs only (no YTD / TTM / prior-month / average columns). Compute
      it with the EXACT attached expression (IIf zero guard, Int, Round
      banker's, Null handling) from the current edited row values.
   B. Every other derived field (YTD, TTM, prior-month, averages, and
      any field whose expression uses one of them — e.g. A/R Turn Days,
      Inventory Turn, GPM YTD, Operating Ratio YTD, NI YTD/Rev YTD, FCC
      TTM, Interest Coverage TTM, Net C/O %, Net C/O TTM %, Reserve
      Coverage, Gross A/R Turn, Portfolio Yield, Cash Collections %) shows
      the LAST SAVED value until Save. No client-side estimate.
      - Stop injecting the edited month into rolling24WithEdits for TTM
        cells (M1).
      - Edit-page YTD columns show the stored YTD while editing (M2),
        then refresh from the DB after Save.
   Produce a table: field | expression | class A/B | live? before/after.
3. Diff tblMainCalcs.ts (and TblMainCalcs.cs) against the attached
   expressions; list and fix any formula mismatch.

EVIDENCE & SAFETY (mandatory):
- No change to backend save calculations except item 1.
- Regression: ATHENS (Manufacturing/Wholesale profile), one Trucking
  customer (Opex YTD), one Auto customer. Edit a field -> live values
  per rule -> Save -> all values equal the DB (queries).
- STOP if any saved value would change except Opex YTD.
Build, tests, do not commit.

REPORT FORMAT (mandatory):
- ADDED / REMOVED (file:line)
- BEHAVIOUR CHANGE (live vs after save) incl. NULL case
- NOT TOUCHED
