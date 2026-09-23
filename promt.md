CRITICAL — found via manual QA testing. When Relationship_mgr_name is NULL on
a review (the new expected behavior from the sample-load NULL-on-miss fix),
the Customer Info page's RELATIONSHIP MANAGER field is incorrectly displaying
the review's CRO Name instead of showing blank/"Select...".

Confirmed via direct DB query: Review_id 21888 (MILES H MORRIS) has
Relationship_mgr_number/_name/_email all NULL. But the Customer Info page
shows "Geoffrey Houlditch" in the RELATIONSHIP MANAGER field — which is this
review's CRO Name (visible in the Load Samples grid when this customer was
added), not the RM.

Portfolio Manager Lead, Executive Credit Officer, and Senior Credit Officer
fields correctly show blank on this same screen when their underlying data is
NULL — only Relationship Manager has this bug. This strongly suggests the RM
display logic somewhere falls back to CRO_name when Relationship_mgr_name is
null/empty, rather than falling back to nothing.

Investigate:
1. Find the Customer Info page's Relationship Manager field display logic —
   search for wherever it reads Relationship_mgr_name / Relationship_mgr_number
   for display, and check if there's a fallback chain (?? or ||) that pulls in
   CRO_name or similar when the RM fields are empty.
2. This could be in the frontend component itself, or in an API response
   mapper on the backend that's incorrectly aliasing/defaulting the RM field.

Fix: when Relationship_mgr_name/_number are NULL, the field must show blank/
"Select..." — exactly like PML/ECO/SCO already do — never CRO Name or any
other person's name.

This is urgent: as of the sample-load fix we just shipped, RM will now be
NULL far more often (confirmed ~49% of new reviews), so this bug will surface
constantly and could cause reviewers to see completely wrong people listed as
their Relationship Manager.

After fixing, re-verify: reload the Customer Info page for Review_id 21888 —
RELATIONSHIP MANAGER should show blank/"Select...", not "Geoffrey Houlditch".
Also spot-check 2-3 other reviews with NULL RM to confirm this wasn't
isolated to one record.
