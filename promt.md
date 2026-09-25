SELECT strMonthKey, strCovenantThreshold
FROM tblMainCovenants
WHERE strCustomerName LIKE 'ATHENS PAPER%'
  AND strCovenantName = 'Min Tangible Net Worth'
  AND strMonthKey IN ('202604','202605');


  Min TNW shows 43,469,196 for ATHENS 202604/202605. That is the
covenant THRESHOLD (confirmed in /api/v1/covenants response). The
covenant actual is NULL, and the frontend falls back to threshold.

RULE (legacy): covenant VALUE displays show ACTUAL only. Threshold is
used only for the explicit threshold column/tile and the violation
highlight.

1. covNumeric in view/page.tsx, edit/page.tsx, report/page.tsx: return
   the actual only — no threshold fallback.
2. Search the frontend for any other place where a covenant value
   falls back to threshold (thresholdNumeric, strCovenantThreshold,
   dblThreshold, "threshold ??") in a VALUE display. List file:line
   and remove the fallback. Do not touch the threshold column/tile or
   the violation highlight.
3. A NULL actual must render "—".
Build, tests, do not commit. Report files + lines.
