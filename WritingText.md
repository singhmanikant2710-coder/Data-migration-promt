Hi Geoff,

Thanks for the note on #247, and apologies – I should have checked with you before building it. I had already built Special Assets Managed as described in the log. It is kept on a separate branch and has not been deployed to QA, so nothing is final and I can adjust it based on your input.

What is built today:
- A "Special Assets Managed" Yes/No drop-down in Relationship Overview, showing CORE_Reviews.Special_assets.
- A change at the customer level updates all accounts of the review; a change on any account (Transactions > Account Details) updates the customer level and all other accounts.
- New accounts default to the customer's value.
- Existing data is never changed automatically – values only sync when a reviewer actually changes Special Assets.
- Three reports (CRO Production Summary, CRM Findings & Observations, Covenants Summary) now filter Special Assets at the customer level, like the other reports.

The sync logic is generic, so a CCL field can reuse it with little extra work. Before I build it, could you confirm:
1. Data: should it show CORE_Reviews.CCL (Yes/No) and stay in sync with the CCL checkbox on each account (CORE_Accounts.CCL), exactly like Special Assets?
2. Placement and label: where in the Customer Profile block should it sit, and should the label be "CCL"?
3. New accounts: should they default to the customer's CCL value?
4. Reports: is there a CCL filter or column in any report that should use the customer-level value?
5. Existing data: if accounts already have different CCL values, should they stay as they are until a reviewer changes CCL?

Please also let me know if you would like anything changed in how Special Assets works. Once you confirm, I will build CCL and release both together.

Thanks,
Manikant
