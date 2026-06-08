I am uploading two Excel files:
 
File 1 = Source System
File 2 = Target System
 
Perform a COMPLETE migration readiness assessment.
 
Do not limit analysis to column names only.
 
Analyze every table, column, datatype, relationship, naming convention, duplicate table, hidden field, legacy field, and business impact.
 
Provide findings in the following sections:
 
1. TABLE ANALYSIS
- Tables present in Source but missing in Target
- Tables present in Target but missing in Source
- Duplicate tables in Target representing same Source table
- Duplicate tables in Source representing same Target table
- Sheet/Table naming mismatches
- Plural vs singular naming differences
- Underscore vs space naming differences
- Potential table consolidation opportunities
- Potential migration risks caused by duplicate tables
 
2. COLUMN ANALYSIS
For every table provide:
 
| Source Table | Target Table | Source Column | Target Column | Status |
 
Status should be:
- Exact Match
- Naming Difference
- Data Type Difference
- Missing in Source
- Missing in Target
- Potential Mapping Candidate
- Requires Business Validation
 
3. DATA TYPE ANALYSIS
Identify:
- Data type mismatches
- Precision differences
- Integer vs Text conflicts
- Date vs DateTime conflicts
- Choice vs Text conflicts
- Boolean vs Text conflicts
 
Provide migration impact and recommendation.
 
4. DUPLICATE ENTITY ANALYSIS
Detect:
- Multiple target tables that appear to represent same source table
- Versioned tables (_ss, _org, _local, _backup, _copy, _history)
- Legacy entities
- Staging entities
- Archive entities
 
Explain migration impact.
 
5. LEGACY / HIDDEN FIELD ANALYSIS
Identify fields like:
- *_legacy
- *_hidden
- *_hist
- *_hldn
- audit columns
- migration columns
- staging columns
 
Explain:
- Why they exist
- Whether migration is required
- Risk if ignored
 
6. PRIMARY KEY ANALYSIS
Identify:
- Primary keys
- Candidate keys
- Missing keys
- Surrogate keys
- Auto-generated IDs
 
Highlight migration risks.
 
7. RELATIONSHIP ANALYSIS
Identify:
- Lookup relationships
- Parent-child relationships
- Foreign keys
- Junction tables
- Missing relationship mappings
 
8. DATA QUALITY RISKS
Identify:
- Null risks
- Duplicate risks
- Orphan record risks
- Referential integrity risks
- Missing mandatory fields
 
9. MIGRATION COMPLEXITY REPORT
 
For each table classify:
 
- Low Complexity
- Medium Complexity
- High Complexity
- Critical Risk
 
Provide reason.
 
10. FINAL EXECUTIVE SUMMARY
 
Generate:
 
A. Missing Tables Summary
B. Missing Columns Summary
C. Duplicate Tables Summary
D. Hidden/Legacy Fields Summary
E. Data Type Mismatch Summary
F. Migration Risks Summary
G. Recommended Mapping Strategy
 
Highlight every issue, even minor naming differences.
 
Assume this analysis will be used by Solution Architects and Data Migration Engineers before production migration.
 
Be extremely strict and detailed.
