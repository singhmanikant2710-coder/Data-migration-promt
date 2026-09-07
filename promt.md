Bug 220 — fix category bucketing. DB has 6 covenant categories (confirmed via SQL): Financial Performance, Monitoring, Financial Statement Requirements, Financial Indicators, Default Covenants, No Covenants. IsPerformanceCategory currently only matches "Financial Performance", so Financial Indicators + Default Covenants wrongly go to Monitoring.

Mirror the mapping from frontend useCovenants.ts:
- PERFORMANCE: "Financial Performance", "Financial Indicators", "Default Covenants"
- MONITORING: "Monitoring", "Financial Statement Requirements"

FILE: backend/src/Casrr.Infrastructure/SqlServer/SqlNonCompliantCovenantsReportRepository.cs, IsPerformanceCategory (~line 215): return true (case-insensitive, trimmed) for "Financial Performance", "Financial Indicators", "Default Covenants". Show diff. Rebuild. Do NOT commit.
