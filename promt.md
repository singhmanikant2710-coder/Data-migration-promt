SINGLE-FILE, BOUNDED EDIT. Only frontend/src/blackbook/components/monthSummaryRegistry.ts. Show unified diff BEFORE applying.

FIX 1 — YTD PBT instant calc (generic, all customers): Currently YTD PBT's render short-circuits on the server value BEFORE calling sumYtdForRow, so live edits don't reflect. YTD Revenue works because it sums FIRST. Invert YTD PBT to match — sum the monthly PBT rows first (formula-driven, works for every customer's fiscal year), server value as fallback.

Apply to BOTH call sites (builder ~L926-942 AND makeColumn ~L1787-1802) identically.

Current YTD PBT render (both sites):
      const directAny = pick(row.values, ["curProfitBeforeTaxesYTD", ...]);
      const directNum = toNumberLike(directAny as any);
      if (directNum !== null) return directNum;
      const sum = sumYtdForRow(series, row, pbtAliases);
      ...

Change to (sum FIRST, like YTD Revenue):
      const sum = sumYtdForRow(series, row, pbtAliases);
      if (sum !== null && sum !== undefined) return sum;
      const directAny = pick(row.values, ["curProfitBeforeTaxesYTD", ...]);
      const directNum = toNumberLike(directAny as any);
      if (directNum !== null) return directNum;
      ... (keep rest of fallback chain unchanged)

Quote YTD Revenue's render to confirm we're matching its exact pattern (sumYtdForRow first, server pick fallback).

VERIFY BEFORE SHOWING DIFF:
a) Both YTD PBT sites: sumYtdForRow first, server fallback.
b) Matches YTD Revenue pattern exactly.
c) pbtAliases unchanged; only render order changed.
d) Both sites (builder + makeColumn) done identically.

Show the unified diff for BOTH sites. Apply nothing until I confirm.
