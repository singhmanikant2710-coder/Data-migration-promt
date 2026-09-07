Bug 220 — fix category bucketing. DB has 6 covenant categories (confirmed via SQL): Financial Performance, Monitoring, Financial Statement Requirements, Financial Indicators, Default Covenants, No Covenants. IsPerformanceCategory currently only matches "Financial Performance", so Financial Indicators + Default Covenants wrongly go to Monitoring.

Mirror the mapping from frontend useCovenants.ts:
- PERFORMANCE: "Financial Performance", "Financial Indicators", "Default Covenants"
- MONITORING: "Monitoring", "Financial Statement Requirements"

FILE: backend/src/Casrr.Infrastructure/SqlServer/SqlNonCompliantCovenantsReportRepository.cs, IsPerformanceCategory (~line 215): return true (case-insensitive, trimmed) for "Financial Performance", "Financial Indicators", "Default Covenants". Show diff. Rebuild. Do NOT commit.

Hi Geoff, quick note on Non-Compliant Covenants — the report is rebuilt and working. When verifying, I found that neither the dev nor test database currently contains any completed reviews with actual Non-Compliant/Past Due covenants — sample 357 and others only have "No Covenants" placeholder entries (status not set). The COTTI FOODS / SOUTHERN BREW data in your prototype appears to be from the older MS Access system and wasn't migrated to the current SQL database. I've verified the report renders correctly by setting up a test covenant. Once real non-compliant covenant data exists in the system, the report will populate as designed. Just flagging so you know the empty result isn't a bug — it reflects the current data.
