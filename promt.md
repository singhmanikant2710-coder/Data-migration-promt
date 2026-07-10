Task: On the Customer Info tab (Relationship Overview), convert SEGMENT, UNIT, and MARKET into CASCADING dropdowns (SEGMENT > UNIT > MARKET), sourced from the live database.

Cascade logic (verified against live DB):
- SEGMENT: distinct [Segment] from 01_DATA_01_Data Mart Trial. Independent.
- UNIT: depends on SEGMENT.
    * If SEGMENT = 'Regional Banking' (case-insensitive) -> distinct [Region] filtered by that Segment
    * Else -> distinct [SpecialtyLine] filtered by that Segment
- MARKET: depends on SEGMENT + UNIT -> distinct [Market] filtered by Segment (and Region if Regional Banking, else SpecialtyLine).

Constraints:
- READ-ONLY diagnostics first. Show me:
  (a) current JSX for SEGMENT/UNIT/MARKET in CustomerInfoSection.tsx (section/name/value bindings),
  (b) whether the existing LookupsController/ReportingService pipeline can be reused (like we did for other lookups). Propose the smallest backend addition: methods returning distinct segments, units-by-segment, markets-by-segment-unit.
- SQL: use "SELECT DISTINCT TOP (n)" ordering (NOT "TOP (n) DISTINCT"). Parameterize all filters. Match 'Regional Banking' case-insensitively.
- PERFORMANCE (critical): 
  * Segments: fetch ONCE on mount (useEffect with [] dependency, cancellation guard).
  * Units: fetch only when segment changes (not on every render).
  * Markets: fetch only when segment or unit changes.
  * Do NOT create any effect that re-fetches in a loop. Confirm each effect's dependency array in your plan.
- Cascade behavior: when SEGMENT changes -> clear UNIT and MARKET values + reload UNIT options; when UNIT changes -> clear MARKET value + reload MARKET options.
- Keep bindings (section="customerInfo", names segment/unit/market) unchanged so Save path persists.
- Use ensureIncludesSelected so saved values stay visible on initial load.
- Single-file edits, step-by-step, wait for confirmation at each file. Use LIVE DB, ignore columns.csv.
