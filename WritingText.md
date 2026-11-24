#259 – Fixed. Tested in Dev and pushed to the develop branch. Not yet deployed to QA.

What was wrong: The Final Memos (Consolidated) report used a different rule from the Review Form Final Memo, so it included CRM Observations (and Key Findings) from the CRM components. It also ignored all Hierarchy, Portfolio and General filters, and only ran when a Sample was selected. In addition, Export Report for this report did not produce a file.

What changed:
- The consolidated report now uses exactly the same rule as the Review Form Final Memo: CRM Findings from Risk Recognition through Loan Administration, plus Servicing Systems and Data Integrity Observations. Both reports share the same logic, so they will stay aligned.
- All report filters now apply (Hierarchy, Portfolio, General, Review Status with dates, Special Assets, CCL, Credit Review Officer, CAS Manager, Approving Manager). Only finalized, non-cancelled reviews are included.
- The report runs with a Sample or any filter (e.g. a single business unit for quarterly runs). With nothing selected, a message asks you to choose a Sample or filter. Start/End dates apply together with a Review Status.
- If more than 500 reviews match, a message asks you to narrow the filters instead of producing an incomplete report.
- The Applied Report Filters page now shows the filters used, and Export Report now downloads the Excel file with the same reviews as the PDF.

Tested in Dev: a review with CRM Observations (now excluded, matching the Review Form Final Memo), a Unit filter (count matched the database), the Excel export, and running with nothing selected.

I will confirm here once deployed to QA.
