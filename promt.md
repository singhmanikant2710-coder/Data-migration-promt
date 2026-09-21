Important context update: Geoff has confirmed with First Horizon's DB team that
[03_LIBRARY_Distribution Parties] is being intentionally repopulated —
Recipient_role is being repurposed specifically to store Employee ID (this was
a deliberate schema decision, not an accident). This confirms our finding that
Recipient_role holds numeric employee IDs.

This also likely explains the ID 41249 mismatch I flagged (Michael Smaldone vs
Agnetta, Joseph W) — that may have been comparing pre- and post-repopulation
data, not a genuine data-quality problem.

Action: confirm with me whether the DB team's repopulation (clearing and
reloading Distribution Parties with the 930-user table) has completed in our
environment yet. Once confirmed complete, re-run the ID 41249 check
(and a broader sample) against the CURRENT data before finalizing whether to
proceed with employee-ID-based matching for RM/PM email resolution. Don't
finalize the matching-logic switch until we've verified against post-repopulation
data.
