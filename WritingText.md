UAT #250 – Review Queue "Completed" column shows the Mgr Approved date instead of the CRO Complete date when the Approved, Distributed or Finalized status box is selected. It is correct only for "Draft Completed".

Examples from Geoff: Approved box shows 10/13/26 (Mgr Approval) instead of CRO Complete 9/24/26; Distributed shows 10/14/26 instead of 10/9/26; Finalized shows 10/14/26 instead of 10/8/26.

PHASE 1 – Read-only discovery (file:line):
1. Trace the "Completed" column end to end: Review Queue SQL (SqlReviewRepository / ReviewQueue.cs), API model, reviews.ts, review-queue/page.tsx. Which DB column feeds "Completed" for each status box? Is there a CASE/COALESCE or per-status mapping that substitutes the Mgr Approval date?
2. Check the other date columns in the same grid (Approved, Distributed, Finalized) – is each one sourced from the correct column for every status box?
3. Is the same logic used by Review Progress, Review History, sorting/filtering on Completed, and the status box counts? List every place.
4. Data check (give a read-only SQL in the report, do not save it as a script): how many reviews have a Mgr Approval date but no CRO Complete date? What would "Completed" show for them after the fix?

PHASE 2 – Smallest generic fix: "Completed" must always show the CRO Complete date, whichever status box is selected; every other date column keeps its own source. No schema change. If the bug sits in shared SQL used by other screens, fix it there once and list every screen affected.

PHASE 3 – Implement only that.

PHASE 4 – Verify and report:
- Root cause (file:line).
- ADDED and REMOVED/CHANGED (file:line).
- On-screen before/after for each status box (Draft Completed, Approved, Distributed, Finalized, and All).
- NULL/edge cases: CRO Complete NULL with Mgr Approval set; both NULL; date-only values not shifted by a day (timezone); sorting by Completed.
- Status box counts, Review Progress, Review History and other screens unchanged unless they had the same bug (list them).
- Backend build + tsc + frontend build, git status.
Do not commit or push.
