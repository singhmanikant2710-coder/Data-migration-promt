Read-only. No edits. No plan. Just report with file paths + exact code. Do NOT modify anyone's work (including Jothi's).

Context (UAT #128): On Review Form → Customer Info → Customer Profile, the "FHN NAICS Industry" dropdown must be populated from a DISTINCT select of dbo.[01_DATA_01_Data Mart Trial].[IntRepCMLSubCategory]. Currently it is not connected to any source. (Note: this column is currently empty in the DB, but the code should still be wired so it populates once data is added.)

A very similar lookup was recently added for RM/PM (UAT #29) using the LookupsController -> ReportingService -> IReportingRepository pattern, sourcing from Data Mart Trial. And FHN Portfolio Segment / relationship segments use the same lookup pattern.

Report:
1) In CustomerInfoSection.tsx: the "FHN NAICS Industry" SelectField JSX. What options does it currently use (a hardcoded list, an empty array, or a lookup)? Paste it. What is the field name / section for its value (e.g. naicsIndustry)?
2) Show one existing Data Mart lookup end-to-end as the template to copy: the LookupsController endpoint, the ReportingService method, and the SqlReportingRepository method + SQL (e.g. the relationship segments lookup, or the RM lookup from #29).
3) Confirm the SQL needed: SELECT DISTINCT LTRIM(RTRIM([IntRepCMLSubCategory])) FROM dbo.[01_DATA_01_Data Mart Trial] WHERE [IntRepCMLSubCategory] IS NOT NULL AND LTRIM(RTRIM([IntRepCMLSubCategory])) <> '' ORDER BY 1 — is this the right table/column?
4) Does the save path for FHN NAICS Industry already work (i.e. selecting a value saves to NAICS_industry)? The diagnostics from an earlier ticket showed ReviewService handles naicsIndustry -> NAICS_industry. Confirm it still saves.
5) State exactly what must change and in how many files to: add a new "FHN NAICS Industry" Data Mart lookup and point the dropdown at it, matching the RM/PM (#29) pattern.

Use LIVE DB, ignore columns.csv. Output findings only. Change nothing.
