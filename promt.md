Thanks John, that's clear — legacy is the source of truth, and where the template-specific source fields don't exist, the corresponding TTM/YTD simply won't calculate (rather than showing 0). I'll match that behavior exactly. Going ahead with the fix now.

On your questions:

One thing I'd like to confirm with the LOB/business owner (not blocking, but good to align on):
- For the trailing-12-month window specifically, legacy takes the current month plus the previous 11 regardless of fiscal year (it crosses the fiscal year-end). I want to confirm the business expects TTM to keep spanning the fiscal boundary that way — this is the behavior I'll implement, and it's also what fixes the Sept-30 year-end case (#29, BHG). Just want them to confirm that's the intended reading of "trailing twelve months."

On being ready for an update tomorrow:
- Yes, I think we're in good shape. I've confirmed the root cause with database evidence and a legacy side-by-side, the approach is decided (match legacy, populate the TTM/YTD components at save time, year-agnostic), and I'm implementing it now. By tomorrow I expect to have the fix in and tested against the legacy app for at least the first couple of industries (IndirectAuto and Manufacturing), so we can show a concrete before/after.
- Please do invite the business owner — it'd be a good session to confirm the fiscal-boundary point above and walk them through the legacy vs. new comparison.

I'll keep you posted on progress today.

Thanks,
Manikant
