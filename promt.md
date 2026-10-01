B1 verified. Go ahead with B2: the frontend wiring.

Requirements:
1. reports/page.tsx: RM and PM filter SearchableSelects load options from
   GET /api/v1/lookups/report-managers/relationship and .../portfolio
   instead of getLookupOptions("relationship-managers"/"portfolio-managers").
   Show the label, store the selected option (value + isEmployeeId).
2. In ALL 13 per-report request builders and serializePayload:
   - isEmployeeId = true  → send RelationshipManagerId / PortfolioManagerId
     = value, and leave the name filter (RelationshipManager / PortfolioManager)
     EMPTY.
   - isEmployeeId = false → send the name filter = value, and leave the ID
     EMPTY.
   - Never send both for one selection. Use ONE shared helper for this
     mapping so all 13 builders behave identically; don't hand-edit 13
     different variants.
3. All ~9 filtersEcho objects and buildFilterParagraph: the "Applied Report
   Filters" text in every PDF shows the selected option's LABEL
   (e.g. "17436 - WAGNER, JOHN C"), never a raw ID alone.
4. No filter selected: the request must be byte-identical to today.
5. Saved/restored filter state (if the page persists or restores filters):
   an old saved plain name must still work, i.e. treat it as a name-only
   selection.
6. Don't change Customer Info dropdowns, sample-load, any backend code, or
   any report layout/columns.

Report:
- ADDED / REMOVED lines (file:line)
- A table of all 13 request builders + serializePayload confirming each one
  uses the shared helper
- What the request body contains for: (a) ID option "17436 - WAGNER, JOHN C",
  (b) name-only option "JOHN C WAGNER II", (c) no RM/PM selected,
  (d) RM = ID option and PM = name-only option together
- The PDF "Applied Report Filters" text for (a)-(d)
- Frontend build result
Do not commit.
