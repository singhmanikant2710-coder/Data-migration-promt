Read-only. No edits. No plan. Just report with file paths + exact code. Do NOT modify or revert anyone's existing work (including any code authored by Jothi) — this is inspection only.

UAT #29: On Review Form → Customer Info → Relationship Overview, the "Relationship Manager" and "Portfolio Manager" dropdowns currently source their options from the CAS Users library. They must instead be sourced from dbo.[01_DATA_01_Data Mart Trial]:
- RM: options from DISTINCT [OfficerNumber] + [OfficerName] where both are NOT NULL. Label = "OfficerNumber - OfficerName", value = OfficerNumber. On save: Relationship_mgr_name = OfficerName, Relationship_mgr_number = OfficerNumber.
- PM: options from DISTINCT [PM Number] + [PMName] where both are NOT NULL. Label = "PM Number - PMName", value = PM Number. On save: Portfolio_mgr_name = PMName, Portfolio_mgr_number = PM Number.
Store columns already confirmed on dbo.[02_CORE_02_Reviews]: Relationship_mgr_name (nvarchar), Relationship_mgr_number (int), Portfolio_mgr_name (nvarchar), Portfolio_mgr_number (int).

Report the following, with file paths and exact code snippets:

1) The Customer Info section component that renders the RM and PM dropdowns. Show the JSX for both dropdowns and where their current options come from (the hook/service/state).

2) The current API endpoint + controller + repository that supplies the RM/PM options (the CAS Users source). Show the file paths and the exact SQL/EF query.

3) On save, how are the RM and PM values currently persisted? Trace the full path: the save DTO → the ReviewController postedSections guard → ReviewService.SaveAsync guard → the repository write. State clearly whether all FOUR columns (Relationship_mgr_name, Relationship_mgr_number, Portfolio_mgr_name, Portfolio_mgr_number) are already handled on save, or only the name columns. Show the exact code for these four columns.

4) Is there an existing LookupsController → IReportingService → IReportingRepository pattern (e.g. used for distribution-party-names, relationship segments/units/markets) that I can reuse to add two new lookups sourced from Data Mart Trial? Show that pattern (controller method, service method, repository method + SQL) for one existing lookup so it can be copied.

5) List exactly what must change, and in how many files, to:
   (a) add the two new Data Mart lookups — RM from [OfficerNumber]/[OfficerName], PM from [PM Number]/[PMName], both with a "both NOT NULL" filter and DISTINCT,
   (b) point the RM and PM dropdowns at these new lookups, with label "Number - Name" and option VALUE = the Number (do NOT parse the label string apart to get the number),
   (c) ensure BOTH name and number are saved for RM and PM (all four columns).

Use the LIVE DB, ignore columns.csv. Output findings only. Do not change any file. Do not alter existing logic authored by anyone.
