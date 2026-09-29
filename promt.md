FINAL FIX — backend root cause. Frontend patching is not enough.

Root cause (your own finding): BlackbookSummaryService.cs:135 calls
GetMainRowValuesAsync, which returns the latest month <= requested when
the requested month has no data, so the summary payload for a new month
(e.g. ATHENS 202604) carries 202603 covenant actuals and custom fields
(:325 ReadFirst(values, slotCandidates), :359 custom fields). Every page
(edit, view, report/PDF) consumes this payload.

FIX (legacy parity — legacy never shows another month's values):
1. The summary payload for month X uses month X's row ONLY (exact match).
   If X has no row or no value -> NULL. Remove the "<= requested"
   fallback for this payload. Set payload.monthKey = X.
2. Check every other caller of GetMainRowValuesAsync; if any relies on the
   fallback, keep a separate method for it — do not change their behaviour.
3. Bump payloadVersion so no cached payload is served.
4. Keep the frontend payloadForMonth guard (harmless extra safety).

RULES: read-only check of callers first; smallest change; no change to
calculations or data.
GOLDEN (before AND after): ATHENS 202603 Min TNW 62,297, Min Net Income
8,369, AMZN % / Suppressed Availability unchanged; ECLIPSE 202604;
WESTLAKE 202604 Other 1 (%) 9.06; MIDDLE GEORGIA 202011 Net C/O TTM
7.03%; ATHENS new 202604 on edit, view and PDF: covenants + custom "—".
If any existing-month value changes, STOP.
Build, tests, do not commit.
REPORT: callers of GetMainRowValuesAsync, ADDED/REMOVED (file:line),
BEHAVIOUR CHANGE incl. NULL, golden table, NOT TOUCHED.
