FIX the Blackbook PDF column set to match legacy (generic, all
industries). Scope: PDF / report page only — do not change the edit page.

Legacy rule (frm003...BlackBookView + rpt...BlackBook):
- Covenant columns = the customer's covenants with intCovenantOrder
  1..4, ordered by intCovenantOrder, header = covenant name. Order 0 or
  >4 is not shown (e.g. ATHENS "Other 1 (%)", order 0).
  Values: same rules as the edit page — tblMainCovenants actual only,
  NULL -> "—", format from strCovenantFormat.
- Custom field columns = slots 1..4, header = tblCustomer
  strCustomFieldDescription{i}. Hide slots whose label is blank or the
  default "Custom Field {i}". Format rule same as the UI ($ label ->
  currency).

1. Feed the report page the same customer-driven covenant/custom
   metadata the edit page uses (SummaryPayload / runtimeFields /
   tableConfig) into buildMonthSummaryColumns.
2. For the PDF, drop template covenant/custom columns that are not in
   the customer's list: Min Excess Availability ($), Min Interest
   Coverage, Max Distribution (%), Min Cash Collection (%), Other 1 (%),
   Max C/O (%), Min Liquidity (profile include, hardcoded CustomField1/2
   labels, MCA base-list leakage).
3. Fixed columns unchanged.

Expected ATHENS: covenants = Min Tangible Net Worth, Min Net Income;
custom = AMZN %, Suppressed Availability, AMZN $ ineligible.
Build, tests, do not commit.

REPORT FORMAT (mandatory):
- ADDED: new logic/lines (file:line)
- REMOVED: logic/lines removed (file:line)
- BEHAVIOUR CHANGE: what the user sees on screen, incl. NULL value case
- NOT TOUCHED: related code deliberately left as-is
