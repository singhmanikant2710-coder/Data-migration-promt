Good analysis, especially catching the fan-out risk on (c) — that's a real
defect risk regardless of what we decide on (a). Two things to do now, no
implementation yet:

1. Re-run the RM/PM Distribution Parties match-rate query against CURRENT
   data (post 930-user repopulation) — same query style as before, just
   confirm current % match for RM and PM separately. This directly informs
   whether NULL-on-miss is viable.

2. Also check: does any single Employee ID appear on more than one row in
   Distribution Parties right now? (SELECT Recipient_role, COUNT(*) FROM
   [03_LIBRARY_10_Distribution Parties] GROUP BY Recipient_role HAVING
   COUNT(*) > 1). This tells us whether (c)'s fan-out risk is theoretical or
   already present in the data.

Hold off on implementing the join itself — whatever we build must resolve via
correlated TOP(1) subqueries (or equivalent), never a plain LEFT JOIN, exactly
as you flagged. That part isn't up for debate regardless of Geoff's answer on
NULL-vs-fallback.

I'm taking (a) and the backfill question to the client — will come back with
direction once I hear back.
