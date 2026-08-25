Hi John,

I wanted to give you a detailed update on the TTM/YTD defects you and the team reported during BCAT UAT. I've done a full root-cause investigation, compared the new application against the legacy Access application side-by-side, and confirmed the exact source of the problem. Sharing everything here so you have full visibility before I proceed.

DEFECTS COVERED
- #35 (Steven Garrison, 8/11): "The trailing twelve months values are not pulling for me when I enter new months, but it shows for past months it was capturing them."
- #31 (John Halsrud, 8/5): "After inputting June figures, I don't see Interest Coverage TTM, YTD Net C/O $, Net C/O TTM %, Reserve Coverage populate for the month."
- #30 (John Halsrud, 8/5): "After inputting principal NR, cash collections, and 60+ DPD, the cash collections % and 60+ DPD % are not captured."
- #29 (John Halsrud, 8/5): "Bankers Healthcare Group has a September 30th year end. After inputting month revenue, PBT, and interest expense, it was not captured in YTD revenue, YTD PBT, or interest coverage TTM."

WHAT I FOUND (root cause, confirmed with data)
1. The raw monthly values you enter (Interest Expense, Net C/O, Principal N/R, Cash Collections, etc.) ARE being saved correctly. I verified this directly in the database — every month you entered has the correct monthly figure stored.

2. The problem is with the trailing-twelve-month (TTM) and year-to-date aggregate values. These are stored separately, and for a newly entered month they are staying at 0. Because the ratios depend on them (for example, Interest Coverage TTM = EBIT TTM / Interest Expense TTM), when the denominator is 0 the whole ratio shows 0.00x. That is exactly why past months (which already had these aggregates) display correctly, while newly entered months show 0 — matching what Steven and the team observed.

WHAT I CONFIRMED AGAINST THE LEGACY APPLICATION
To make sure any fix matches the legacy behavior exactly, I exported the legacy Access application's calculation logic and compared it directly to the new app:
- In legacy, the TTM values (Interest Expense, PBT, Net Charge-Off, Depreciation, and related components) are all computed together as a 12-month trailing sum and stored, every time a month is refreshed.
- The legacy 12-month window is "trailing" and spans across fiscal-year boundaries — it simply takes the current month plus the previous 11, regardless of fiscal year.
- The new application reads from the same place the legacy values were stored, but it is not re-computing and re-populating those TTM values when a new month is entered — so they remain 0 for new months.

WHY #29 (September 30 year-end) IS LINKED
The new app currently computes one of these aggregates only within the current fiscal year, whereas legacy computes it across the year boundary. For a customer like Bankers Healthcare Group with a Sept-30 year end, this means the trailing window gets cut off at the boundary, which is very likely why the YTD/TTM values didn't capture for that customer. I've factored this into the analysis.

WHAT I'M PROPOSING
A fix that recomputes the full set of TTM/YTD values exactly the way legacy does (trailing 12 months, across fiscal-year boundaries) at the moment a month is saved. This one correction should resolve #35, #31, and #30 together, and should also address #29.

I've already applied and verified the first, smaller part of the fix (the values now flow through correctly for the current month while editing, with no impact to existing months). The remaining part is the calculation piece, which I'm validating carefully.

MY ONE OPEN QUESTION FOR YOU
Since every industry (IndirectAuto, Manufacturing, Consumer Finance, etc.) has its own layout and some industry-specific fields, I want to confirm one thing to guarantee I match your expectations exactly:

For a newly entered month, should the TTM values behave exactly as they do in the legacy Access app in every case — i.e. legacy is the source of truth I should match field-for-field — or are there any TTM/YTD fields where you'd expect the new app to behave differently from legacy?

If legacy is the definitive reference (which is my assumption), I'll match it precisely and test each affected industry against the legacy app before marking these resolved.

Happy to hop on a quick call and walk through the legacy vs. new comparison if that's easier.

Thanks,
Manikant
