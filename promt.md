Hi Geoffrey, UAT #127 update — the assignment fields are now working as you described:
Reviewer Name and Manager now display blank when no CRO is assigned, instead of falling back to the Relationship Manager / Portfolio Manager names.
Examiner in Charge now comes from the sample's EIC_Name in the Samples table, rather than the CRO name. Verified against the DB — it populates correctly for samples that have an EIC, and stays blank for the older samples that don't.
The remaining items on #127 are the FHN Portfolio and FHN NAICS Industry fields, which are still blocked on the Data Mart Trial columns (InternalPortCat / IntRepCMLSubCategory) being populated. I re-checked both Dev and QA earlier today and they're still empty.
Thanks, Manikant
