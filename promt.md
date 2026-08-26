There is ONE typo to fix. In TryMergeTtmIntoSeries, this line is wrong:

dict["curCPLTDTMM"] = Read("curCPLTDTMM", "CPLTDTMM");

It has "TMM" but it must be "TTM". Change it to exactly:

dict["curCPLTDTTM"] = Read("curCPLTDTTM", "CPLTDTTM");

Change every "TMM" to "TTM" on that one line: the dict key, and both Read arguments. So:
- curCPLTDTMM  ->  curCPLTDTTM
- "curCPLTDTMM"  ->  "curCPLTDTTM"
- "CPLTDTMM"  ->  "CPLTDTTM"

Do NOT touch any other line. After the change, show me that single line so I can confirm it reads curCPLTDTTM (with TTM, not TMM).
