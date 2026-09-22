Critical, time-sensitive: find the function that computes/recomputes
intElapsedFiscalDays (likely UpdateElapsedFiscalDaysForRowAsync, ~line 4094).
Show me its exact current logic.

The original BCAT spec formula is: intElapsedFiscalDays := intFiscalMonth * 30
— a direct multiplication, not a DateDiff against datFiscalYearStart. Confirm
whether the current code deviates from this spec, and if so, propose a fix
that replaces the DateDiff logic with intFiscalMonth * 30 (using the now-
validated, correct intFiscalMonth from today's fix).

Verify against Athens 202510: intFiscalMonth=1, expected
intElapsedFiscalDays=30 — which matches the pre-corruption stored value
I'm restoring right now. Also check dblAccountsReceivableTurnDays and
curInventoryTurn, since both multiply by intElapsedFiscalDays and will
self-correct once it's fixed.

Show diff only, do not apply.
