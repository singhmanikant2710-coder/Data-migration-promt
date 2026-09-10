SINGLE-FILE, BOUNDED EDIT. Only frontend/src/blackbook/mappings/generic.ts. Show unified diff BEFORE applying.

ISSUE (E3): The Cash & Charge-offs panel has its own "YTD Net C/O $" tile (generic.ts ~L386) that is a plain pick(v(latestPoint), ytdNetCoDollarAliases) — pure server passthrough. It shows $0 while the Top Strip YTD Net C/O shows $50,473 (which now sums correctly via Fix 2). They disagree.

FIX: Make the in-panel YTD Net C/O $ sum the monthly Net C/O too, consistent with the Top Strip.

BUT NOTE (from prior analysis): the generic mapper is invoked with the RAW `series`, not seriesWithEdits, so a sum inside the mapper won't reflect in-flight edits until saved. 

So there are two options — tell me which:
OPTION A (make it sum, reflects on save): Change generic.ts L386 YTD Net C/O render/value to sum monthly Net C/O via sumYtdForRow (if available in that file) or an equivalent monthly sum, matching the Top Strip aliases ["curNetChargeOff","NetChargeOff","NetCO","NetChargeOffDollar"]. It will match after save (mapper uses raw series).

OPTION B (drop the duplicate tile): Remove the "YTD Net C/O $" tile from the generic middle panel entirely, so the Top Strip is the single source of truth (no disagreement). Cleaner — no two widgets showing different values.

First QUOTE:
1) The generic.ts YTD Net C/O $ tile line (~L386) + ytdNetCoDollarAliases.
2) Whether sumYtdForRow (or any monthly-sum helper) is importable/available in generic.ts.
3) How the mapper receives series (raw vs seriesWithEdits) — confirm it's raw at the call site.

Then recommend A or B based on what's cleanest and lowest-risk, and show the diff for the recommended option.

No apply yet. Findings + recommended diff.
