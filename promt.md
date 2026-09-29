READ-ONLY. After deleting ATHENS PAPER 202604 and doing Add New Month
202604 (before any save), the UI still shows Min Tangible Net Worth
62,297 and Min Net Income 8,369 (202603 values), even after your
SeedFromLatestAsync fix. API was restarted.

Trace EVERY path that can put 202603's covenant values on the 202604
screen, backend and frontend:
- Backend: add-month endpoint, SeedFromLatestAsync, SeedCovenantsFrom
  PreviousMonthAsync, PropagateCovenantSlotUpdatesAsync, summary payload
  (value source per slot), TryMergeCovenantsIntoSeries — does any of
  them read the previous month?
- Frontend: Top Strip tile build, latestPointComputed / latestPoint
  selection (does it pick the latest month WITH data instead of the
  selected month?), latestValueUpToRow / carry-forward, add-month
  valuesTemplate, Monthly Summary covenant columns.
Quote file:line and say which one produces 62,297 for 202604.
Also check whether the running Bcat.Api was started after the last
build (compare process start time vs Bcat.Infrastructure.dll).
Report only.
