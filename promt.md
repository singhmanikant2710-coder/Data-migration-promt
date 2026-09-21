READ-ONLY — do not edit any files.

Confirmed: GET /api/v1/covenants (SqlCovenantRepository.GetAsync) reads from
tblMainCovenants, matched by (strCustomerName, strMonthKey) — NOT
strCustomerNumber (that column is NULL in this table). Data DOES exist there
for Athens Paper across many months — the earlier "table is empty" finding
was based on a wrong filter, not an actual empty table. Retract that claim.

Now trace: does ANY write path (UpsertRowWithConnectionAsync, UpsertBatchAsync,
or anything else in the save flow triggered by the Black Book Edit "Refresh/Save"
or the batch covenant save) ever write to tblMainCovenants.strCovenantActual?
Or does every covenant-labeled key from the UI end up only in
tblMain.dblCovenantActual{N} via the IsLikelyCovenantKey/ApplyIncomingCovenantsToSlotsAsync
path previously traced?

Also check: is there a SqlCovenantRepository write/update method at all (not
just GetAsync)? If yes, is it ever called from the save flow, or does it exist
unused?

Report findings only. Do not propose or write a fix.
