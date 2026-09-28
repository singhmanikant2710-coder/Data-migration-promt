SELECT strCovenantName1, strCovenantName2, strCovenantName3,
       strCovenantName4, strCovenantName5,
       dblCovenantActual1, dblCovenantActual2, dblCovenantActual3
FROM tblMain
WHERE strCustomerName = 'WESTLAKE SERVICES LLC' AND strMonthKey = '202604';

READ-ONLY. WESTLAKE SERVICES LLC 202604 (IndirectAuto): in the browser
the Monthly Summary first renders "Other 1 (%)" and the custom fields
with $ / %, then they disappear / lose symbols after a moment.
Attached: Network status + summary payload JSON, and tblMain slot
captions. /api/v1/covenants and metadata return "Other 1 (%)".

1. Trace the render sequence: which effect/fetch replaces the first
   column set (fallback -> payload), and which exact line then drops
   "Other 1 (%)" and coerces the custom values. Quote file:line.
2. From the payload JSON: what Order does "Other 1 (%)" get and why
   (backend slot resolution vs tblMain captions)?
3. Check caches: payload/profile keys, Redis/in-memory, client cache —
   could an old cached payload be served?
Report only.
