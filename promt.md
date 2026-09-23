Hi Geoff,

Confirmed — proceeding with NULL-on-miss as specified. Thanks for confirming
the historical migration is on your team's side next week; we'll only handle
newly loaded reviews going forward.

Breakdown of the mismatches, split by cause, on the 23,743 distinct customers
in current Data Mart Trial:

Relationship Manager:
- 2,136 (9.0%) have a NULL OfficerNumber in Data Mart Trial itself — no
  officer assigned at the source, so no match was ever possible here.
- 9,469 (39.9%) have a valid OfficerNumber, but it doesn't match any record in
  Distribution Parties — this is the genuine data-quality gap.
- 12,138 (51.1%) matched successfully.

Portfolio Manager:
- 4,172 (17.6%) have a NULL PM Number in Data Mart Trial itself.
- 4,359 (18.4%) have a valid PM Number with no Distribution Parties match.
- 15,212 (64.1%) matched successfully.

So the "genuine gap" (valid number, no match) is the larger driver for RM
(39.9%) and roughly equal to the NULL-source gap for PM. If closing that gap
matters, it would mean adding the missing officers to Distribution Parties —
happy to pull a list of the specific unmatched OfficerNumbers/PM Numbers if
useful for reconciliation.

We'll move forward with implementing NULL-on-miss for the sample-loading
process now.

Thanks,
Manikant
