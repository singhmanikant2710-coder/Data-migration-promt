Context: CASRR. On the Reports page, the RM and PM filter dropdowns show the
same person multiple times in different name formats, e.g. for "wagner":
  JOHN C WAGNER II / WAGNER II, JOHN C / WAGNER, JOHN C
(all Employee ID 17436), plus WAGNER, JACK C. (likely a different person).
The options come from distinct Reviews.Relationship_mgr_name /
Portfolio_mgr_name via getLookupOptions('relationship-managers' /
'portfolio-managers').

Investigate first, then fix:
1. Find how the selected RM/PM filter value is applied in every report query
   (WHERE ... Relationship_mgr_name = @x, IN (...), LIKE, etc.) and list every
   report/endpoint that uses it.

Fix (generic; must not break any report):
2. Build the RM/PM filter options grouped by Employee ID
   (TRY_CONVERT(int, Relationship_mgr_number / Portfolio_mgr_number)), one
   option per ID. Label = the Distribution Parties canonical name for that ID
   if it exists, else the most recent stored name. Do NOT merge different IDs,
   and do NOT merge by name similarity (JACK C ≠ JOHN C).
3. Reviews with NULL/all-zero ID but a stored name (≈640 RM / 750 PM legacy
   rows) stay as separate name-only options, deduplicated by exact trimmed
   name only.
4. Apply the filter consistently in every report found in step 1:
   - ID option → match all reviews with that numeric ID, regardless of the
     stored name format.
   - name-only option → match by exact name, as today.
   Selecting "WAGNER, JOHN C" must return the reviews currently split across
   all three name variants, in one run.
5. The "Applied Report Filters" text in PDFs must show the canonical label,
   not an internal ID.
6. Do not change Customer Info dropdowns, sample-load, or any report's
   layout/columns.

Report:
- ADDED / REMOVED lines (file:line)
- Every report affected, with before/after row counts for the Wagner (17436)
  filter
- On-screen change in the RM/PM filter dropdowns
- NULL / all-zero ID case, and "no filter selected" (must be unchanged)
- Backend + frontend build results
Do not commit.


Hi Geoff, FYI: 640 historical reviews have an RM name with no Employee ID,
and 750 have a PM name with no Employee ID. The app handles them (it shows
the name, and a reviewer can re-select from the Distribution Parties list),
but they can't be matched by ID. If your Friday historical reload can
populate the IDs (or NULL them out for associates no longer with the bank),
that will also clean up the duplicate RM/PM names showing in the Reports
filters.
