UAT #56 — CRM Findings: Info button must open a "CAS CRM Findings Library" help screen.

I'm attaching the client's Access prototype screenshot.

Client requirement:
"The Info button does not open the needed help screen that displays our Findings library. This feature allows the users to see all findings and related finding guidance at one time to aid them in selecting the correct finding."

From the screenshot, the expected behavior is:
- There is an ℹ️ (info) icon at the TOP-RIGHT of the CRM Findings panel header (next to "Add Row").
- Clicking it opens a modal titled "CAS CRM Findings Library".
- The modal shows a scrollable table with THREE columns: FINDING CODE | DESCRIPTION | GUIDANCE.
- It lists ALL findings from the library (all components together, e.g. CRM-00, RR-101, RR-102, CS-*, SS-*...), each with its full description and full guidance text.
- Guidance text is long — it must WRAP over multiple lines, not truncate.

CONTEXT (reuse — do not refetch):
- The CAS Findings library is ALREADY fetched client-side in CrmFindingsAndRatingsSection.tsx (from the #53/#54 work). Each item contains: component, findingCode, description, category, GUIDANCE.
- If the current code only retains labelMap/categoryMap, keep the raw library items (or add a guidanceMap) from the SAME existing fetch — no new API call.

YOUR TASK:
1. FIRST report (read-only, no edits):
   a. Is there already an info icon in the CRM Findings panel header? What does it currently do (if anything)?
   b. Does the codebase already have a reusable MODAL/dialog component (search for existing modals — e.g. the delete-confirmation modals used in the Maintenance tabs)? Show how it's used. We want to REUSE it, not build new.
   c. Confirm the guidance text is reachable from the already-fetched library data in this component, or what minimal change keeps it.
2. THEN propose ONE minimal fix implementing the modal as described.

Requirements:
- No new API calls, no fetch loops — reuse the already-fetched library.
- DISPLAY ONLY — no changes to save logic or persisted data.
- Modal: scrollable body (max-height ~70vh), sticky header row, close on X / Esc / outside click.
- Columns: Finding Code (narrow, ~100px), Description (medium), Guidance (widest) — all wrapping, no truncation, no horizontal scroll.
- Sorted sensibly (e.g. by component then code).
- Styling consistent with the app (dark navy #1F3864 header, same fonts/padding as existing modals).

Report findings and plan with exact files touched. STOP and wait for approval.
