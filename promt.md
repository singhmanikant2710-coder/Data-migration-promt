Approved. On point 3 (blast radius widening): accept it as-is, no
conditional logic needed. We've independently confirmed delta-0 is
legacy's own current convention (post-2020, in the actual Access system,
not our invention) — so any old row flipping to delta-0 on edit is
correctly aligning with legacy, not corrupting anything. Apply the diff
now.

After applying: run `git status` and `git diff --stat`, confirm only
SqlMainRepository.cs changed.

SELECT Over30DayPercent, per30DPD, per60DPD, perCashCollections, perIneligiblePercent,
       perIneligiblesDividedByNetFundsEmployed, perNetChargeOff, perOver30DPD
FROM tblMain WHERE strCustomerName='ATHENS PAPER COMPANY INC' AND strMonthKey='202510';

SELECT strMonthKey, intFiscalYear, intFiscalMonth, datFiscalYearStart, intElapsedFiscalDays
FROM tblMain WHERE strCustomerName='NATIONWIDE SPECIALTY FINANCE INC' AND strMonthKey='202601';
