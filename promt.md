Task: On the Customer Info tab (Relationship Overview), wire THREE dropdowns to load options from the live database: "Portfolio Manager Lead", "Executive Credit Officer", and "Senior Credit Officer".

Data source (use LIVE DB, ignore columns.csv):
- Table: 03_LIBRARY_10_Distribution Parties
- Display column: Recipient_name
- Filter by Recipient_role:
  * Portfolio Manager Lead   -> Recipient_role IN ('PML','RPML')
  * Executive Credit Officer -> Recipient_role = 'ECO'
  * Senior Credit Officer    -> Recipient_role = 'SCO'

Constraints:
- READ-ONLY diagnostics first. Show me:
  (a) the CURRENT JSX for these three fields in CustomerInfoSection.tsx — are they <Field> or <SelectField>? What are their section/name/value bindings? Are options already wired (and to what source)?
  (b) whether an endpoint already returns Distribution Parties names by role; if not, propose the smallest backend addition. Reuse the existing LookupsController/ReportingService pipeline (same approach used for the relationship cascade). A single endpoint accepting one or more roles is preferred: GET /api/v1/lookups/distribution-party-names?roles=PML&roles=RPML.
- SQL: parameterized IN clause (one @param per role), "SELECT DISTINCT TOP (n)" ordering, trimmed, sorted. Return 200 with empty array if roles is empty.
- PERFORMANCE: fetch each field's options ONCE on mount (useEffect with [] dependency, cancellation guard). No re-fetch loops. Confirm dependency arrays in your plan.
- Keep each field's binding (section/name/value) unchanged so the existing Save path persists values.
- Use ensureIncludesSelected so saved values stay visible.
- Single-file edits, step-by-step, wait for confirmation at each file.


  SELECT Recipient_role, COUNT(*) AS Cnt
FROM [dbo].[03_LIBRARY_10_Distribution Parties] WITH (NOLOCK)
WHERE Recipient_role IN ('PML','RPML','ECO','SCO')
GROUP BY Recipient_role;
