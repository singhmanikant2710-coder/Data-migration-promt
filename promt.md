BUG (recurring): ATHENS Add New Month 202604 — before any input, Min
Tangible Net Worth, Min Net Income (covenants) and AMZN %, Suppressed
Availability (custom fields) show 202603 values. DB for 202604 is NULL
for all of them; API responses for 202604 have no value. So the
FRONTEND fills the new month from 202603 (or from a client cache).

STEP 1 — READ-ONLY, quote the exact line that produces the 202603 value:
- latestPoint / latestPointComputed selection (edit/page.tsx) falling
  back to the latest month with data instead of the selected month
- Top Strip covenant tile slot fallback reading latestPointComputed
- custom-field tiles: any fallback to an earlier row / latest value
- carry-forward (latestValueUpToRow / pick over earlier rows)
- client caches: does the new POST /api/v1/main/month path call
  invalidateCustomerCaches (summary memo, GET cache, lookups) like the
  save paths do? If not, the page renders a cached 202603 payload.
- monthlyTopStrip memo deps (latestPointComputed missing)

STEP 2 — FIX (generic, legacy parity):
- Every tile/column shows the SELECTED month's value only; missing -> "—".
- Call invalidateCustomerCaches after POST /api/v1/main/month and reload
  the payload/series for the new month.
- Add the missing memo dependency.

RULES: read-only first; smallest fix; frontend only unless STEP 1 proves
otherwise; no change to calculations/data.
GOLDEN (before AND after): ATHENS 202603 Min TNW 62,297, Min Net Income
8,369, AMZN % / Suppressed Availability unchanged; ECLIPSE 202604;
WESTLAKE 202604 Other 1 (%) 9.06; ATHENS new 202604 all "—"/0 before
input. If any existing-month value changes, STOP.
Build, tests, do not commit.
REPORT: root cause (file:line), ADDED/REMOVED, BEHAVIOUR CHANGE incl.
NULL, golden table, NOT TOUCHED.
