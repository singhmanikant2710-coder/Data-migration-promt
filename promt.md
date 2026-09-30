Hi team,

While comparing BCAT with legacy we found several customers whose dev data does not match legacy Access. These are data issues, not application defects, and John's team will correct them in the data before cutover. Please do not test these customers or raise defects on them until the data is fixed.

1. Fiscal year labelled differently from legacy (fix in Access before cutover)
- BANKERS HEALTHCARE GROUP LLC
- KEYSTONE PRIVATE INCOME FUND
- NATIONWIDE SPECIALTY FINANCE INC
- VERMEER MOUNTAIN WEST INC

2. Dev data differs from Access (edited or older copy)
- MARINER FINANCE LLC (PBT, YTD PBT, Interest Coverage, Net C/O basis, covenants)
- SHABANA MOTORS LLC (custom field values, Avg Principal N/R TTM)

3. Months missing in dev or leftover covenant rows
- ALAN WIRE COMPANY (202508-202512 missing)
- ADIR INTERNATIONAL LLC (202401-202404)
- WORLD ACCEPTANCE CORPORATION (202508)
- ALLSTATES WORLDCARGO INC (202609)
- MDR CONSTRUCTION INC (202601)
- THUNDER CARRIER SERVICES LLC (row with blank month)

4. Recent months entered in Access after the dev load (values show $0 or blank in dev)
- FIRST TOWER LOAN LLC, SAC FINANCE INC, GATEWAY COMMERCIAL FINANCE LLC, SUNSET MANAGEMENT INC, FIRST FINANCIAL CREDIT INC, BLOOMFIELD CAPITAL INCOME FUND V, LIBERTY BANKER LIFE INSURANCE, STANDARD PREMIUM FINANCE (202604-202606)

5. Customers with no industry set
About 44 customers have no industry in the customer record, so the screens show a generic template (e.g. ALLSTATES WORLDCARGO INC, TEMPO GLOBAL RESOURCES). Please skip these until the industry is filled in.

Applies to all customers: where legacy shows a blank but BCAT shows $0 (for example Cash Collections, Net C/O, Reserve Coverage on some months), the dev copy stored the blank as 0. This is part of the same data fix, so please don't log it as a defect.

If you see a mismatch on any other customer, please share the customer, month, screen, legacy value and BCAT value so we can check whether it is code or data.

Thanks.
