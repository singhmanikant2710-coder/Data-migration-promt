Context: CASRR (.NET 8 + Next.js/React/TypeScript + SQL Server). Production banking app: existing behaviour must not break.

BUG (found while testing #212): Review 20120. DB has Review_approval_date, Review_distributed_date and Review_finalized_date all = 2026-10-05. On the Review Form (Review Status grid, approved/locked review), MGR APPROVAL shows 10/5/2026 (correct) but DISTRIBUTED and FINALIZED show 10/4/2026 and the calendar highlights the 4th. My machine is IST (UTC+05:30); the bank's users are in US Central (UTC-06:00/-05:00). This is a date-only value shifted by the timezone.

Phase 1 (read-only): trace with file:line how distributedDate/finalizedDate travel API → frontend → DateInputWithCalendar (value parsing, display formatting, picker selection, onChange output), and compare with how MGR APPROVAL is formatted (it's correct). Identify the exact conversion causing the shift (e.g. new Date("YYYY-MM-DD") parsed as UTC, toISOString().slice(0,10), DateTimeKind/"Z" from the API). List every other screen/field using the same component or helper and whether it has the same bug (CRO Start/Complete, Second Review Date, Prior Review Date, Samples, Load Samples, Reports date filters).
Also confirm whether a shifted value can be SAVED back (e.g. user opens the calendar or locked-review save path) — i.e. is there data-corruption risk.

Fix (generic, smallest): treat these as date-only values end to end — parse "YYYY-MM-DD" (or the API date string) as a LOCAL calendar date, format without UTC conversion, emit "YYYY-MM-DD" from local Y/M/D. Fix it once in the shared component/helper so every caller is correct; do not special-case fields. Must show the same date for users in IST and in US Central. No backend change unless the API sends a timezone-shifted value (then report and ask).

Verify with real functions under TZ=Asia/Kolkata AND TZ=America/Chicago (and UTC): display, picker selection, round-trip save for 2026-10-05, month/year boundaries (2026-01-01, 2026-12-31), DST dates (2026-03-08, 2026-11-01), empty/NULL.

Report: root cause (file:line), ADDED / CHANGED / REMOVED (file:line), before vs after per timezone, other fields affected/fixed, data-corruption risk found (yes/no + which path), build results, git status. Do not commit or push.
