Thanks John, that's helpful — good to know datFiscalYearStart is behaving correctly, and the quarterly-reporting note is useful context I hadn't accounted for.

But I realize my question was about a different field, and I should've been more precise. Not datFiscalYearStart (the actual calendar date the fiscal year begins) — I'm asking about intFiscalYear, the numeric LABEL assigned to that fiscal cycle.

Concrete example from the row you screenshotted: strMonthKey 202010, datFiscalYearStart = 10/1/2020 (so this fiscal year runs Oct 2020 – Sep 2021). Should intFiscalYear for that row be:
(a) 2020 — named for the year it STARTS in, or
(b) 2021 — named for the year it ENDS in?

For Athens Paper and 4+ other customers we've checked, it's (b) — e.g. their Oct-2025-to-Sep-2026 cycle is labeled "FY2026." But Bankers Healthcare's actual stored intFiscalYear for this same row is 2020, i.e. (a) — the opposite.

Is that intentional for Bankers Healthcare (and Keystone Private Income Fund, Vermeer Mountain West, Nationwide Specialty Finance — same pattern), or should it also be labeled 2021 like the others?
