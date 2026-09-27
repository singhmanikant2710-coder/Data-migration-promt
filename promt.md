GENERIC FORMATTING FIX — must work for ALL customers and ALL industries.
No customer-, label- or industry-specific patches.

SAFETY RULES (strict):
- Change ONLY how covenant and custom-field values are formatted/sourced.
- Do NOT change any calculation, stored value, fixed column, column
  order, API, or backend persist logic.
- If format metadata is missing for a column, keep today's behaviour.
- Fixed (non-covenant, non-custom) columns must render exactly as today.
- STOP and report if any change would affect a fixed column.

RULE 1 — Covenant values, all surfaces (Summary Top Strip, Monthly
Summary, view page, DetailGrid, PDF, CSV), whatever path built the column:
- Value = actual of the covenant with the SAME NAME from tblMainCovenants
  (never a registry alias of a fixed metric; never another covenant's value).
- Only covenants with intCovenantOrder 1..4 are shown.
- Grid / PDF / CSV format (legacy funSave:494), from strCovenantFormat:
    "$" -> "$" + #,##0
    else -> FormatNumber(x,2) + format appended verbatim
            ("%" -> "0.03%" NO x100, "x" -> "3.44x", blank -> "3.44")
- Summary Top Strip covenant tiles: plain number, no $/x/% —
  0 decimals if format is "$", else 2 decimals.
- NULL -> "—".
- Remove hardcoded covenant kinds (currency/percent/ratio) wherever a
  covenant value is rendered (incl. registry 1574/2006 and ratio kinds
  for covenant labels).

RULE 2 — Custom fields (slots 1..4, header = strCustomFieldDescription{i}):
- Numeric value + label contains "%" -> FormatNumber(x,2) + "%" (NO x100)
- Numeric value, any other label -> "$" + #,##0
- Non-numeric text -> as stored. NULL -> "—".
- Same on all surfaces incl. Top Strip. Replace the current
  "$ in label" / "suppressed availability" special-cases with this rule.

RULE 3 — Cash & Charge-offs panel in EVERY industry mapping file:
Interest Expense TTM = curInterestExpenseTTM, Fixed Charges TTM =
curFixedChargesTTM, FCC = dblFixedChargeCoverageTTM (stored only; no
monthly field, recompute, or Interest Coverage fallback). NULL -> "—".

Expected ECLIPSE (ABL), latest month:
- Grid: Min TNW $401,175 | Max Senior Debt/TNW 3.44x | Min Interest
  Coverage 1.84x | Min Excess Availability ($) $22,477 | Other 1 (%) and
  Other 1 ($) not shown (order 5,6)
- Top Strip: 401,175 | 3.44 | 1.84 | 22,477
- Custom: Max c/o (%) -> 0.03% | Min Liquidity -> $ value
- Cash & Charge-offs: Int Exp TTM $88,212 | Fixed Charges TTM $88,212 |
  FCC 5.43x
ATHENS must stay as currently verified.

Build, tests, do not commit.

REPORT FORMAT (mandatory):
- ADDED: new logic/lines (file:line)
- REMOVED: logic/lines removed (file:line)
- BEHAVIOUR CHANGE: what the user sees on screen, incl. NULL value case
- NOT TOUCHED: related code deliberately left as-is
- List every industry mapping file touched.

- WITH cust AS (
  SELECT m.strIndustry, m.strCustomerName, MAX(m.strMonthKey) AS lastMk
  FROM tblMain m
  WHERE m.strMonthKey >= '202501'
  GROUP BY m.strIndustry, m.strCustomerName
),
scored AS (
  SELECT c.*,
    (SELECT COUNT(DISTINCT v.strCovenantName) FROM tblMainCovenants v
      WHERE v.strCustomerName = c.strCustomerName
        AND v.intCovenantOrder BETWEEN 1 AND 4) AS covenants,
    (SELECT CASE WHEN t.strCustomFieldDescription1 IS NOT NULL THEN 1 ELSE 0 END
          + CASE WHEN t.strCustomFieldDescription2 IS NOT NULL THEN 1 ELSE 0 END
          + CASE WHEN t.strCustomFieldDescription3 IS NOT NULL THEN 1 ELSE 0 END
          + CASE WHEN t.strCustomFieldDescription4 IS NOT NULL THEN 1 ELSE 0 END
       FROM tblCustomer t WHERE t.strCustomerName = c.strCustomerName) AS customFields
  FROM cust c
)
SELECT strIndustry, strCustomerName, lastMk, covenants, customFields
FROM (
  SELECT *, ROW_NUMBER() OVER (PARTITION BY strIndustry
           ORDER BY covenants DESC, customFields DESC, lastMk DESC) AS rn
  FROM scored
) x
WHERE rn = 1
ORDER BY strIndustry;
