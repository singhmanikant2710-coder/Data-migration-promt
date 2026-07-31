Subject: CRM PD Grade Migration — clarification on two chart labels and the % column calculations
Hi Geoff,
I've worked through most of your edits on the CRM PD Grade Migration report. Before I implement the last two items, I want to confirm the exact intent so I get the calculations right the first time. I've thought through the logic below and included my recommended interpretation for each.
1. Commitment distribution chart label
You asked to move the count chart from "Scorecard Count" to "Number of Accounts," which I've done. The paired chart on the right is currently titled "PD Distribution by Scorecard Commitment." Since that chart shows dollar amounts ($MM), "Number of Accounts" wouldn't fit there. My recommendation is to simply drop "Scorecard" and title it "PD Distribution by Commitment." Please confirm, or let me know your preferred wording.
2. "% OF COUNT" column — PD Migration Totals by Account table
This table lists one row per FROM PD → TO PD migration, with a COUNT of accounts for each, and a grand total at the bottom (currently 492 in the sample). You asked to add a "% OF COUNT" column as "Row Total / Column Total."
Because this is a flat list (not a cross-tab matrix), "Row Total" and "Column Total" can be read two ways, so I want to confirm which you intend:
Interpretation A (my recommendation): Each row's COUNT ÷ the grand total of all counts. This shows each migration's share of the total. Example: a row with 51 accounts out of 492 total → 51 / 492 = 10.4%. All rows would sum to 100%.
Interpretation B: Some other row/column relationship (e.g., relative to a subtotal by FROM PD).
Interpretation A is the standard "% of total" reading and is what I'd expect for a distribution report. Please confirm A, or clarify if you meant B.
3. "% OF COMMITMENT" column — PD Migration Totals by Commitment table
Same structure, but the value is a dollar commitment per row, with a grand total (currently $492 in $MM terms in the sample). Applying the same logic:
Recommended: Each row's COMMITMENT ÷ the grand total commitment, shown as a %. Example: a row of $85.7M out of a $492M total → 17.4%. Rows sum to 100%.
I'd format both new percentage columns to one decimal place (e.g., 10.4%) and right-align them, consistent with the existing numeric columns.
Once you confirm these three points, I'll finish the remaining changes and send you an updated report to review. Thanks!
Best,
Manikant
