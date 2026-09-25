Display bug: A/R Turn Days and Inventory Turn. Stored values are
correct and identical to Access (ATHENS 202603 dblAccountsReceivableTurnDays
= 60.8277). Legacy shows 61, our Summary Top Strip and Monthly Summary
show 60 — we truncate/floor on display, legacy rounds.

Fix: wherever A/R Turn Days and Inventory Turn are FORMATTED for display
(Top Strip, Monthly Summary, and any other screen), round to 0 decimals
half away from zero (Access Format behaviour) instead of floor/trunc.
Do NOT change the stored calculation or the Int() logic in
TblMainCalcs / SQL persist.

Build, tests, do not commit.

REPORT FORMAT (mandatory):
- ADDED: new logic/lines (file:line)
- REMOVED: logic/lines removed (file:line)
- BEHAVIOUR CHANGE: what the user sees on screen, incl. NULL value case
- NOT TOUCHED: related code deliberately left as-is
