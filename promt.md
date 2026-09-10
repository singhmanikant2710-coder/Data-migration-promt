Apply the Option A diff exactly as shown in generic.ts:
Replace the two lines (Net C/O $ + YTD Net C/O $) with:
    { label: "YTD Net C/O $", value: sumYtd(currentYearSeries, latestPoint, netCoDollarAliases) ?? pick(v(latestPoint), ytdNetCoDollarAliases), kind: "currency" },
    { label: "TTM Net C/O %", ... }

Wait — the diff shows removing the "Net C/O $" line too. Confirm: the diff REPLACES both "Net C/O $" and "YTD Net C/O $" lines with just the YTD line? Or does it keep "Net C/O $" and only change "YTD Net C/O $"? 

Quote the exact before/after so I confirm we're NOT accidentally removing the "Net C/O $" input tile (which must stay — it's the editable field). We only want to change the YTD Net C/O $ line to sum; the Net C/O $ line must remain.

Show the corrected diff if needed, then apply.
