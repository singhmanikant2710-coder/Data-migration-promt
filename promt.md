Task: On the Customer Info tab (Relationship Overview), convert SEGMENT, UNIT, and MARKET into CASCADING dropdowns (SEGMENT > UNIT > MARKET), sourced from the live database.

Cascade logic (per UAT requirement):
- SEGMENT: distinct values of [Segment] from 01_DATA_01_Data Mart Trial. Independent (not dependent on Unit/Market).
- UNIT: depends on SEGMENT.
    * If SEGMENT = 'Regional Banking'  -> distinct [Region] values (filtered by chosen Segment)
    * Else                              -> distinct [SpecialtyLine] values (filtered by chosen Segment)
- MARKET: depends on SEGMENT and UNIT -> distinct [Market] values filtered by chosen Segment (and Unit).

Constraints:
- READ-ONLY diagnostics FIRST. Show me:
  (a) Confirm these columns exist on 01_DATA_01_Data Mart Trial (live DB): Segment, Region, SpecialtyLine, Market. Give a few sample distinct values of each, and confirm exact column names/spelling.
  (b) The CURRENT JSX for SEGMENT, UNIT, MARKET in CustomerInfoSection.tsx — exact bindings (section/name/value). I believe they are currently read-only <Field>. Confirm.
  (c) Whether cascading (dependent) lookups can reuse the existing CustomerInfoLookup repository/controller pattern. Propose the smallest backend addition: likely endpoints that accept a segment (and unit) filter and return distinct dependent values. Follow the same pattern as distribution-party-names.
- Then propose the FULL plan (backend endpoints + frontend service + cascading state wiring) BEFORE editing.
- Keep each field's binding (section/name/value) unchanged so the existing Save path persists values.
- Cascading behavior: when SEGMENT changes, reset/reload UNIT options; when UNIT changes, reset/reload MARKET options. Keep current saved values visible via ensureIncludesSelected on initial load.
- Single-file edits, step-by-step, wait for my confirmation at each file.
- Use LIVE DB, ignore columns.csv. I will run any read-only SQL in SSMS and paste results.
