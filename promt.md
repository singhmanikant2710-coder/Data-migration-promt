Bug #34 — 202604 (April 2026) not appearing in blackbook PDF
Reported by: Pamela Sullivan

STATUS: Resolved.

ROOT CAUSE: The blackbook data/PDF was limited to a calendar-year
(≈12-month) window and, for non-December fiscal-year customers,
the month range didn't include all fiscal-year months. So months
like 202604 could be entered and visible in the edit view but
were excluded from the PDF's month set.

FIX: Resolved as part of the general fiscal-year-aware month
logic and the rolling-24 PDF correction — the data query now
loads the correct fiscal-year months, and the PDF renders the
full month set rather than a truncated ~12-month window.

VERIFIED: 202604 now appears correctly in both the blackbook
edit view AND the blackbook PDF (view and download confirmed).
December fiscal-year customers are unaffected.


═══════════════════════════════════════════════════════════════
 Bug #32 — Covenants: unable to save / update covenants on the
 Customer View/Edit screen
 Reported by: Jay Luckett (8/10/2026) + others (group chat)
═══════════════════════════════════════════════════════════════

STATUS: Resolved. Covenant field values (threshold, reported,
description) and ordering now save correctly for all customers.

───────────────────────────────────────────────────────────────
 SYMPTOMS
───────────────────────────────────────────────────────────────
• "Save failed — Invalid customer name (400): Provide a non-empty
  'customer' query parameter."
• "Partial save — Some covenant changes could not be saved."
• Covenant ORDER saved, but field values (threshold, reported,
  description) came back as 0/NULL in the database.

───────────────────────────────────────────────────────────────
 ROOT CAUSE
───────────────────────────────────────────────────────────────
The covenant save calls send the customer as a QUERY parameter
(?customer=...). However, the shared apiClient merges query
options into the request BODY for non-GET requests instead of
appending them to the URL. As a result:

  • PUT /api/v1/covenants/by-name   (per-covenant field save)
  • POST /api/v1/covenants/order    (bulk order save)

…were sending an EMPTY customer to the server → 400 error and a
partial/failed save.

Because per-row saves failed, the threshold/reported/description
values never reached the database, so they persisted as 0/NULL,
while only the order (via a separate path) appeared to save.

───────────────────────────────────────────────────────────────
 FIX
───────────────────────────────────────────────────────────────
Appended the required parameters directly to the URL query
string (instead of relying on apiClient's { query } option,
which places them in the body):

1) frontend/src/lib/covenants.ts — updateCovenantByComposite:
   PUT /api/v1/covenants/by-name
       ?customer={cust}&monthKey={mk}&covenantName={cov}
   (customer, monthKey, covenantName now in the URL, encoded)

2) frontend/src/app/customer/edit/page.tsx — order save:
   POST /api/v1/covenants/order?customer={qpName}
   (customer now in the URL, encoded)

With the customer reaching the backend correctly, the full
covenant payload (threshold, reported/reportedText, description)
is accepted and persisted.

───────────────────────────────────────────────────────────────
 VERIFIED (MDR CONSTRUCTION INC, 202601)
───────────────────────────────────────────────────────────────
Entered and saved covenant field values; confirmed the payload
and the database:

   Covenant                  Threshold   Reported    Description
   Max Debt/EBITDA           1.50        Quarterly   Testing 1
   Max Default Ratio         1.00        Quarterly   Testing 2
   Min Collateral Recovery   2.00        Quarterly   Testing 3
   Max A/R Related Parties   2.50        Quarterly   Testing 4
   Max Debt/Net Worth        1.25        Quarterly   Testing 5

All values (threshold, reported, description) now persist to the
database and reload correctly. The order/sequence also saves.
No "Save failed" or "Partial save" warnings.

───────────────────────────────────────────────────────────────
 IMPACT
───────────────────────────────────────────────────────────────
Applies to all customers — the fix is in the shared covenant
save calls, so every customer's covenant add/update now works.
Reported by multiple users; this resolves it for all of them.
═══════════════════════════════════════════════════════════════
