Sample 354 has review 21861 (ARMSTRONG RELOCATION) with 2 covenants — status Not-Compliant and Past Due, category "Financial Indicators". The DB predicate matches them (confirmed via SQL). But the report's covenants array is still empty for this sample. Find why the covenants don't reach the report. READ-ONLY, no edits. Answer, STOP.

1. In SqlNonCompliantCovenantsReportRepository, there are two queries: the envelope (which reviews) and the covenant-load (PopulateNonCompliantCovenantsAsync). 
   a. Does the envelope query actually INCLUDE review 21861 for sample 354? What determines which reviews are in the envelope — is there a ReviewStatusPredicate or status/date filter that might EXCLUDE review 21861 (e.g. requires Completed status or a date window)? Paste the envelope WHERE clause.
   b. Does PopulateNonCompliantCovenantsAsync (the covenant query with the normalized status predicate) run for review 21861's id? Paste its exact WHERE clause as executed.

2. CATEGORY handling: the covenants have Covenant_category = "Financial Indicators". 
   a. Does the covenant-load query or the C# aggregation filter/normalize by category? 
   b. In BuildReportSections, how are covenants split into Monitoring vs Performance totals? Does it recognize "Financial Indicators" as a category, or does it only handle "Monitoring" / "Financial Performance" and silently drop "Financial Indicators"?
   c. Does the DETAILS table include ALL matched covenants regardless of category, or only those in known categories?

3. Trace for review 21861 specifically: envelope includes it? → covenant query returns its 2 rows? → they land in Details? → they're counted in Monitoring/Performance totals? Identify the exact step where the 2 covenants get dropped.

Report the exact WHERE clauses and the category-handling logic. Do NOT fix yet.
