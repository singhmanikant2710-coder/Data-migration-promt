FCC display bug (ATHENS 202603). DB and Access are identical:
dblFixedChargeCoverage = 3.93, dblFixedChargeCoverageTTM = 4.975.

1. Cash & Charge-offs section shows FCC = 3.19x; legacy shows 4.98x.
   3.19 matches no stored column. READ-ONLY first: quote the file:line
   that produces the FCC value in that section and what it computes.
   FIX: display the stored dblFixedChargeCoverageTTM (no frontend
   recomputation), formatted 2 decimals + "x".
2. Summary Top Strip "FCC TTM" shows 4.98x; legacy shows 4.98 (no x).
   Remove the "x" suffix for that Top Strip tile only.
Do not change calculations or the persist/SQL layer.
Build, tests, do not commit.

REPORT FORMAT (mandatory):
- ADDED: new logic/lines (file:line)
- REMOVED: logic/lines removed (file:line)
- BEHAVIOUR CHANGE: what the user sees on screen, incl. NULL value case
- NOT TOUCHED: related code deliberately left as-is
