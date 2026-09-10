READ-ONLY. Find why editing Net C/O in the "Cash & Charge-offs" section does NOT update YTD Net C/O, while editing it in the Summary (Top Strip) DOES. Quote with paths.

CONTEXT: We fixed YTD Net C/O $ to sum monthly Net C/O (Fix 2, in monthSummaryRegistry.ts — top-strip + makeColumn). Editing Net C/O in the Top Strip now updates YTD Net C/O. BUT editing Net C/O in the "Cash & Charge-offs" section does NOT update YTD Net C/O.

1) Find the "Cash & Charge-offs" section render (likely a DetailGrid or a separate panel component). Where is its "Net C/O $" input? Quote it. What field key does its onChange write to (which alias — curNetChargeOff, NetCO, etc.)?
2) Compare with the Top Strip's "Net C/O $" input — what field key does IT write to? Are they the SAME key or DIFFERENT?
3) The YTD Net C/O sum (Fix 2) uses netChargeOffMonthlyAliases = ["NetChargeOff","NetCO","NetChargeOffDollar","curNetCO","curNetChargeOff"]. When the Cash & Charge-offs section writes Net C/O, does it write to one of THESE aliases (so the sum picks it up), or to a different key not in this list?
4) Does the Cash & Charge-offs section use seriesWithEdits (which runs the recompute loop) OR a separate data path that doesn't feed into the YTD sum? Quote how the Cash & Charge-offs panel gets/writes its row data.
5) Does the Cash & Charge-offs section have its own YTD Net C/O render (separate from monthSummaryRegistry), and does THAT one sum or just passthrough?

OUTPUT:
- A) Cash & Charge-offs "Net C/O $" input + the key its onChange writes, quoted.
- B) Top Strip "Net C/O $" input + its key, quoted. Same or different key?
- C) Does Cash & Charge-offs' Net C/O write to an alias in netChargeOffMonthlyAliases (so YTD sum sees it)? Or a different key?
- D) Root cause: (i) different field key (edit not picked by sum), (ii) separate data path (not seriesWithEdits), or (iii) separate YTD render that doesn't sum.
- E) Exact fix so both edit locations update YTD Net C/O consistently.
- No fix. Findings only.
