Bug: Share via Email To/CC are NOT pre-filled on review 21592, even after
restarting frontend + API and a hard refresh. The DB has the emails:
Relationship_mgr_email = [paste]
Portfolio_mgr_email = [paste]

Your earlier verification used mock data. Trace the REAL data path:
1. Which GET endpoint feeds response.form.customerInfo on the review page?
   Does its SQL SELECT and DTO include Relationship_mgr_email,
   Portfolio_mgr_email, Portfolio_mgr_lead_email, SCO_email, ECO_email?
   What property names reach the frontend (exact casing)?
2. Do those names match what the modal pre-fill reads? Report file:line on
   both sides.
3. Fix the actual gap generically (add the missing columns/properties to the
   read path, or correct the property names). Don't change the save path,
   the resolver, or any other screen.

Report root cause (file:line), ADDED/REMOVED (file:line), and the real API
response fields for review 21592 after the fix. NULL case: a review with all
emails NULL must still open with empty To/CC and no errors. Build results.
Do not commit.
