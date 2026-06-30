Fix the CRM Findings header alignment issue on the Review Form screen (/review/id-{id}/review-info?section=crm-findings-and-ratings).

PROBLEM: The "REVIEW FORM — CRM FINDINGS" top bar (Edit/Save/Cancel/Email + borrower info) is aligned correctly when the page is at the top, but on scroll the header detaches/offsets from the content below it. On the static Share page the same bar works fine — so the issue is scroll-specific to this screen only.

RULES (Agent Assist — strict):
1. Read-only diagnostics first. Tell me the root cause before changing anything.
2. Single-file edit only, scoped to the CRM Findings screen's own component/CSS.
3. Do NOT touch the shared top-bar component — it works on other screens.
4. No new npm/NuGet packages.
5. STOP and ask me if the fix needs the shared component or any second file.
6. Wait for my manual approval before applying the edit.

INVESTIGATE LIKELY CAUSES: a scroll container on this page with its own overflow/height that the sticky header isn't anchored to, a transform/overflow ancestor breaking position: sticky, or the findings table scroll area not sharing the same scroll context as the header.

Diagnose first, propose the single-file fix, then wait for my approval.
