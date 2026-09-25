Correction to the previous analysis, confirmed against Access:
Access has the SAME stale values in tblMain.dblCovenantActual{N}
(ATHENS 202510 = 44469197, 202604/202605 = 62297) while
tblMainCovenants.strCovenantActual is NULL for those months. Legacy
still shows blank because its form displays dblCovenantActual{N}Formatted,
which is built from tblMainCovenants on save.

RULE: tblMainCovenants.strCovenantActual is the source of truth for
covenant values. tblMain.dblCovenantActual{N} must never override it.

1. SqlMainRepository: when a tblMainCovenants row exists for
   customer + month + covenant name, use its actual for the canonical
   key (MinTangibleNetWorth etc.) AND for that slot's
   dblCovenantActual{N} / dblCovenantActual{N}Formatted in Values —
   including NULL. Fall back to tblMain slot values only when no
   tblMainCovenants row exists. Change TryMergeCovenantsIntoSeries
   (:2043) from "fill only missing/zero" to authoritative.
2. BlackbookSummaryService (:293, SummaryPayload / Top Strip): same
   rule — read actual from tblMainCovenants, not dblCovenantActual{slot}.
3. SqlMainRepository.SeedCovenantsFromPreviousMonthAsync (3167-3259):
   do not copy dblCovenantActual{i} / Formatted into a new month.
Do NOT modify data. Leave AccessMainRepository as-is.
Build, tests, do not commit. Report files + lines.
