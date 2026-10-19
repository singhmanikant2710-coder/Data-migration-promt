#233 – Covenants / Policy Exceptions Save Errors

Status: Fixed. Tested in Dev and pushed to the develop branch. Not yet deployed to QA.

What was wrong:
The database save logic for both tabs already had the same protection added for Collateral, PSOR and SSOR. The data loss came from the screen side, which was sending values the user had not changed:
- Policy Exceptions: adding a new exception reset the saved Notes to blank and Exception Mitigation to "Not Mitigated". If a review had no exception rows, the stored Notes/Mitigation could also be overwritten on the first save.
- Covenants: saving only the Covenant Information text also wrote "No" into blank Yes/No questions (Tracking Accuracy, Validation Accuracy, Breaches Addressed, Stepped-up Servicing) and could re-save the dates.

What changed:
- Both tabs now save only the fields the user actually changed. Everything else keeps its stored value.
- Stored Policy Exception Notes and Mitigation are never replaced by default values.

What you may notice:
- Screens and PDFs look the same as before.
- In the raw CSV data export, Covenant Yes/No questions that were never answered now show as empty instead of "No"/"False", which reflects that no answer was given.

#235 – Policy Exceptions: E00 default selection

Status: Fixed. Tested in Dev and pushed to the develop branch. Not yet deployed to QA.

What changed:
- The tab now works like Covenants. No row appears automatically in Edit mode; the reviewer clicks "+ Add Policy Exception" and selects E00 (or any exception). E00 now saves on the first selection.
- This removes the automatic E00 row added under #206.
- With no exception rows, Exception Mitigation and Notes are disabled with the hint "Add a policy exception to enter mitigation and notes." Previously these fields looked editable, but anything typed was not saved.

Related issues found and fixed during testing:
- Deleting a newly added, unsaved exception could also delete a saved exception with the same code.
- An exception that was edited and then deleted could reappear after Save.
- The same exception type could be selected twice on one review, which silently overwrote the saved row. Already-used types are now greyed out in the dropdown.
- Notes typed before deleting a row are no longer lost.

Please note:
- Exception Mitigation and Notes apply to the whole review, not to each exception row. A newly added row shows the review's current Notes and Mitigation, and deleting all rows does not clear them.

#241 – CAS Linesheet paragraph spacing

Status: Fixed. Tested in Dev and pushed to the develop branch. Not yet deployed to QA.

What was wrong:
The CAS Linesheet converted finding comments to plain text, removing paragraph breaks and formatting. The Initial/Final Memos did not.

What changed:
- CRM Findings and Observations comments in the CAS Linesheet now display paragraphs, line breaks and rich text formatting the same way as the Initial/Final Memos.
- A very long comment that does not fit on one page now continues onto the next page, with the table header repeated, instead of being cut off or missing from the PDF.
- Automatic word hyphenation (e.g. "how- / ever") is turned off, so text wraps consistently whichever screen the Linesheet is opened from.

Verified in Dev against review Gator Millworks Inc, comparing the CAS Linesheet with the Initial and Final Memos.

Next step for all three: I will confirm here once these are deployed to QA for your testing.
