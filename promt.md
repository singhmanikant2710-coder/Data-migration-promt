Hi Geoffrey, I dug into the sample-load logic for UAT #127 and have clarity on the three points. A couple need your confirmation before I make changes:
FHN Portfolio & FHN NAICS Industry (points 2 & 3):
The load query currently sources these from Data Mart Trial exactly as the ticket specifies — Internal_portfolio from InternalPortCat, and NAICS_industry from IntRepCMLSubCategory. The reason they load blank is that both of these columns are entirely empty in the Data Mart Trial table (all NULL across ~86,875 rows).
However, there are populated equivalents:
PortCat is fully populated (could feed FHN Portfolio)
ExtRepCMLSubCategory (External) is populated, while the Internal one is empty (could feed FHN NAICS Industry)
Could you confirm whether we should switch to PortCat and ExtRepCMLSubCategory? If you'd prefer to keep InternalPortCat / IntRepCMLSubCategory, those fields will stay blank until that source data is populated.
CRO Name (point 1):
In the load query, the CRO Name is taken directly from the Load Samples input, with no fallback to the Relationship Manager name in that code path. So the Relationship Manager value you're seeing may be coming from a different place. Could you point me to a specific example (sample + customer) where a blank CRO loaded with the Relationship Manager name? That'll help me trace exactly where it's happening.
Once you confirm, I'll get these done quickly given the priority.
Thanks, Manikant
