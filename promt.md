Read-only. No edits. No plan. Just report file paths + exact code. Do NOT modify anyone's work (including Jothi's).

Context (UAT #167): On Review Form → Collateral → Collateral Summary, the "FACILITY NUMBER" column is empty. It should display dbo table column [Facility_number_calc], falling back to [Facility_number] when [Facility_number_calc] is NULL.

Report:
1) The component rendering the Collateral Summary table (likely a Collateral section under frontend/src/app/review/[ecif]/review-info/components/sections/). Paste the JSX for the FACILITY NUMBER column and what row field it binds to.
2) The API type for a collateral summary row (frontend service). Does it include facilityNumber / facilityNumberCalc?
3) The backend repository/query that builds collateral summary rows. Which table and columns does it SELECT? Is [Facility_number_calc] and/or [Facility_number] selected, and how mapped? Give the table name.
4) State exactly what must change and in how many files to display COALESCE(Facility_number_calc, Facility_number) in that column.

Use LIVE DB, ignore columns.csv. Output findings only. Change nothing.
