Hi Geoffrey, quick confirmation on UAT #127 (point 1) before I make the change.
I traced the issue. Using Review 21882 as the example: in the database, CRO_name is NULL (no CRO assigned), and Relationship_mgr_name is "KEVIN M BRISKE". The backend correctly returns the CRO field as empty — but the UI has a fallback that substitutes the Relationship Manager name when the CRO is blank. The same fallback applies to Manager (falls back to Portfolio Manager) and Examiner in Charge.
So on the Review Info → Assignments panel for that review you currently see:
Reviewer Name — blank (correct)
Manager — "GARY L MILLER Jr" (this is the Portfolio Manager, not a CRO manager)
Examiner in Charge — "KEVIN M BRISKE" (this is the Relationship Manager)
Just confirming before I remove the fallback: when no CRO is assigned, should all three — Reviewer Name, Manager, and Examiner in Charge — display as blank? And the same for Reviewer Email / Manager Email?
Once you confirm I'll make the change straight away.
Thanks, Manikant
