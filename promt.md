Items 2 and 3 — the legacy expressions are pasted below (ground truth,
from Access tblMain Field.Expression). Use them exactly.

<<< PASTE THE FULL FORMULA LIST HERE >>>

Answer to your open question: FCC TTM, Interest Coverage TTM and any
field whose expression uses a TTM / YTD / prior-month / average column
are class B — they keep the LAST SAVED value until Save, even if a
same-row input (e.g. curCPLTDTTM) was edited. Legacy recalculates
nothing live, so B is the safe side.

Also in 2B: trucking.ts:323 Opex YTD — show the stored value only; NULL
renders blank. Remove the client-side monthly-sum fallback.

Everything else as in my previous message (2A, 2B, 3, evidence &
safety, regression on ATHENS + one Trucking + one Auto customer,
report format with ADDED / REMOVED, BEHAVIOUR CHANGE live vs after
save incl. NULL, NOT TOUCHED). Build, tests, do not commit.
