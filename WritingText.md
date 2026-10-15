Read-only follow-up on UAT #233 (no edits):
1. Covenant accuracy Yes/No fields: with your change, a NULL column now stays NULL while the UI shows "No". List every PDF/report/screen that reads Covenant_tracking_accuracy, Covenant_validation_accuracy, Covenant_breaches_addressed, Stepped_up_servicing (file:line). Does each one read through the same GET mapping (NULL shown as "No") or raw from DB (NULL shown blank)? Before/after output for a review where these are NULL.
2. PE mitigation: how does your "nothing stored yet" check detect it (file:line)? If Policy_exception_mitigation is NULL in DB, what does the GET return – NULL or "Not Mitigated"? Confirm a brand-new review adding its first PE row still saves "Not Mitigated" and "" notes.
Report only. Do not commit or push.
