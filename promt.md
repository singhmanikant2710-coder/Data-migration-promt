Hi [DB team contact],

Since you're already updating RM/PM-related data, a few things worth
bundling into this pass so we don't need a separate reload later:

1. Leading zeros: Recipient_role (Distribution Parties), Relationship_mgr_
   number, and Portfolio_mgr_number are all currently declared as int, which
   drops leading zeros from Employee IDs (e.g. 00030 becomes 30). If these
   need to preserve leading zeros, this would be the right time to change
   them to varchar/nvarchar before/during this reload — otherwise it'll need
   a second pass.

2. Could we add a unique constraint on Recipient_role in Distribution
   Parties? Our application logic resolves RM/PM/PML/ECO/SCO emails by
   joining on this column as an Employee ID, and while there are no
   duplicates today, a DB-level constraint would prevent one from being
   introduced later and silently causing incorrect email matches.

3. Following up on the two Portfolio Managers from the gap-analysis list
   that Geoff mentioned he'd add to Distribution Parties himself — just
   confirming whether that's done, so we can re-check the match rate.

Let us know if any of this needs to wait for a separate pass.

Thanks,
Manikant
