READ-ONLY — do not edit any files.

Confirmed context (already verified, don't re-derive):
- Editing "Min Tangible Net Worth" for Athens Paper Company Inc, month 202510,
  via TWO different UI paths (Black Book Edit "Refresh/Save" button, and a
  separate "batch" save triggered from covenant-related network calls) both
  fail with the identical error:
  "Violation of PRIMARY KEY constraint 'tblMain$PrimaryKey'... duplicate key
  value is (202510, ATHENS PAPER COMPANY INC)."
- The "batch" path hits PUT /api/v1/main/batch → MainController.UpsertBatch
  → SqlMainRepository.UpsertBatchAsync, which (per earlier trace) converges
  on the same SqlMainRepository.UpsertRowWithConnectionAsync as the
  single-row save path.
- The batch request payload sends a generic label/value shape:
  { customer, updates: [{ monthKey, values: { "Min Tangible Net Worth": <number> } }] }
- The GET response for this same row separately returns a field literally
  named "MinTangibleNetWorth" (no space) with the same value — this is NOT
  dblCovenantActual1 or dblCovenantActual2 (both are 0 for this row/customer).
  It is also not in tblMainCovenants, tblMainDisplayCovenants, or
  v_ifu_covenants (confirmed empty for this customer).

What to find:
1. Where does the label "Min Tangible Net Worth" (or the key "MinTangibleNetWorth")
   get resolved to an actual tblMain column name? Look for a label→column
   mapping/dictionary in the application or infrastructure layer — likely
   near UpsertBatchAsync or wherever the GET endpoint assembles its response.
2. What is the actual tblMain column this resolves to?
3. Confirm UpsertBatchAsync for this payload shape also funnels into
   UpsertRowWithConnectionAsync's existence-probe/UPDATE logic (the same
   calendar-derived fy/fm matching already identified as the Bug 1 root
   cause), or whether the batch path has its own separate (possibly
   differently-broken) upsert logic.

Report the mapping mechanism, the resolved column name, and whether the
batch path shares the exact same defect already diagnosed, or a related
but distinct one. Do not propose or write a fix yet.
