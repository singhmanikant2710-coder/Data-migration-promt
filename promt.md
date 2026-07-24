Read-only. No edits. No plan. Just report with file paths + exact code. Do NOT modify anyone's work (including Jothi's).

Context (UAT #151): On Review Form → Review Info → Review Status, the "CRO START" field should lock once it has been initially edited, for CRO and CRA roles.

Report:
1) In frontend/src/app/review/[ecif]/review-info/components/sections/ReviewInfoSection.tsx — paste the JSX for the "CRO START" field. Which value does it bind to, is it editable, and is there any existing lock/disabled/readOnly logic on it?
2) Are there any other fields on this page that already implement a "lock once set" or role-based lock? If yes, paste that pattern (e.g. the reviewerLocked / severityLocked style logic) so it can be reused.
3) How does the app know the current user's role? Show where the role is read on the frontend (a hook, context, or the user object), and list the possible role values (e.g. CRO, CRA, ECO, SCO, Admin). Paste the exact code.
4) Which DB column does CRO START map to — is it dbo.[02_CORE_02_Reviews].[Start_date]? Show the read mapping and the save mapping for this field (backend and frontend).
5) Is there an existing "Unlock Review" capability on this page (I can see an Unlock Review button in the UI)? Show what it unlocks and which roles can use it.

Use LIVE DB, ignore columns.csv. Output findings only. Change nothing.
