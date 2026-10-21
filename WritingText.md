UAT #249 – implement Option 3 (Geoff's option): for a review in a CLOSED sample that has a stored Finalized date, unlocking must keep the Distributed and Finalized dates, and re-approval must use a Mgr Approval date on or before the stored Finalized date.

Use your #249 discovery (file:line) as the starting point. Open-sample reviews must behave exactly as today.

PHASE 1 – Confirm (read-only, brief): the six unlock payloads (ReviewInfoSection.tsx ~1062, 1097, 1273, 1312, 1488, 1527), the SaveAsync Review Info branch (ReviewService.cs ~161-190), GetReviewTimelineDatesAsync (SqlReviewRepository.cs ~2813-2835), and where sampleClosed is already loaded on the form (~246-266).

PHASE 2/3 – Implement the smallest change:
Backend (the real enforcement):
- Extend the existing timeline read to also return the sample's Closed flag (no new endpoint, no schema change).
- In SaveAsync, when the sample is Closed AND a Finalized date is stored:
  - reject any change to Review_distributed_date or Review_finalized_date, including clearing them (400 with a clear message);
  - reject an approvalDate later than the stored Finalized date (400, message shows the Finalized date).
- When the sample is Closed but Finalized is NULL (existing orphans), do not block: allow entering Finalized so admins can repair them.
Frontend:
- When sampleClosed is true and a Finalized date is stored, the unlock payloads must not send finalizedDate/distributedDate; keep the Distributed and Finalized inputs disabled; show a short hint: "Closed sample: Mgr Approval must be on or before the Finalized date (MM/DD/YYYY)."
- Show the backend 400 message using the app's existing error toast.
Do not touch the #212 rules for open samples, the sample close/re-open flow, or Review History SQL.

PHASE 4 – Report:
- Root cause, ADDED and REMOVED/CHANGED (file:line).
- On-screen before/after for: unlock in a Closed sample; re-approve with a date after Finalized (rejected); re-approve on/before Finalized (saved, review stays in Review History); unlock in an Open sample (unchanged).
- NULL/edge cases: Closed sample with Finalized NULL (orphan repair allowed); Distributed NULL but Finalized set; approval equal to the Finalized date; date-only comparison with no timezone shift; a crafted API request that clears finalizedDate (rejected by the server).
- Backend build + tsc + frontend build, git status (restore next-env.d.ts / tsconfig.tsbuildinfo if rewritten).
Do not commit or push.
