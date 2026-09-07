Hi Geoff,
I've done a detailed analysis of the Non-Compliant Covenants report against the Covenant Violations prototype you shared (09_Covenant Violations.pdf). Before I rebuild it, I need to confirm a few decisions — the prototype differs from the current report in more than just styling, so I want to get these right rather than guess and rework later.
Here's what I found and what I need from you:
Context — what's different today
The current "Non-Compliant Covenants" report shows one page per borrower with a covenant detail table (Frequency, Grace Days, Threshold, Eval Dates, Status). The prototype is a completely different layout: summary totals up top, a Monitoring vs Performance breakdown, a violation details table, and an applied-filters page. So this is a rebuild of the report, not just a header/footer tweak. It also needs new data from the database that the current report doesn't produce (borrower counts, exposure totals, a Commitment column, monitoring/performance grouping).
I'll handle all the rebuild work — I just need these 5 answers to build it correctly:
1. Footer — logo or no logo?
The prototype PDF actually contains the First Horizon star logo in the footer, next to "CAS RiskReview • Page X of Y". However, our other CRM reports use a cleaner footer: just " • Page X of Y", centered, with no logo. Which do you want — match the prototype (with the FH logo), or follow our house standard (report name + page number, no logo)?
Impact: purely visual, no data effect. Easy either way.
2. Header title — what should the banner say?
The prototype's header reads "Covenant Violations". Should the Non-Compliant Covenants report's banner say "Covenant Violations", or keep "Non-Compliant Covenants"? Related: are "Covenant Violations" and "Non-Compliant Covenants" meant to be two separate reports, or is one replacing the other? (There are currently two separate report entries in the system.)
Impact: affects naming and whether we consolidate two reports into one.
3. THRESHOLD vs RESULT columns — the prototype looks mismatched.
In the prototype's detail table, the values under THRESHOLD and RESULT appear swapped — the frequency (Annual/Monthly) shows under THRESHOLD, and the threshold value (e.g. 6.00x) shows under RESULT. Can you confirm the intended column mapping? i.e. what should actually appear under THRESHOLD vs RESULT vs a Frequency column?
Impact: gets the detail table columns right. If I follow the prototype literally, the columns may be labeled wrong.
4. Exposure basis — Commitment or Balance?
The exposure figures in the prototype match the borrowers' Commitment amounts (not their outstanding Balance). The current reports calculate exposure from Balance. Should exposure be based on Commitment (matching the prototype)?
Impact: this changes the exposure dollar figures shown. Since this is a risk report, I want your confirmation before changing how exposure is calculated.
5. Which violations to include?
The prototype shows rows with statuses "Past Due" and "Not Compliant" (and there's a "Waived" example too). The current report only includes strictly "Non-Compliant" covenants and filters out Past Due / Waived. Should the report include Past Due, Waived, and Not Compliant — i.e. all covenant violations — or only strictly Non-Compliant ones?
Impact: determines which rows appear in the report. With the current strict filter, some of the prototype's own sample rows wouldn't show up.
One more thing (I'll fix regardless): There's a routing bug — selecting "Non-Compliant Covenants" in the report dropdown currently opens the Covenants Summary report instead. I'll fix that as part of this work.
Once you confirm the above, I'll rebuild the report to match. Thanks!
