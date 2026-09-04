Hi Team, I’ll be logging off early today due to a medical emergency. Apologies for any inconvenience, and thank you for your understanding.

Hi Geoff,
I've investigated the Primary Key error on the CAS Findings library edit, and I'd like to confirm the intended behavior before implementing, as it affects data consistency.
What's happening: In the database, each Finding Code is unique and belongs to exactly one Component — the codes follow a strict per-component pattern:
00-CRM Admin → CRM-xx
01-Risk Recognition → RR-xxx
02-Scorecard Management → SM-xxx
03-Underwriting → UW-xxx
04-Credit Servicing → CS-xxx
05-Loan Administration → LA-xxx
06-Servicing Systems → SS-xxx
07-Data Integrity → DI-xxx
(Confirmed: no Finding Code exists under more than one Component.) The Category and Description are also driven by the Finding Code.
The error occurs because, in edit mode, the Component is editable but the Finding Code is not. When you change the Component (e.g. CRM-00 from "CRM Admin" to "Scorecard Management") and save, the Finding Code CRM-00 collides with the existing one — and it also wouldn't match Scorecard Management's code pattern (SM-xx). This is the inconsistency we need to resolve.
Since Component and Finding Code are tightly linked, the current mix (one editable, one not) is the root cause. There are two consistent ways to fix it — please confirm which you'd prefer:
Option 1 (recommended): Component becomes read-only; Finding Code stays the key.
When editing a finding, Component and Finding Code are both locked (they define the finding's identity). You can edit Category, Description, Guidance, Finding Group, and Active. To place a finding under a different component, use "Add Finding" to create a new one. This matches the original data model, where the Finding Code drives the Component/Category/Description.
Option 2: Make everything editable with linked dropdowns.
Selecting a Component filters the available Finding Codes for that component, and selecting a Finding Code auto-populates its Category/Description. This allows re-assigning across components but is a larger change and effectively means replacing the finding.
My recommendation is Option 1 — it's consistent with the existing data model, prevents mismatched codes, and resolves the Primary Key error cleanly. Let me know which you'd like and I'll implement accordingly.
