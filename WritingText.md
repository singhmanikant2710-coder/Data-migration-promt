Subject: Request: Visual Studio 2026 Enterprise license – trial expired (CASRR)

Hi Team,

My Visual Studio 2026 Enterprise (version 18.6.2) extended trial has expired, and I am now unable to use the IDE.

Account: aa.manikant.singh@firsthorizon.com

I tried "Check for an updated license" and got this error: "We could not unlock Visual Studio with the accounts available. Please ensure that you have added an account with a valid subscription and the account is authenticated."

It looks like my account does not have a Visual Studio subscription assigned. Could you please assign a Visual Studio Enterprise subscription to my account, or share the correct way to get the license?

I am actively working on the CASRR project, so a quick resolution would really help. Screenshots are attached for reference.

Thanks & Regards,
Manikant Singh



#242 – Implemented. Tested in Dev and pushed to the develop branch. Not yet deployed to QA.

What changed (CRM Findings Summary Table report only):
- A ratio is shown in bold red when it is above its KRI tolerance:
  - Unsatisfactory Transactions COUNT (# / %) for all five CRM components;
  - Unsatisfactory Transactions EXPOSURE ($ / %) for Risk Recognition only;
  - Borrower Finding Totals COUNT (# / %) for all five CRM components (exposure is not formatted).
- A ratio equal to its tolerance is not treated as a breach (e.g. 5.00% against a 5% tolerance stays normal). The comparison uses the percentage exactly as printed.
- A short note under the summary table explains the formatting: "Bold red = above KRI tolerance (CAS KRI, Selections Library)."
- Tolerances are read from the Selections Library each time the report runs, so changes you make there apply straight away. No other report is affected.

Action needed in QA/Test and Production: please add the CAS KRI tolerances in the Selections Library as described in the log – Tab "CAS KRI", Selection ID 1, with these exact Section names: RR Unsat Count, RR Unsat Dollar, RR Findings, SM Unsat Count, SM Findings, UW Unsat Count, UW Findings, CS Unsat Count, CS Findings, LA Unsat Count, LA Findings, and values such as "5%". Until a tolerance is entered, its cells simply show without formatting.

Tested in Dev: report with all values within tolerance (no formatting), and with tolerances lowered in the Selections Library to force breaches (only the breached cells shown in bold red).

I will confirm here once deployed to QA.
