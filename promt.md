Query 3 came back empty — no duplicate Employee IDs currently in Distribution
Parties. No live defect in the shipped resolver today. As a defensive measure
(protects against a future duplicate being introduced), please still add a
deterministic ORDER BY to the existing TOP(1) resolver — low priority, not
blocking anything, just good practice given we now know duplicates are
possible in principle.

Match-rate numbers (Query 1 & 2) are in — sharing with the client now to
decide on the NULL-vs-fallback question. Will follow up once we have
direction.

Hi Geoff,

Match-rate numbers on the post-repopulation data (the number that decides the
NULL-on-miss question):

Out of 23,743 distinct customers currently in Data Mart Trial:
- Relationship Manager resolves via Distribution Parties for 51.1% (12,138) —
  48.9% would have no match.
- Portfolio Manager resolves for 64.1% (15,212) — 35.9% would have no match.

So under your spec as written (default to NULL when there's no match), roughly
half of newly loaded reviews would load with a blank RM, and about a third
would load with a blank PM — versus today, where all of them get a Data Mart
name/number (just not always a matching email).

Given these numbers, do you want to proceed with NULL-on-miss as specified, or
would you prefer we keep the Data Mart name/number as a fallback (leaving only
the email blank) when there's no Distribution Parties match? Let us know and
we'll move forward with implementation.

Also flagging separately: no duplicate Employee IDs currently exist in
Distribution Parties, so no immediate data-integrity concern there.

Thanks,
Manikant
