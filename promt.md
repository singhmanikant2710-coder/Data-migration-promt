Haan, correct. Aapne script execute nahi kiya tha. Chakravarthy/Lekkala ne TEST me execute kiya aur unko errors aaye the. Aapko Ashok ne sirf task par follow-up aur coordination karne ko bola tha.

Isliye group me message aise likhna zyada accurate rahega:

> Hi Team, I followed up with Aditya regarding the errors encountered while executing the view migration scripts in TEST. Aditya confirmed that dbo.vw_Linesheet_Header needed to be created first, followed by dbo.vw_Linesheet_Flat. The Linesheet_Header view has now been created, so the dbo.vw_Linesheet_Flat script can be re-run and should work now.

Regarding the Msg 2714 error, Aditya confirmed that the corresponding object does not need to be migrated and can be ignored.

Chakravarthy, could you please re-run the required script and confirm if it works now?



Isme clear hai ki aapne execute nahi kiya, aapne Aditya ke saath follow-up karke resolution coordinate kiya hai, aur actual re-run Chakravarthy ko karna hai.
