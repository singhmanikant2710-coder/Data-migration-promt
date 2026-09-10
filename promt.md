SINGLE-FILE, BOUNDED EDIT. Only frontend/src/blackbook/components/monthSummaryRegistry.ts. Show unified diff BEFORE applying.

FIX 2 — YTD Net C/O $ instant calc (generic, all customers): Currently "YTD Net C/O $" is only an alias-entry in extraCols (L500) with no render function — it falls to the generic else (L1506), a bare alias pick, so it never sums monthly Net C/O and doesn't reflect live edits. Make it a rendered column that sums the MONTHLY Net C/O values (like YTD PBT/Revenue), server value as fallback.

IMPORTANT: it must sum the MONTHLY Net C/O aliases (cur NetChargeOff etc.), NOT the YTD aliases it currently carries in extraCols.

1) In the extraCols builder loop, add an "else if (ec.label === 'YTD Net C/O $')" branch (alongside the existing "TTM Net C/O %" branch ~L1364) that pushes a column with render:
    render: (row) => {
      const sum = sumYtdForRow(series, row, ["curNetChargeOff", "NetChargeOff", "NetCO", "NetChargeOffDollar", "cur NetChargeOff"]);
      if (sum !== null && sum !== undefined) return sum;
      return pickExact(row.values, ec.aliases);   // fallback to server YTD aliases
    }
   (Use the MONTHLY Net C/O alias list for the sum — quote the exact monthly Net C/O alias names used elsewhere, e.g. in per60DPD/perNetChargeOff or the Net C/O $ tile, so we match them.)

2) Add the matching makeColumn case for "YTD Net C/O $" with the SAME render (sum monthly first, server fallback), so top-strip and Detail grid agree.

First, QUOTE:
- The current extraCols entry for "YTD Net C/O $" (L500) and its aliases.
- The monthly "Net C/O $" tile/column render (to get the exact MONTHLY Net C/O alias names to sum).
- The existing "TTM Net C/O %" branch (L1364) as the pattern to mirror for adding a new branch.

Then show the diff:
- extraCols builder: new branch for "YTD Net C/O $" (sum monthly, fallback server).
- makeColumn: new/updated case for "YTD Net C/O $" (same render).

VERIFY BEFORE SHOWING DIFF:
a) sumYtdForRow uses MONTHLY Net C/O aliases (not YTD aliases).
b) Both sites (extraCols builder + makeColumn) have the same render.
c) Server YTD alias pick remains as fallback.
d) Nothing else changed.

Show the unified diff. Apply nothing until I confirm.
