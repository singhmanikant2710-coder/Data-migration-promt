Display fix (ATHENS, WholesaleTrade): Equity / Liabilities / Availability
section, "D/TNW (B/A)" shows 0.98x; legacy shows 0.98 (no "x").
Remove the "x" suffix for this tile only, using the same
formatRatioNoSuffix approach as the FCC TTM Top Strip fix (banker's
rounding via formatRatio, strip only the x). Check if the same tile
appears on other screens (view page, PDF) and apply the same there.
Do not change values or calculations.
Build, tests, do not commit.

REPORT FORMAT (mandatory):
- ADDED: new logic/lines (file:line)
- REMOVED: logic/lines removed (file:line)
- BEHAVIOUR CHANGE: what the user sees on screen, incl. NULL value case
- NOT TOUCHED: related code deliberately left as-is
