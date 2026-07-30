Re: Issue #170 — Report Name Selection sort order
Hi Geoff,
Quick heads-up before I implement this. The dropdown you want sorted by Selection_id is powered by a shared backend query that also feeds several other dropdowns in the app. So I want to confirm the intended scope.
The same data source currently drives these dropdowns (all sorted alphabetically today):
Report Name Selection (Reporting page) — the one you flagged
FHN Portfolio Segment — Customer Info section
PD Grade — Scorecards & Transactions sections
LGD Grade — Scorecards & Transactions sections
Values list — Maintenance → Selections screen
Two ways I can do this:
Option A (global): Change the sort to Selection_id ascending everywhere. This is a clean one-line change and would also reorder the other four dropdowns above. For fields like PD Grade / LGD Grade this is arguably better, since Selection_id usually reflects the intended grade order rather than alphabetical.
Option B (Reporting only): Sort only the Report Name dropdown by Selection_id and leave the other four exactly as they are today. This keeps the change isolated but requires a bit more work on the backend.
Could you confirm which you'd prefer — should all these dropdowns move to Selection_id order, or only the Report Name Selection dropdown?
Thanks,
Manikant
