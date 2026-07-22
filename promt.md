Read-only. No edits. No plan. Just report with file paths + exact code. Do NOT modify anyone's work (including Jothi's).

Context (UAT #127, HIGH): The sample-load process (client refers to it as the "01_SAMPLE LOAD_01_Add Reviews" query) has three issues when a customer is loaded into a sample and a review row is created in dbo.[02_CORE_02_Reviews]:
1. CRO Name: when no CRO Name is provided in Load Samples, the review is incorrectly populated with the Relationship_mgr_name value. It should be left empty when not provided.
2. FHN Portfolio (Internal_portfolio) is not being populated on load. Client asked for the FIRST applicable [InternalPortCat] from dbo.[01_DATA_01_Data Mart Trial] — but that column is empty in the DB; a column [PortCat] is fully populated.
3. FHN NAICS Industry (NAICS_industry) is not being populated on load. Client asked for the FIRST applicable [IntRepCMLSubCategory] — but that column is empty; [ExtRepCMLSubCategory] is populated.

Report:
1) Find the backend sample-load logic that creates review rows when loading a sample (search for "Add Reviews", "SampleLoad", "LoadSample", "AddReviews", or any INSERT into dbo.[02_CORE_02_Reviews]). Give file paths (repository, service, controller).
2) Paste the exact INSERT statement / column mapping that creates a new review row. Specifically show how these target columns are populated (or if they are not populated at all):
   - the CRO name column (CRO_manager_name or similar)
   - Internal_portfolio (FHN Portfolio)
   - NAICS_industry (FHN NAICS Industry)
3) For issue 1: show exactly where the CRO name value comes from. Is it falling back to Relationship_mgr_name when the CRO input is empty/null? Paste that logic and the source of the CRO input.
4) For issues 2 & 3: does the sample-load query read from dbo.[01_DATA_01_Data Mart Trial] at all? If yes, show the join/lookup and which Data Mart columns it reads. Confirm whether Internal_portfolio and NAICS_industry are sourced from Data Mart during load, or left blank/defaulted.
5) State exactly what must change and in how many files to: (a) leave CRO name empty when not provided (remove the Relationship_mgr_name fallback), (b) populate Internal_portfolio from the first applicable Data Mart column, (c) populate NAICS_industry from the first applicable Data Mart column. Note the join key used to match a customer to their Data Mart Trial row.

Use LIVE DB, ignore columns.csv. Output findings only. Change nothing.
STRICT NOTICE: Do not break, remove, or alter any existing working functionality or business logic. Make only the minimum changes required for the requested task. Preserve all existing behavior, dependencies, validations, and integrations. Ensure no regression or impact on other screens, modules, or workflows.
