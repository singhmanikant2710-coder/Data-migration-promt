#247 – Implemented. Tested in Dev and pushed to the develop branch. Not yet deployed to QA.

What changed:
- Relationship Overview (Customer Info) now has a "Special Assets Managed" Yes/No drop-down under Unit, showing CORE_Reviews.Special_assets.
- Changing it updates Special_assets on all accounts of the review. Changing the Special Assets checkbox for any account in Transactions > Account Details updates the customer-level value and all other accounts of the review. A short note under both fields explains that the change applies to all accounts.
- New accounts added through Add Account now default to the customer's current Special Assets value.
- If, in one save, Special Assets is set to Yes in one place and No in another, the save is stopped with a message and nothing is changed, so the reviewer can correct it.
- Existing data is never changed automatically: values are only synced when a reviewer actually changes Special Assets. Saving other fields leaves Special Assets exactly as stored.

Reports (your consideration): three reports – CRO Production Summary, CRM Findings & Observations and Covenants Summary – filtered Special Assets at the account level (Yes if any account was Yes). They now use the customer-level value, like the other reports. In Dev this changes the result for one review only: 21541 (1201 EDWARDS MILL ROAD LLC), where the customer level is No but 3 of 4 accounts are Yes – an example of the data-integrity case you described. Once a reviewer sets Special Assets on that review, all its accounts will match.

Tested in Dev: customer-level change (all accounts updated), account-level change (customer and all accounts updated), conflicting values in one save (blocked), saving other fields (no change), Add Account default, and the three reports with the filter set to Yes and No.

I will confirm here once deployed to QA.
