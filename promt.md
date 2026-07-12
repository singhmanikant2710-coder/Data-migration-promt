UAT #55 — CRM Findings: default Severity to "Observation" for certain components.

Client requirement:
"Can we default the Severity (Finding level) to Observation when a user selects a 06-Servicing Systems or 07-Data Integrity finding? We don't want users selecting 'Finding' for these type items."

YOUR TASK:
1. FIRST report (read-only, no edits):
   a. How the Severity dropdown currently works in CrmFindingsAndRatingsSection.tsx — its options (show SEVERITY_OPTIONS) and how the value is set/saved.
   b. Where the CRM Component change is handled for a row (the onChange that sets row.component).
2. THEN propose ONE minimal fix:
   - When a user selects CRM Component = "06-Servicing Systems" OR "07-Data Integrity" on a row, the Severity for that row should DEFAULT to "Observation".
   - Ask me to confirm: should Severity then be LOCKED (read-only/disabled) for these two components so users cannot pick "Finding", or should it just default to Observation while still allowing change? The client says "We don't want users selecting Finding for these type items" — this suggests locking, but confirm the exact wording with me before implementing.
   - Existing rows already saved with a different severity must NOT be silently overwritten on load — only apply the default when the user CHANGES the component.

Requirements:
- Single file if possible.
- Do not break the save path; the severity value saved must remain a valid SEVERITY_OPTIONS value.
- Works alongside the recent #52/#53/#54 changes.

Report findings and plan. STOP and wait for approval.
