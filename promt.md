Factoring fix (all Factoring customers, all surfaces). Evidence:
TBS FACTORING SERVICE LLC 201907 — tblMain:
perNetIncomeYTDDividedByRevenueYTD = 0.1094546 (legacy 10.95%),
curEBITTTM = 11887.767 (legacy $11,888).

1. YTD PBT Margin: UI shows 0.11%. Stored fraction must be shown as
   Access Percent (x100) -> 10.95%, on Top Strip, Monthly Summary,
   Rolling 24, Fiscal YTD, Detail grid, PDF, CSV. Also check every other
   percent field in the Factoring template for the same missing x100
   and list them (fix only those whose frm006 control is a bound Percent
   control).
2. Month/TTM "EBIT TTM" is blank. Bind it to curEBITTTM (frm006 control
   source) -> $11,888. Check the other Month/TTM rows are bound too.
3. Cash & Charge-offs: remove "Collections %" — frm006 has no such
   control in that block (it was re-added in Part D). Quote frm006 to
   confirm before removing.

EVIDENCE & SAFETY (mandatory):
- Quote frm006 control source + format for every field changed.
- Scope: Factoring only. STOP if another industry is affected.
- Regression before/after: TBS FACTORING + TOWER CAP SPV, LLC.
- No customer-specific code. Do not change values, calculations,
  persistence. Build, tests, do not commit.

REPORT FORMAT (mandatory):
- Root cause per item + ADDED / REMOVED (file:line)
- BEHAVIOUR CHANGE per surface incl. NULL case
- NOT TOUCHED


BUG (generic): after editing a value (e.g. Min Tangible Net Worth) and
"Save and Close", reopening the page shows the OLD value. The DB
(tblMainCovenants + tblMain) has the NEW value. So a cache serves stale
data.

1. READ-ONLY first: list every cache between DB and screen for the edit/
   view/report pages — backend BlackbookSummaryService payload KV cache
   (payloadVersion, 180s), MetricsController series cache (300s), client
   _summaryCache in metadataService, lib/api GET cache / in-flight dedupe,
   covenants/metadata caches. Quote file:line and TTL, and which ones
   the save path already clears.
2. FIX: after ANY successful save (Refresh/Save and Save and Close, incl.
   covenant and custom-field writes), invalidate for that customer:
   - backend payload + metrics cache entries (server side, in the save
     endpoint)
   - client caches (summary, metrics, covenants, metadata)
   so the next load always fetches fresh data. Do not disable caching
   for reads that did not change.

EVIDENCE & SAFETY: no change to values, calculations, persistence.
Test on 2 customers from different industries: edit -> Save and Close ->
reopen -> new value; revert -> reopen -> old value.
Build, tests, do not commit.

REPORT FORMAT (mandatory):
- Caches found (file:line, TTL, cleared on save yes/no)
- ADDED / REMOVED (file:line)
- BEHAVIOUR CHANGE incl. NULL case
- NOT TOUCHED
