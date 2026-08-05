Great — starting on Option B. I've mapped out the data and I can build the "Changes" and "% Change" rows entirely in the report (no backend changes needed). I've got the reordering underway too.
Before I code the two new rows, I want to confirm the exact calculations so they match how you originally built them — since you set these up, you'll know the intended formula:
"Changes" row (per CAS PD column):
My understanding: for each CAS PD column, "Changes" = the total in that column minus the diagonal (no-change) cell — i.e., the off-diagonal movement, which represents the actual grade changes (upgrades + downgrades) landing in that CAS PD. Is that right?
"% Change" row:
This is where I want to be sure of the denominator. Should "% Change" for each column be:
(a) Changes ÷ that column's total — i.e., what portion of that CAS PD's exposure/count was an actual change vs. no-change, or
(b) Changes ÷ the grand total — i.e., that column's changes as a share of the whole matrix?
Whichever matches your original design — just let me know which one.
Also confirming the column labels: 13 / SM, 14 / SUB, 15 / DFUL, 16 / LOSS (Special Mention, Substandard, Doubtful, Loss). Correct?
Once you confirm the "% Change" basis and the labels, I'll build both rows for the Count and Commitment matrices and verify they tie out.
Thanks!
Manikant
