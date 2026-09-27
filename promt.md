GENERIC FIX (all customers/industries): Monthly Summary must show only
months up to the month selected in the month dropdown, like legacy.
Example ATHENS: select 202602 -> Monthly Summary shows 202510-202602
(not 202603). Same for the view page and any grid fed by the selected
month (current-year, prior-year, Rolling 24 end month, Detail grid).
PDF Fiscal YTD already filters <= selected month — keep consistent.

1. READ-ONLY first: quote where each grid gets its rows and whether it
   filters by the selected month.
2. FIX: filter rows to monthKey <= selected month (and for Rolling 24,
   the 24 months ending at the selected month). Do not change values.
Build, tests, do not commit.

REPORT FORMAT (mandatory):
- ADDED / REMOVED (file:line)
- BEHAVIOUR CHANGE incl. NULL case
- NOT TOUCHED
