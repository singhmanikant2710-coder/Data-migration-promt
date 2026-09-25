IMPLEMENTATION — Batch 6, covenant display. Same rules as before:
change only what's listed, no refactors, build + unit tests after each
step, STOP and report on any mismatch, do not commit.

RULE (legacy parity): every place that shows a covenant VALUE shows
ACTUAL from tblMainCovenants, formatted by strCovenantFormat.
The view covenant tile that is explicitly a threshold/limit display
stays as-is.

6a Backend SqlMainRepository: for the 7 keys, emit ACTUAL, not
   threshold ?? actual (1445-1447 / 1500-1502 and 1989-1991 /
   2034-2036). If actual is missing, emit null (legacy shows blank).
   Fix the SMR:1427 comment to match.
6b Remove the computed quotient override for MinTNW
   (computeMinTnwForRowLocal MST:765, computeMinTnwForRow REG:1577 and
   REG:2011). Do not delete the helper functions, just stop calling them.
6c view/page.tsx covNumeric: ACTUAL first (same as edit/report).
   view enrichment 785-789: fill only when null (same as edit/report).
6d MonthSummaryTable 763/981: use mapKind(f.format) from the payload
   instead of hardcoded "currency", same as Top Strip.
6e Dead aliases: add "MaxSeniorDebtTNW" to directAuto.ts:246; add
   "MaxCARatio" to energyRelated.ts:125.

6f READ-ONLY check: SQL Server has no MinTNW covenant row with
   strCovenantFormat = '%'. So where does the Top Strip get "%" for
   MinTNW? Check the format inference in BlackbookSummaryService
   303-321 (ThresholdText/label fallback) and whether slot N's actual
   can be paired with another slot's format. Quote file:line. If it is
   the inference, make it return '$' when strCovenantFormat is '$' or
   empty for a currency covenant — report before applying.

DO NOT TOUCH: MinFCC/MaxDilution render branches, ratio "x" suffix,
MinPBT negative formatting, carry-forward logic, AccessMainRepository.

Final report: files + lines changed, build, tests, and exact UI steps
to check one customer's covenants on Top Strip, Monthly Summary, view
page and report.
