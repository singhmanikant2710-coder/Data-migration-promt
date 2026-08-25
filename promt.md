READ-ONLY. Read ONE file only: backend/src/Bcat.Infrastructure/SqlServer/SqlMainRepository.cs. Read it ONCE. Do NOT re-read. Do NOT open any other file. Do NOT analyze, do NOT summarize, do NOT give a verdict. Only copy verbatim the exact code blocks asked below, then STOP.

Quote these, each as a verbatim code block exactly as written in the file:

1) The full method RecomputePbtTtmAsync (from its signature to its closing brace) — every line, including the SQL string.

2) The exact line(s) where RecomputePbtTtmAsync is CALLED inside the save/upsert path (the await RecomputePbtTtmAsync(...) call with its arguments), plus the 3 lines above and 3 lines below it.

3) The method RecomputeOtherYtdsAsync (full body, signature to closing brace) — this is the closest existing example of recomputing multiple aggregate columns, I need to see its exact structure.

4) The method TryMergeTtmIntoSeries (full body) — I need to see exactly which TTM column names it reads and from which table (dbo.tblMainTTMCalculations), verbatim.

5) Any helper these methods use to run an UPDATE against tblMain with a windowed SUM(...) OVER(... ROWS BETWEEN 11 PRECEDING AND CURRENT ROW ...) — quote that SQL verbatim (it likely appears inside RecomputePbtTtmAsync; if it's a shared helper, quote the helper too).

OUTPUT: only the five verbatim code blocks, labeled 1-5. Nothing else. Then STOP. Do not propose changes, do not explain, do not loop.
