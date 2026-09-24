Hi both — looping Jacob in since a few of these need business-side input that only he can give, and John for the data-side items. Quick context for Jacob: we've been deep in a root-cause investigation on BCAT's fiscal-year and TTM calculations over the past few days — found several real bugs, fixed and verified most, and a few open items need your input to close out properly. Trying to be precise rather than vague so you can answer without needing a call, but happy to jump on one if that's faster.

Already resolved, no action needed from either of you:
• The datFiscalYearStart column formula — verified directly against both SQL Server and MS Access on 36 untouched historical rows (Athens Paper, FY2022–FY2024), confirmed our code already matches legacy's current behavior. No fix needed.
• 5 core save-path bugs (PK collisions, fiscal month/year drift, a covenant write going to the wrong table, an elapsed-days formula error) — fixed, verified against live data and legacy Access, and committed.

FOR JACOB — formula provenance (this is the big one):
Legacy MS Access has NO query, VBA, or calculated-field definition anywhere for these fields: curFixedCharges, perInterestCoverage, perInterestCoverageTTM, curEBIT, curEBITTTM. They just have stored values — meaning someone (an analyst?) typed them in, or they came from an external feed. Our new app currently COMPUTES these from formulas in an internal reference doc that itself is labeled "Confidence: Inferred" — meaning that validation against you never happened before it shipped.

1. Where did these values historically come from — analyst-entered, or an external/upstream source? If analysts typed them, the app silently overwriting them on every save is a real problem, not a formula question.
2. For interest coverage — EBIT or EBITDA?
3. Are distributions included in or excluded from fixed charges?
We're separately running a data comparison against pre-app legacy months to see whether our inferred formulas match reality — will share what we find, which may answer some of this without needing your time.

FOR JOHN:
4. Keystone Private Income Fund — before you normalize the 4 exception customers, wanted to flag: the other 3 (Bankers Healthcare, Nationwide, Vermeer) consistently show the starting-year fiscal-year convention across their history. Keystone doesn't match that pattern on a single row (0 of 62). Worth confirming it actually belongs in that group before its data gets changed.
5. Does anything copy data from the new .NET application back into the legacy Access database? If nothing does, we can close out a related data-flow question completely.
6. Lower priority, no rush: there are ~1,027 older (pre-2020) rows where datFiscalYearStart follows an earlier legacy convention. No code change needed on our end either way — purely your call whenever convenient whether to backfill them.

FYI, not needed from either of you: we found 4 API endpoints serving customer financial data with no authentication check at the code level — already routed to infra/security to confirm whether it's covered at the gateway.

We'll keep making progress on items that don't depend on these answers in the meantime. Let us know if a call is easier than typing all this out.
