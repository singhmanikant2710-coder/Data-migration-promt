READ-ONLY — do not edit any files.

CONFIRMED (from database + legacy screenshots, not to be re-derived):
- tblMainCovenants holds per-month covenant rows: strCovenantActual (varies
  monthly, NULL on non-reporting months), strCovenantThreshold (constant,
  copied from tblCustomer, never NULL), strCovenantReported (e.g. "Quarterly").
- Legacy Access ("Min Tangible Net Worth" field on the Black Book summary
  screen) displays strCovenantActual for the selected month — NOT
  strCovenantThreshold. Verified: month 202509, legacy shows 59652, which
  equals that month's strCovenantActual; strCovenantThreshold for the same
  row is 43469196 (unrelated, never displayed there).
- The new app's GET /api/v1/covenants response for "MinTangibleNetWorth"
  currently returns 43469196 for Athens Paper regardless of month — this is
  the threshold, not the actual — because MapMetricPoint's logic
  (`threshold ?? actual`) prefers Threshold for thresholdKeys, and Threshold
  is never null. This does NOT match legacy behavior.
- Editing this field via Black Book Edit writes to tblMain.dblCovenantActual{N}
  (via IsLikelyCovenantKey/ApplyIncomingCovenantsToSlotsAsync), NOT to
  tblMainCovenants.strCovenantActual, which is what GET/legacy-equivalent
  logic should read from.

What to find:
1. Quote verbatim the `covenantTargets` and `thresholdKeys` dictionary/set
   definitions (referenced earlier around line 1518) — specifically, does
   "MinTangibleNetWorth" appear in thresholdKeys? What other covenant keys
   are in that set, and might they have the same legacy-parity mismatch?
2. Is there ANY write path anywhere in the codebase (backend or frontend)
   that writes to tblMainCovenants.strCovenantActual? If yes, where, and
   when is it invoked vs. the tblMain slot-write path?
3. Confirm: does the frontend's extractMinTnw (page.tsx ~992-1023) also
   independently prefer a threshold-shaped value, or does it just take
   whatever the backend sends?

Report findings only, verbatim where possible. Do not propose or write a
fix yet — a consolidated fix covering all four now-confirmed issues (PK
collision, fiscal-month derivation, wrong write target, wrong read
preference) will be written after this.
