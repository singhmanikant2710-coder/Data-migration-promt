SELECT strCustomerName, intFiscalYearMonthStart
FROM tblCustomer
WHERE strCustomerName LIKE 'FREZ%';

REGRESSION (critical): FREZ-N-STOR INC (tblCustomer.strIndustry = Other)
renders the GENERIC template instead of the Other (frm009) template.
New UI shows Month/TTM = PBT, Interest Expense, EBIT, Interest Coverage +
Cash & Charge-offs (Net C/O, 60+ DPD, Reserve Coverage). Legacy frm009:
- Month/TTM: PBT, Interest Expense, Depreciation, Amortization, EBITDA,
  Distributions, Cash Avail for FC; TTM: same rows TTM
- Box 2: CPLTD (prior period), Interest Expense TTM, Fixed Charges TTM,
  FCC (x)
- Box 3: Equity / Liabilities / Availability (already correct)

1. READ-ONLY: trace industry "Other" through normalizeIndustryKey,
   resolveIndustryMapping (edit + view), backend NormalizeIndustry and
   profiles. Quote the line where it falls to generic.
2. FIX: "Other" -> the Other mapping (frm009) exactly as before the
   resolver refactor. Then verify ALL 13 industry keys resolve to their
   own mapping (not generic) on edit AND view pages: list key -> mapping.
3. Do not change any mapping's content.

GOLDEN: FREZ-N-STOR matches legacy frm009 layout above; one customer per
industry (ATHENS, ECLIPSE, WESTLAKE, LEON'S AUTO, MIDDLE GEORGIA, TBS
FACTORING, CSC LEASING, GREEN PLAINS, TIMEPAYMENT, CREST, UNIVERSAL MGMT,
CIT NORTHBRIDGE) still renders its own template.
Build, tests, do not commit. Report root cause (file:line), ADDED/
REMOVED, key->mapping table, NOT TOUCHED.
