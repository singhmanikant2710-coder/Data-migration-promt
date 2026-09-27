SELECT strCovenantName, intCovenantOrder, strCovenantFormat, strCovenantActual
FROM tblMainCovenants
WHERE strCustomerName = 'ECLIPSE BUSINESS CAPITAL SPV LLC'
  AND strMonthKey = (SELECT MAX(strMonthKey) FROM tblMainCovenants
                     WHERE strCustomerName = 'ECLIPSE BUSINESS CAPITAL SPV LLC')
ORDER BY intCovenantOrder;

SELECT TOP 1 strMonthKey, curInterestExpense, curInterestExpenseTTM,
  curFixedChargesTTM, dblFixedChargeCoverage, dblFixedChargeCoverageTTM
FROM tblMain
WHERE strCustomerName = 'ECLIPSE BUSINESS CAPITAL SPV LLC'
ORDER BY strMonthKey DESC;

GENERIC fix — must apply to ALL customers and ALL industries. No label-
or industry-specific patches.

1. ONE covenant formatter, used by every surface (Summary Top Strip,
   Monthly Summary, view page, DetailGrid, PDF, CSV) for every covenant
   value, whatever path built the column (payload, registry, fallback):
     fmt == "$"  -> "$" + #,##0
     otherwise   -> FormatNumber(x, 2) + fmt appended verbatim
                    ("%" -> "0.03%" with NO x100; "x" -> "1.84x";
                     blank -> "1.84")
     NULL -> "—"
   Format comes from that covenant's strCovenantFormat (tblMainCovenants /
   SummaryPayload / /api/v1/covenants). Remove hardcoded covenant kinds
   (currency/percent/ratio) wherever a covenant value is rendered,
   including registry 1574/2006 and the "x" ratio kinds for covenant
   labels (Max Senior Debt/TNW, Min Interest Coverage, Max CAR Ratio...).

2. Covenant VALUE source: if the customer has a covenant with a given
   name, its value is the tblMainCovenants actual — never a registry
   alias for a fixed metric with the same/similar label (e.g. "Min
   Excess Availability ($)" must not read curExcessCollateralAvailability).

3. Cash & Charge-offs panel in ALL industry mappings (abl.ts,
   wholesaleTrade.ts, manufacturing.ts, equipment.ts, ... every file):
   Interest Expense TTM = curInterestExpenseTTM, Fixed Charges TTM =
   curFixedChargesTTM, FCC = dblFixedChargeCoverageTTM — stored values
   only, no monthly fields, no recompute, no Interest Coverage fallback.
   NULL -> "—". List every mapping file you change.

Build, tests, do not commit.

REPORT FORMAT (mandatory):
- ADDED: new logic/lines (file:line)
- REMOVED: logic/lines removed (file:line)
- BEHAVIOUR CHANGE: what the user sees on screen, incl. NULL value case
- NOT TOUCHED: related code deliberately left as-is
