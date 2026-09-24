Start development on this item — investigate first, then propose a fix.
Show diff only, do not apply.

Item: YTD overwritten by TTM (Part 4-I, lines ~2130-2144)

Investigate:
1. Quote verbatim the exact code where curProfitBeforeTaxesYTD is checked
   for missing/zero and replaced with the TTM value, including the four
   alias keys it mirrors into.
2. Is this substitution intentional (any comment/rationale in the code),
   or does it look like an unintended bug?
3. Check legacy MS Access — does any VBA/query there ever substitute a
   zero/missing YTD with a TTM value? Quote what you find, or confirm you
   found nothing.

Propose a fix:
- Stop treating "YTD is exactly zero" as equivalent to "YTD is missing."
  A legitimately zero YTD (e.g. a break-even month) should display as
  zero, not get silently replaced with a twelve-month figure.
- Do NOT assume the NULL/missing-YTD fallback (if one exists) should also
  change — investigate what that does first, and preserve it if it's
  correct behavior.
- Show the diff only. Do not apply.

Before proposing the diff, confirm explicitly: does this fix have ANY
dependency on the TTM redesign (P1) — i.e. does it read or write anything
that P1 will also change? I believe it doesn't (this is a "never conflate
YTD with TTM" bug, independent of whether TTM's own numbers are later
fixed) — but confirm or refute that with evidence rather than assuming.
