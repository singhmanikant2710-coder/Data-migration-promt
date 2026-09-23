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


Hi Geoff,

Investigated the sample-loading process for RM/PM per items 4-5 of your spec.
Confirmed: Distribution Parties is currently not referenced anywhere in that
process — RM/PM fields on new reviews are populated entirely from Data Mart
Trial (in the FIRST MIDDLE LAST format), and Relationship_mgr_email /
Portfolio_mgr_email aren't populated at all on load (they stay NULL until
someone edits RM/PM manually on the review form). Before we build this, two
things need your call:

1. Your spec says: if there's no Distribution Parties match, default
   Number/Name/Email to NULL for a reviewer to assign manually. Today, 100%
   of new reviews get a Data Mart number and name — none are ever blank. If
   we implement your spec as written, any review whose RM/PM doesn't have a
   Distribution Parties match would load with a BLANK RM/PM (no name, no
   number) instead of what it shows today. Is that the intended behavior —
   force deliberate manual assignment whenever there's no match — or would
   you prefer we keep the Data Mart name/number as a fallback (and only
   leave email blank) when there's no match? This also affects consistency
   with the review-form save behavior we just shipped, which falls back to
   the Data Mart name rather than going blank.

2. Should this only apply to newly loaded reviews going forward, or do you
   also want existing reviews backfilled with the canonical Distribution
   Parties name/number/email where a match exists? Your original reporting-
   consistency concern (JOHN DOE vs DOE, JOHN across review history) would
   only be fully resolved if historical reviews are included — a load-time-
   only fix leaves existing records as-is.

We're re-checking the RM/PM match rate now that the full 930-record
Distribution Parties reload has landed, since that's the main input to how
often reviews would end up blank under option 1 above. Will share once we
have it.

Thanks,
Manikant
