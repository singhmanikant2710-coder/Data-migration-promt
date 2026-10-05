Hi Geoff,

Thank you – really glad to hear everything looked good!

On #229 (D-2/DAIRIO LLC, Customer # 84935024): yes, we found the root cause.

- In the Data Mart, this customer has 32 accounts. 30 of them are under Market "Winston-Salem" and 2 (facility 3950129114, cost center 9728) are under Market "Greensboro & High Pt".
- The Load Samples process was creating one review per distinct combination of Segment / Unit / Market / LOB for a customer. Because this customer had two different Markets, it created two reviews – all 32 accounts were attached to one of them and the second review was left empty.
- Customer # 2899 loaded correctly because all of its accounts share the same Market. The comma in the name and the two customer numbers were not related to the issue.

Fix (included in this release):
- A customer now always creates exactly one review with all of its accounts. Where accounts have different Segment/Unit/Market/LOB values, the review takes them from the account with the largest commitment (Winston-Salem for this customer).
- A safeguard was added so that if a load would ever create more than one review for the same customer, the load stops with a clear message instead of silently creating a duplicate.

The two empty duplicate reviews already created in QA (Review IDs 21957 and 21942) have been flagged to the DB team via a report script, and will be removed only after review.

Thanks again for all your support!

Manikant
