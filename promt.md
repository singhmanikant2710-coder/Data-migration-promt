SELECT DISTINCT [Finding_level]
FROM dbo.[02_CORE_02_Reviews] WITH (NOLOCK)
WHERE [Finding_level] IS NOT NULL;


Read-only. No edits. No plan. Just report with file paths + exact code. Do NOT modify or revert anyone's existing work (including Jothi's).

Context (UAT #108): On Review Form → CRM Findings, when the Finding Code is "CRM-00", the Severity (Finding_level) must automatically default to "N/A".

A very similar rule already exists (UAT #55): Severity defaults AND locks to "Observation" when the Finding CRM component is "06-Servicing Systems" or "07-Data Integrity".

Report:
1) The component rendering the CRM Findings rows and the Severity field. File path + the exact JSX for the Severity control.
2) The existing UAT #55 logic — where Severity is defaulted and/or locked based on the component value. Paste that code exactly, including where it lives (component, hook useCrmFindings.ts, or elsewhere) and when it fires (on finding code change, on row click, on mount).
3) What are the available Severity options and where do they come from? Is "N/A" one of them, and what is its exact string?
4) How is the Finding Code stored on a findings row — the raw code (e.g. "CRM-00"), or a composite label? Show the field name and an example value.
5) State exactly what must change, and in how many files, to add the CRM-00 rule following the same pattern as #55.

Use LIVE DB, ignore columns.csv. Output findings only. Change nothing.
