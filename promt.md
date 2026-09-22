READ-ONLY investigation first, then propose a fix — do not apply anything yet.

Confirmed regression: SqlCovenantRepository.SeedFromLatestAsync's minimal
tblMain insert (lines ~396-421) now runs before UpsertRowWithConnectionAsync
for every new month, because of my MainController reorder. This insert:
- Uses the old calendar-only fy/fm convention (mkInt/100, mkInt%100)
- Never sets datFiscalYearStart
- Causes UpsertRowWithConnectionAsync to see exists=true and take the
  UPDATE branch, which corrects fy/fm but never touches datFiscalYearStart
  (that column is only ever set in the INSERT branch, which no longer runs
  for new months)

Investigate:
1. Why does SeedFromLatestAsync insert into tblMain at all? Check git
   blame/history and any FK constraint from tblMainCovenants to tblMain
   requiring the row to pre-exist. Is this insert load-bearing for
   anything, or just defensive/idempotent scaffolding?
2. If tblMainCovenants has no real dependency on tblMain already
   existing, propose removing the tblMain insert from SeedFromLatestAsync
   entirely — let UpsertRowWithConnectionAsync remain the sole writer/
   inserter of tblMain rows, with correct fy/fm/datFiscalYearStart, for
   both the pre-write hook and the standalone POST /api/v1/covenants/seed
   endpoint.
3. If it IS load-bearing (something breaks without it), instead propose:
   have SeedFromLatestAsync call the SAME fiscal-derivation logic
   (GetFiscalYearStartMonthAsync + DeriveFiscalFromMonthKey) and set
   datFiscalYearStart correctly on its own insert, so there is only one
   correct implementation instead of two that can drift.

Report your finding and proposed fix as a diff. I will review before
applying, same as every prior change in this session.
