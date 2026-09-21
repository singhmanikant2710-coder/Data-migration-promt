Final check before I approve. For Athens Paper Company Inc, month 202510:

Show me, side by side:
- What curProfitBeforeTaxesYTD (or whichever YTD/TTM field the recompute
  functions touch) currently returns TODAY (with the old calendar-year fy).
- What it will return AFTER this fix (with the corrected fy=2026).
- Which rows get summed/aggregated in each case — i.e., which fiscal-year
  bucket the recompute currently pulls from vs. which bucket it will pull
  from after the fix.

I need to know exactly what visibly changes for this one customer/month
so I can eyeball it against the legacy Access screen before I sign off.
Don't apply anything — just report the before/after values and which rows
feed them.
