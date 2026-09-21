Reviewed the 30-row sample — this confirms ID-based matching is SAFE, but the
verdict logic in Query 4 has a false-negative bug: it's flagging every row as
"NAMES DIFFER" purely because of the FIRST-MIDDLE-LAST vs LAST, FIRST-MIDDLE
format difference (e.g. "NATHANIEL J SPEARS" vs "SPEARS, NATHANIEL J" — same
person). Across all 30 rows, I don't see a single case where the underlying
person is actually different — it's a format-comparison artifact, not a data
problem.

HOWEVER: ID 41249 needs a closer look specifically. In query 3's output:
- Data Mart: "JOSEPH W AGNE..." (truncated, need full value)
- Distribution Parties: "AGNETTA, JOSEPH W"
These actually might be the SAME person too (Joseph W Agnetta) — my earlier
"Michael Smaldone" read was from a blurry/reused photo and may have been
misread, or was pre-repopulation stale data. Please re-run this specific check
and paste the FULL untruncated names for ID 41249 from both sources, plus
re-check that specific review (198 Madison Ave Realty NY LLC, Review ID 21592)
in the app UI right now to see what Portfolio Manager currently displays for it.

Once 41249 is confirmed as either a genuine match (same person, format
difference only) or a real anomaly, proceed as follows:
- If confirmed clean: switch RM/PM email resolution to join on employee ID
  (Recipient_role = dropdown's employee ID) instead of name matching. This is
  now verified as reliable.
- Additionally, as a safety net (not a blocker), log a warning whenever the
  ID-matched Distribution Parties name and the Data Mart name don't agree even
  AFTER normalizing both to a common format (strip punctuation, reorder
  Last/First, drop suffixes) — this would catch a genuine future anomaly like a
  reused/reassigned employee ID without relying on manual review.
