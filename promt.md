Read-only. No edits. No plan. Just report with file paths + exact code. Do NOT modify anyone's work (including Jothi's).

Context (UAT #127, point 1): On Review Form -> Review Info -> Assignments, for Review 21882 the UI shows:
  REVIEWER NAME = blank (correct — CRO_name is NULL in DB)
  MANAGER = "GARY L MILLER Jr"
  EXAMINER IN CHARGE = "KEVIN M BRISKE"

In the DB for that review: CRO_name = NULL, CRO_manager_name = NULL, Relationship_mgr_name = 'KEVIN M BRISKE', Portfolio_mgr_name = 'GARY L MILLER Jr'. There is NO EIC / examiner / charge column on dbo.[02_CORE_02_Reviews].

So the UI's "Examiner in Charge" appears to be showing the Relationship Manager name, and "Manager" appears to be showing the Portfolio Manager name — both incorrect. They should be blank when the corresponding CRO fields are null.

Report:
1) The component rendering the Assignments panel on Review Info (likely ReviewInfoSection.tsx under frontend/src/app/review/[ecif]/review-info/components/sections/). Paste the JSX for REVIEWER NAME, MANAGER, REVIEWER EMAIL, MANAGER EMAIL and EXAMINER IN CHARGE, showing exactly which field each binds to.
2) In frontend/src/services/api/reviews.ts, paste the type for the review-info section showing the reviewer/manager/EIC fields.
3) In the backend read path (SqlReviewRepository.cs, the SELECT that builds the review-info section), paste the exact columns that feed reviewerName, managerName, and examinerInCharge. Is there a COALESCE/ISNULL fallback from CRO_name to Relationship_mgr_name, or from CRO_manager_name to Portfolio_mgr_name? Show the exact SQL.
4) State exactly where the incorrect mapping/fallback is, and what must change (and in how many files) so that Manager and Examiner in Charge are blank when the corresponding CRO fields are null, instead of showing the Relationship/Portfolio Manager names.

Use LIVE DB, ignore columns.csv. Output findings only. Change nothing.
