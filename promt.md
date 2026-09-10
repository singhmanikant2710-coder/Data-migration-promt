Apply all the FIX 2 hunks exactly as shown:
- Add netChargeOffMonthlyAliases const (monthly Net C/O aliases, canonical in-file list).
- extraCols builder: new "YTD Net C/O $" branch — sumYtdForRow(series, row, netChargeOffMonthlyAliases) first, server pick(ec.aliases) fallback.
- makeColumn: matching "YTD Net C/O $" case with the same render.

Use the plain sum-first pattern (full parity with YTD Revenue / YTD PBT). Do NOT change the visibility gate (keep it as-is so no new column appears on other customers' screens). Both sites identical.

Apply now, then run typecheck/build and report any errors.
