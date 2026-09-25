Save now takes 1-2 min after Batch 5. Keep results identical, make it fast:

1. Add Stopwatch timing logs per step in the save path ([PERF] prefix),
   so we can see before/after.
2. Read INFORMATION_SCHEMA columns ONCE per save and reuse them in the
   X..X+11 loop instead of probing on every iteration.
3. Mirror UPDATE and RecomputePbtTtmAsync: compute the window over rows
   from (X - 11 months) onward, but UPDATE only months X..X+11. Earlier
   months don't change.
4. Remove the call to RecomputePbtTtmAsync at 1264 — the mirror now
   writes curProfitBeforeTaxesTTM with the same value.
5. Remove all [TTM DIAG] logs.

Build, run unit tests, report the [PERF] numbers for one save.

