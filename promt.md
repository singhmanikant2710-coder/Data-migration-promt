Column names and sample values confirmed:
- Segment: Corporate Segments, Regional Banking, Specialty Banking, UNKNOWN, Wholesale
- Region (Unit for Regional Banking): Mid-*, Other, South, Sout*, West
- SpecialtyLine (Unit for non-Regional): Corporate Banking, Corporate Correspondent, Energy, Equipment Finance, Franchise Finance, Healthcare, Mortgage Warehouse, Other
- Market: Acadiana, Acadiana Retail, AL Birmingham, etc.

Cascade logic confirmed. Plan approved. Proceed step-by-step, starting with step 1 (interface: add the 5 methods to ICustomerInfoLookupRepository). Pause after each file for my confirmation.

Reminder: use "SELECT DISTINCT TOP (n)" ordering in the SQL repository, NOT "SELECT TOP (n) DISTINCT".

For the units endpoint, match segment case-insensitively when checking for 'Regional Banking'.
