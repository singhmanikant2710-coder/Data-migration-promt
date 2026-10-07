Hi Geoff,

Thank you for narrowing down the steps – that made it quick to confirm. The Collateral / PSOR / SSOR rating issue is now fixed.

Root cause
When the Risk Rating Justification was saved, the app only sent the updated text, but the save logic on the server also rewrote the three rating fields and cleared them because they weren't included. The summary boxes were not the cause – they only display the values – but they showed the result (blank ratings displayed as "Weak").

What we fixed
- Saving any section now updates only the fields that were actually changed; all other values stay exactly as stored.
- Applied to Risk Rating Justification, Collateral, Repayment, Scorecards and Key Risks. The same issue was also clearing the Key Risks follow-up flag and notes, which is now fixed as well.
- As a preventive step, the same safeguard was added to Covenants and Policy Exceptions. In Policy Exceptions it also stops the Variance Mitigation value from occasionally being cleared.

Testing
We verified: saving Risk Rating Justification keeps all ratings; Key Risks keeps the follow-up details; Covenants and Policy Exceptions keep all their fields when only one value is edited; and changing a value on purpose still saves correctly.

After deployment
Ratings that were already cleared before this fix cannot be restored automatically. If any reviewer notices missing Collateral, PSOR or SSOR ratings (or Key Risks follow-up details), they just need to re-select them once and save.

The fix will be included in the next deployment. Please let me know if you have any questions.

Thanks,
Manikant

