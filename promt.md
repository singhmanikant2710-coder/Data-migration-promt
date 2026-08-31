Hi John,

Regarding Bug #28 — the "Reported (Monthly/Quarterly/Annually)" 
field in the covenants section not pulling over for both 
relationships — I'd like to confirm a couple of details so I can 
verify the fix against the exact scenario you saw:

1. Which customer did you observe this on? (customer name / number)

2. When you mention "both relationships" — do you mean:
   a) Two loan facilities/relationships under the same customer, or
   b) Two related customer records (e.g. parent/subsidiary), or
   c) Something else?

Context on what we've fixed today:
We identified and resolved an issue where covenant saves were 
failing because a required parameter wasn't reaching the server 
(the customer identifier). As a result, the Reported field (and 
threshold/description) were not being saved at all — so they 
would not appear/pull over anywhere.

After the fix, the Reported field now saves and persists correctly. 
I've verified this on MDR Construction Inc — the Reported value 
(e.g. "Quarterly") now saves and shows up as expected.

If your #28 was caused by the same save issue, it should now be 
resolved. But if "both relationships" refers to a specific 
copy/carry-over behavior between two relationships that's separate 
from saving, please let me know the customer and scenario so I can 
test that exact case.

Thanks!
Manikant
