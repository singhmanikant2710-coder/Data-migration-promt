
Read-only. No edits. No plan. Just report with file paths + exact code. Do NOT modify anyone's work (including Jothi's).

Context (UAT #108): On Review Form → CRM Findings, when the Finding Code is "CRM-00", the Severity field must automatically default to "N/A". A similar rule already exists (UAT #55): Severity defaults and locks to "Observation" when the Finding CRM component is "06-Servicing Systems" or "07-Data Integrity".

Report:
1) The frontend CRM Findings component/hook that renders the Severity dropdown (look in frontend/src/app/review/[ecif]/review-info/components/sections/ — CrmFindingsAndRatingsSection.tsx and hooks/useCrmFindings.ts). Paste the Severity control JSX and the exact list of Severity options — is "N/A" one of them?
2) The existing UAT #55 logic that defaults/locks Severity to "Observation" based on the component being "06-Servicing Systems" or "07-Data Integrity". Paste it exactly, and say when it fires (on finding code change, row click, or mount).
3) How is the Finding Code stored on a findings row — the raw code like "CRM-00", or a composite label? Show the field name and an example value.
4) State exactly what to change and in how many files to add: when Finding Code = "CRM-00", set Severity to "N/A", matching the #55 pattern (note whether #55 does default-only or default+lock).

Use LIVE DB, ignore columns.csv. Output findings only. Change nothing.
