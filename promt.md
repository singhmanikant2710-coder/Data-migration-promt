Task: On the Customer Info tab (Relationship Overview section), convert THREE fields into editable dropdowns sourced from the live database. These are: "Portfolio Manager Lead", "Executive Credit Officer", and "Senior Credit Officer".

Data source (use LIVE DB, ignore columns.csv):
- Table: 03_LIBRARY_10_Distribution Parties
- Filter by Recipient_role:
  * Portfolio Manager Lead  -> Recipient_role IN ('PML','RPML')
  * Executive Credit Officer -> Recipient_role = 'ECO'
  * Senior Credit Officer    -> Recipient_role = 'SCO'
- Use the party name column from that table as the dropdown display text (inspect the table's columns on the live DB first and tell me which column holds the person/party name).

Constraints:
- READ-ONLY diagnostics FIRST. Show me:
  (a) the live DB columns of 03_LIBRARY_10_Distribution Parties, and a few sample rows for each of the three Recipient_role filters (PML/RPML, ECO, SCO), so we confirm which column is the display name.
  (b) the CURRENT JSX for these three fields in CustomerInfoSection.tsx. I recall they may already be <SelectField> placeholders — show me exactly what's there now, including their section/name/value bindings and whether options are wired.
  (c) whether an existing endpoint already returns Distribution Parties by role; if not, propose the smallest backend addition following the same pattern we just used for naics-industry (CustomerInfoLookup repository/controller).
- Then propose the FULL plan before any edits.
- Keep each field's binding (section/name/value) unchanged so the existing Save path persists values.
- Single-file edits, step-by-step, wait for my confirmation at each file.
