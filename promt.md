Apply both hunks exactly as shown in the diff:
- Builder site (~L922-942): YTD PBT render — sumYtdForRow(series, row, pbtAliases) FIRST, then server pick fallback, then TTM chain.
- makeColumn site (~L1783-1802): same YTD PBT render inversion — sumYtdForRow first, server pick fallback, TTM chain.

Use the plain sum-first pattern (matching YTD Revenue exactly). Do NOT use the "sum !== 0" variant — we want full parity with YTD Revenue, and a legitimate zero YTD PBT should render as zero (sum), not fall through to server.

Keep pbtAliases, the hasPbt/hasYtdPbtExact gate, the TTM fallback chain, and the final return null unchanged. Both sites identical.

Apply now, then run typecheck/build and report any errors.
