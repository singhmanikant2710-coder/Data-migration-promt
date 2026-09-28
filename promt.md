READ-ONLY analysis. No code changes.

Attached: SSMS outputs of 6 queries (covenant order 0/>4, covenant name
not matching tblMain slot captions, custom field labels, non-zero
CPLTD TTM, industry mismatch list, 12-customer industry test set).

Build a regression TEST PLAN for the changes made in this session
(covenant slot/order fix, covenant + custom-field formatting rules,
Cash & Charge-offs TTM trio + CPLTD <- curCPLTDTTM, industry template
rewrites for DirectAuto/IndirectAuto/Factoring/MCA/ConsumerFinance/
Trucking/Other/ABL/EnergyRelated, selected-month clamp, PDF column set).

For each risk group pick at most 3 representative customers (dedupe
across groups; prefer customers that hit several groups). For each
customer give ONE row:
- Customer, industry (tblCustomer), month to open (latest month with data)
- Risk group(s) and why this customer is risky
- Exact screen + section + column/tile to check (Top Strip, Monthly
  Summary, Month/TTM, Cash & Charge-offs, right rail, PDF Fiscal YTD /
  Rolling 24)
- Expected display value per our rules, computed from the data
  (covenant: "$" -> $#,##0 in grid, plain in Top Strip; else
  FormatNumber(x,2)+format; custom: "%" label -> x.xx%, else $#,##0,
  text as-is; NULL -> "—")
- A flag where the rule itself may be wrong for that customer (e.g. a
  custom field label that looks like a count/days/FICO but would get $)
- The SQL to re-check that value for that customer/month

Output as a table sorted by priority (highest risk first), and save it
as test-plan.md in the repo root. Report only.
