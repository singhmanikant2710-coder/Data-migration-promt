SELECT * FROM tblMainCovenants
WHERE strCustomerName = 'ATHENS PAPER COMPANY INC'
  AND strCovenantName LIKE '%Adjusted Debt%';

  READ-ONLY — do not edit any files.

Confirmed root causes (from prior investigation, do not re-derive):
1. UpsertRowWithConnectionAsync's existence probe and UPDATE WHERE clause
   match on (intFiscalYear=@fy AND intFiscalMonth=@fm) OR strMonthKey=@mk,
   where (@fy,@fm) come from DeriveFiscalFromMonthKey — a pure calendar
   split of the month key string, ignoring the customer's actual fiscal
   year start. For non-calendar-FY customers this can match a DIFFERENT
   real row than the one being edited, and the UPDATE also writes
   strMonthKey/strCustomerName (the primary key), causing either a PK
   violation or a silent cross-row overwrite.
2. New-row intFiscalMonth is derived as prevRow.intFiscalMonth + 1 (reading
   the immediately preceding row by strMonthKey), not from the customer's
   true fiscal year start — so any gap or any prior corruption from (1)
   propagates wrong fiscal values into every subsequent month.
3. Covenant-labeled edits from Black Book Edit (e.g. "Min Tangible Net
   Worth") are resolved via IsLikelyCovenantKey/ApplyIncomingCovenantsToSlotsAsync
   and written to tblMain.dblCovenantActual{N} — but the corresponding read
   path (TryMergeCovenantsIntoSeries, GET /api/v1/covenants) reads from
   tblMainCovenants.strCovenantActual, a different table entirely. The
   write is structurally unreachable from tblMainCovenants: MainController's
   PropagateCovenantSlotUpdatesAsync only fires on a slot-numbered key
   (dblCovenantActual{i}), and the Black Book Edit payload sends the label
   ("Min Tangible Net Worth"), never the slot key — so propagation never
   triggers for this path.

Fix all three, scoped narrowly:

1. In UpsertRowWithConnectionAsync: change the existence probe and UPDATE
   WHERE clause to match ONLY on (strCustomerName, strMonthKey) — remove
   the calendar-derived (intFiscalYear, intFiscalMonth) OR-branch entirely.
   The UPDATE must never write strMonthKey or strCustomerName (the primary
   key is immutable once a row exists).

2. Replace the prevRow.intFiscalMonth + 1 derivation for new rows with a
   calculation from the customer's actual fiscal-year-start month
   (tblCustomer) and the calendar month embedded in the new strMonthKey —
   must produce the correct value regardless of gaps or whether the
   preceding row's fiscal fields are already correct.

3. For any Black Book Edit payload key that resolves via
   IsLikelyCovenantKey to a covenant slot, write to
   tblMainCovenants.strCovenantActual (matched by strCustomerName +
   strMonthKey + strCovenantName) INSTEAD OF tblMain.dblCovenantActual{N}
   — or, if writing to tblMain must be kept for other reasons, ensure
   PropagateCovenantSlotUpdatesAsync is invoked with the slot number that
   ApplyIncomingCovenantsToSlotsAsync resolved (currently that number is
   never returned to the caller — surface it so propagation can fire).

Constraints:
- Generic fix only — must work correctly for all nine fiscal year start
  months, not just Athens Paper's October start.
- Do not touch MapMetricPoint, TryMergeCovenantsIntoSeries, covNumeric,
  computeMinTnwForRowLocal, or any threshold-vs-actual display logic —
  that is explicitly out of scope for this fix and is being decided
  separately.
- Do not change AccessMainRepository.cs unless it shares the exact same
  three defects and is confirmed still in active use — flag it instead of
  editing it if uncertain.
- Cross-check against legacy Access output as ground truth after the fix.
- Show me the diff only, don't apply. I'll review and approve before you
  write.
- Flag any other callers of UpsertRowWithConnectionAsync or
  ApplyIncomingCovenantsToSlotsAsync so I can scope regression testing
  beyond Athens Paper.
