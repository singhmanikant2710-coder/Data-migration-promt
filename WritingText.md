#252 – Fixed. Tested in Dev and pushed to the develop branch. Not yet deployed to QA.

What was wrong: The monthly table grouped reviews by month only and ignored the year. It always showed January–December and labeled every column with the start date's year. For 7/1/2025–6/30/2026, the January–June 2026 counts appeared under January–June 2025. The totals were correct, but the column headers were not.

What changed:
- The columns now follow the selected date range in order, with the correct month and year (e.g. JUL 2025 … JUN 2026).
- Only the months in the range are shown (e.g. 7/1/2026–9/30/2026 shows JUL, AUG and SEP 2026).
- A range longer than 12 months is split into tables of up to 12 months each, with a TOTALS column per table and a GRAND TOTAL on the last table.
- If only one date (or no date) is entered, the columns cover the months of the reviews returned.

Tested in Dev: 7/1/2025–6/30/2026, 7/1/2026–9/30/2026 and an 18-month range; reviewer totals matched a direct database count.

I will confirm here once deployed to QA.
