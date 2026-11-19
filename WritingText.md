#247 – Implemented (Special Assets Managed and CCL Managed). Tested in Dev and pushed to the develop branch. Not yet deployed to QA.

Special Assets Managed (approved): as described previously.

CCL Managed (per your answers):
- New "CCL Managed" Yes/No drop-down in Customer Profile, showing CORE_Reviews.CCL. Layout as you preferred: CCL Managed below Customer Size, FHN Portfolio Segment moved below FHN Portfolio, FHN NAICS Industry below that.
- It stays in sync with the account-level CCL fields exactly like Special Assets: a change at the customer level updates all accounts, and a change on any account updates the customer and all other accounts. Setting both Yes and No in one save is blocked with a message. Existing data is only changed when a reviewer changes CCL.
- New accounts default to the customer's CCL value.
- Special Assets and CCL can be changed together in one save; each is applied independently.

Reports: all reports now filter CCL on the customer-level value. Three reports (CRO Production Summary, CRM Findings & Observations, Covenants Summary) previously used the account level and were switched. In Dev this changes the result for 7 reviews – 5 from No to Yes (19235, 19238, 19244, 19246, 19974) and 2 from Yes to No (21178, 21541). Only 21541 is from 2026; the others are older reviews where the customer and account CCL values differ.

Tested in Dev: new layout, customer-level and account-level CCL changes, changing CCL and Special Assets in the same save.

I will confirm here once deployed to QA.
