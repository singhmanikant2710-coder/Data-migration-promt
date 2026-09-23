Hi Geoff,

Investigated the leading-zero question — here's what we found:

Recipient_role (Distribution Parties), and Relationship_mgr_number /
Portfolio_mgr_number (Reviews) are all declared as int in the database
schema — not text. This means leading zeros (like the 5 in "00030") are lost
at the moment the value is stored, regardless of what the source file
contains. It's not a UI display issue — our screen is correctly showing what
the database actually holds.

By contrast, Data Mart Trial's OfficerNumber and PM Number are stored as text
(nvarchar), so they could technically hold leading zeros if the source data
has them.

This is a data-type decision, not something we can adjust from the
application side. If preserving leading zeros matters (e.g., if Employee IDs
are meant to always be a fixed 5-digit format), the fix would be:
1. Change Recipient_role, Relationship_mgr_number, and Portfolio_mgr_number
   to a text type (varchar/nvarchar), and
2. Make sure the DBA team's load/reload process writes the ID with its
   leading zeros intact (as text, not as a number).

If leading zeros aren't actually meaningful for matching/reporting purposes
(i.e., 30 and 00030 always refer to the same employee and nothing downstream
cares about the zero-padding), then no change is needed — our RM/PM
email-resolution joins already convert both sides to integers when matching,
so this doesn't affect correctness there either way.

Let us know which way you'd like to go, and we'll coordinate with Ashok if a
schema change is needed.

Thanks,
Manikant
