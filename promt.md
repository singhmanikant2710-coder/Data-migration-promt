Hi Geoffrey, I've checked both environments and the fields are still empty in each — the update doesn't appear to have landed anywhere yet.
I queried the Data Mart Trial table in both the Dev and the QA/Test databases. In both, InternalPortCat and IntRepCMLSubCategory show 0 populated rows.
In total, these 14 columns are still completely empty in both environments (TermAmort included, which you noted should stay NULL):
AccountType, CIConcentration, Conversion, CorporateRegionalBanking, CorporateSpecialty, Days Past Due, DaysPastDueType, DelinquentID, InterestType, InternalPortCat, IntRepCMLSubCategory, LimitGroup2, PortfolioLimitCategory, TermAmort
Could you check with John which environment he applied the update to? It's possible it went to a different database than the one you're testing against.
Once the data is in, both #127 (FHN Portfolio / NAICS on sample load) and #128 (NAICS Industry dropdown) should work with no code change — I've already verified the wiring is correct on both, including testing #128 with a temporary value to confirm the dropdown populates.
Thanks, Manikant
