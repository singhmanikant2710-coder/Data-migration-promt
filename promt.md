Stop — hunk (f) is wrong, confirmed against live legacy Access data.

Evidence: Imperial Trading Co LLC (start=6) and Athens Paper (start=10)
both show datFiscalYearStart following a "-1" convention (year =
intFiscalYear - 1) through roughly 2020, then switching MID-FISCAL-YEAR
to a "0" convention (year = intFiscalYear exactly), and staying on "0"
consistently ever since, through the most recent rows in both customers'
history. This is a genuine historical convention change baked into the
legacy Access application itself (confirmed happening across multiple
customers with different fiscal start months around the same real-world
time) — not corruption, and not something to "preserve" going forward.

Confirmed current-correct value directly from legacy Access: Athens
202510 → intFiscalYear=2026, datFiscalYearStart=10/1/2026 (delta=0).

Your hunk (f) computes datFiscalYearStart from the CALENDAR year in the
month key, deliberately reverting to the deprecated "-1" convention to
avoid what you thought was a regression. That's backwards — it would
produce 10/1/2025 for Athens 202510, contradicting the confirmed current
legacy value of 10/1/2026.

Fix: revert hunk (f) to the ORIGINAL simple line, unchanged, just fed the
corrected `fy` from the new formula:

DateTime? datFiscalYearStart = (fy > 0 && fiscalStartMonth.HasValue)
    ? new DateTime(fy, fiscalStartMonth.Value, 1)
    : (DateTime?)null;

Drop the __calYear extraction entirely — it's not needed and was
producing the wrong value. fy here is already the corrected fiscal year
from the validated Variant B formula, and multiplying it straight into
this line reproduces the confirmed current legacy behavior exactly.

Re-verify against Athens 202510 → 10/1/2026 and Imperial Trading's most
recent rows before resubmitting this hunk.
