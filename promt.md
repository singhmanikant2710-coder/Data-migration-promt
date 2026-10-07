Haan, Gangai ka response clear hai. Unka branching strategy basically ye hai:

1. release/1.0.0 → current production baseline/source of truth hai. Ab freeze rahega; isme koi new change nahi aayega.


2. develop → ab saara new development yahin hoga.


3. release/1.1.0 → develop ke required changes se bana hai aur abhi QA/next production release ke liye use hoga.


4. release/1.0.0 → release/1.1.0 merge nahi hoga.


5. Jab release/1.1.0 production mein successfully release ho jayega, woh bhi freeze hoga aur uske baad develop ka new baseline/source of truth release/1.1.0 banega.



Important point

Iska matlab ye hai ki ab aapko develop mein changes karne hain, aur next release ke liye wahi changes release/1.1.0 mein promote honge.

Aap Gangai ko short confirmation ke liye ye reply kar sakte ho:

> Thanks Gangai, understood. So going forward, we will continue all new development on the develop branch, and release/1.1.0 will be used for the current QA/production release. We will not merge any changes from release/1.0.0 into release/1.1.0.

Once release/1.1.0 is released to production and frozen, it will become the new baseline/source of truth for develop. This clarifies the branching strategy for us. Thanks.
