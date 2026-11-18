Answers: 1A 2A 3A 4 switch 5A 6B
4: switch only CRO Production Summary, CRM Findings & Observations and Covenants Summary to the review-level, NULL-as-No form. Dev impact checked: 1 review moves Yes→No (Review_id 21541, review = No, accounts 3 Yes / 1 No – a data-integrity case). Do not touch the other 11 reports.
6B: on Add Account, default the new account's Special Assets to the review's current Special_assets value (NULL → No, as today). This is only a default; the user can still change it, and that change follows the normal rules (sync + 1A conflict check).
Proceed with Phase 3 and 4.
