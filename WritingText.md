#251 – Review Progress: Status Date Column

Status: Fixed. Tested in Dev and pushed to the develop branch. Not yet deployed to QA.

What was wrong:
When the Borrowers Sampled box (All Status) was selected, the date column header read "Completed", even though each row shows the date for its own status (for example, the cancelled date for Cancelled reviews and the approval date for Approved reviews).

What changed:
- When Borrowers Sampled / All Status is selected, the date column header now reads "Status Date".
- The headers for all other status boxes are unchanged.
- The dates shown in the column, sorting and filtering are unchanged; this is a label change only.

Next step: I will confirm here once this is deployed to QA for your testing.
