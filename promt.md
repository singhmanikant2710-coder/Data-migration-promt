We need implement a genuine, generic fiscal-year calculation fix for BCAT.

IMPORTANT:
Do NOT hardcode customer names or fiscal start months in application code.

SOURCE OF TRUTH:
The customer's fiscal-year start month is stored in:

tblCustomer.intFiscalYearMonthStart

The application must dynamically read this value for the selected customer.

We have approximately 131 customers and different fiscal-year start months:
1, 2, 4, 5, 6, 7, 9, 10, 11, etc.

The fiscal-year calculation MUST be generic and customer-specific.

CORE RULE:

Given:
- strMonthKey = YYYYMM
- intFiscalYearMonthStart = customer's fiscal start month

Calculate calendar year:
    calendarYear = first 4 digits of strMonthKey

Calculate calendar month:
    calendarMonth = last 2 digits of strMonthKey

Fiscal Year:

IF calendarMonth >= intFiscalYearMonthStart
    fiscalYear = calendarYear + 1
ELSE
    fiscalYear = calendarYear

Fiscal Month:

IF calendarMonth >= intFiscalYearMonthStart
    fiscalMonth = calendarMonth - intFiscalYearMonthStart + 1
ELSE
    fiscalMonth = calendarMonth + 12 - intFiscalYearMonthStart + 1

EXAMPLES:

For ADIR INTERNATIONAL LLC:
intFiscalYearMonthStart = 2

202401 -> FY2024 / Month 12
202402 -> FY2025 / Month 1
202403 -> FY2025 / Month 2
202412 -> FY2025 / Month 11
202501 -> FY2025 / Month 12
202502 -> FY2026 / Month 1
202503 -> FY2026 / Month 2

For WORLD ACCEPTANCE CORPORATION:
intFiscalYearMonthStart = 4

202603 -> FY2026 / Month 12
202604 -> FY2027 / Month 1
202605 -> FY2027 / Month 2
202606 -> FY2027 / Month 3
202607 -> FY2027 / Month 4

CRITICAL REQUIREMENT:

Do not trust tblMain.intFiscalYear and tblMain.intFiscalMonth blindly when determining the fiscal year/month for the UI.

Those fields may contain legacy/stale/wrong values.

The fiscal year/month displayed by BCAT must be derived consistently from:

tblMain.strMonthKey
+
tblCustomer.intFiscalYearMonthStart

unless there is an explicitly documented reason why the stored fields must be used.

TRACE THE ENTIRE FLOW:

1. Customer selection
2. MonthKey selection
3. tblCustomer lookup
4. intFiscalYearMonthStart retrieval
5. Fiscal year calculation
6. Fiscal month calculation
7. FY dropdown
8. Month dropdown
9. "Fiscal: YYYY/M" display
10. Add New Month
11. Database insert
12. Page reload
13. Selected FY/month after reload

ADD NEW MONTH REQUIREMENT:

The new month must ALWAYS be generated from the customer's latest existing MonthKey.

It must NOT be generated from the FY currently selected by the user.

Example:

Latest existing MonthKey:
202606

Next MonthKey:
202607

Then calculate the fiscal year/month of 202607 using the customer's
intFiscalYearMonthStart.

For World Acceptance:
fiscal start = 4

202607 -> FY2027 / Fiscal Month 4

Therefore, even if the user has selected FY2026 in the UI, clicking Add New Month must create:

202607
FY2027
Fiscal Month 4

It must NOT create a month under FY2026.

IMPORTANT:
The selected FY is UI navigation/filter state only. It must not control the fiscal year assigned to a newly created month.

AFTER ADDING:
Reload the page and select/show the newly created latest MonthKey and its calculated fiscal year.

DO NOT:
- hardcode ADIR
- hardcode World Acceptance
- hardcode any customer
- hardcode fiscal start months
- hardcode FY2026/FY2027
- manually update database records
- change legacy database data
- change unrelated UI
- add/remove UI rows
- duplicate months
- refactor unrelated code

FIRST INVESTIGATE, THEN FIX.

Before changing code, identify:

1. Where fiscal year/month is currently calculated.
2. Where tblMain.intFiscalYear/intFiscalMonth is being used.
3. Where tblCustomer.intFiscalYearMonthStart is retrieved.
4. Why ADIR 202503 currently displays FY2025/3 instead of FY2026/2.
5. Why Add New Month can use the selected FY instead of the latest MonthKey.
6. Which exact functions/files need to change.

Then implement the smallest generic fix.

VALIDATION MUST INCLUDE:

ADIR (start month 2):
202402 = FY2025/1
202501 = FY2025/12
202502 = FY2026/1
202503 = FY2026/2

WORLD ACCEPTANCE (start month 4):
202603 = FY2026/12
202604 = FY2027/1
202606 = FY2027/3
202607 = FY2027/4

Also test customers with start months:
1, 5, 7, 9, 10, and 11.

The same generic formula must work for every customer.

Finally provide:
- root cause
- exact files/functions changed
- exact diff
- validation results
- confirmation that no customer-specific hardcoding was introduced.

below is the customer the fiscal start year
1ST FRANKLIN FINANCIAL CORPORATION = 1
ADIR INTERNATIONAL LLC = 2
ALAN WIRE COMPANY = 5
AMERICAN CREDIT ACCEPTANCE = 1
ASBURY MANAGEMENT GROUP, INC = 7
ATHENS PAPER COMPANY INC = 10
ATLAS CREDIT COMPANY INC = 1
ATLAS FUNDING SPV I LLC = 1
AXIS RENTAL SOLUTIONS, LLC = 1
B WI COMPANIES INC = 11
BANKERS HEALTHCARE GROUP LLC = 10
BASIC FINANCE INC = 1
BELMONT FINANCE LLC = 1
BETTERBUILT STORAGE LLC = 1
BLOOMFIELD CAPITAL INCOME FUND V. = 1
BLUFF ROAD AUTO SALES, LLC = 1
BMI FINANCE INC = 1
BPI PACKAGING LLC = 1
CAPITALCREDIT, LLC = 1
CAR FINANCIAL SERVICES INC = 1
CARS FINANCIAL INC = 1
CENTRAL NETWORK RETAIL GROUP, LLC = 1
CENTURY FINANCE LLC = 1
CIT NORTHBRIDGE FUNDING I LLC = 1
CITIZENS SAVINGS & LOAN CORPORATION = 1
CLASSIC AMERICAN HARDWOODS INC = 1
COLONIAL LOAN ASSOCIATION INC = 1
COMPASS EQUIPMENT FINANCE LLC = 1
CREDIT ACCEPTANCE CORPORATION = 1
CREST OPERATIONS LLC = 1
CROWN ENTERPRISES INC = 1
CSC LEASING CO = 1
DAVID'S AUTO SALES INC = 1
DEEPWELL ENERGY SERVICES LLC = 1
DRIVE NOW ACCEPTANCE LLC = 1
DUFF REAL ESTATE LLC = 7
DWIGHT FUNDING LLC = 1
EAGLE FINANCIAL SERVICES INC = 1
EASY AUTO CREDIT INC = 1
ECLIPSE BUSINESS CAPITAL SPV LLC = 1
ENOVA INTERNATIONAL, INC. = 1
ERGON INC = 7
FGI WORLDWIDE LLC = 1
FIRST COAST SECURITY SERVICES INC = 1
FIRST FINANCIAL CREDIT INC = 1
FIRST TOWER LOAN LLC = 1
FREIGHT FACTORING SPECIALISTS LLC = 1
FREZ-N-STOR INC = 9
GATEWAY COMMERCIAL FINANCE LLC = 1
GATEWAY FINANCIAL SERVICES INC = 1
GLOBAL PHONE SOLUTIONS LLC = 1
GLS LEASCO INC = 1
GRACELAND PROPERTIES = 1
GRACELAND RENTALS LLC = 1
GREEN PLAINS FINANCE COMPANY LLC = 1
HANKINS INC = 10
HCG CONSUMER CREDIT II TRUST = 1
HCG SME II TRUST = 1
INTERNATIONAL FINANCIAL SERVICES = 7
ITHRIVE FUNDING, LLC = 1
J.L.O. VENTURES LLC = 1
KEYSTONE PRIVATE INCOME FUND = 10
KEYSTONE REAL ESTATE LENDING FUND = 1
LEGACY CORPORATE LENDING ASSET = 1
LEGACY NW FUND LLC = 1
LEGACY OPPORTUNITY FUND, LLC = 1
LEON'S AUTO SALES, INC. = 1
LIBERTY BANKER LIFE INSURANCE = 1
LOBEL FINANCIAL CORPORATION = 1
LOMANCO = 1
MAMMOTH MEDICAL INC = 1
MARINE ONE ACCEPTANCE CORPORATION = 1
MARINER FINANCE LLC = 1
MARTIN INCORPORATED = 1
MAVERICK USA INC = 1
MDR CONSTRUCTION INC = 1
MEMPHIS INTERNATIONAL TRADING CO = 1
NATIONAL EQUIPMENT DEALERS LLC = 1
NATIONWIDE SPECIALTY FINANCE INC = 6
NEWFOUND II LLC = 1
NEWPARK RESOURCES INC = 1
OAK TREE FINANCE, LLC = 1
OKLAHOMA STEEL & WIRE CO INC = 2
OUTDOOR CAR CO INC = 1
P A M TRANSPORT INC = 1
PACE FINANCIAL LLC = 1
PASCHALL TRUCK LINES INC = 1
PINC = 9
PORTER CAPITAL CORPORATION = 1
PREFERRED CREDIT INC = 1
PRIMAL CAPITAL PARTNER LP = 1
PROGRESSIVE FINANCE HOLDINGS, LLC = 1
QUALITY LEASING CO., INC. = 1
RAINEY FINANCE, INC = 1
REGIONAL MANAGEMENT CORP = 1
ROSENTHAL & ROSENTHAL = 1
RYAN TRANSPORTATION SERVICES = 1
SAC FINANCE INC = 1
SAVVY GROUP LLC = 1
SDA INC = 1
SECURITY GROUP INC = 1
SERVICE PAYMENT PLAN INC = 1
SHABANA MOTORS LLC = 1
SIUPREM INC = 10
SKYWORKS, LLC = 1
SNAP FINANCE LLC = 1
SOUTHERN TIRE MART LLC = 7
STANDARD PREMIUM FINANCE = 1
STRATEGIC TRANSPORTATION RESOURCES = 1
SUN LOAN FUNDING LTD = 1
SUNBELT FINANCE CO LLC = 1
SUNRISE ACCEPTANCE INC = 1
SUNSET MANAGEMENT INC = 1
SUPERIOR FINANCIAL SERVICES INC = 1
SWK HOLDING = 1
THE AUTO CONNECTION, INC. = 1
THE HAWTHORN GROUP = 1
THE INDEPENDENT SAVINGS PLAN COMPANY = 1
THUNDER CARRIER SERVICES LLC = 1
TIMEPAYMENT CORP = 1
TNT CRANE & RIGGING, INC. = 1
TOWER CAP SPV, LLC = 1
TRANS LEASE INC = 1
UNIFOUR FINANCIAL SERVICES, LLC = 1
UNIVERSAL MANAGEMENT SERVICES = 1
UPBOUND GROUP, INC. DBA = 1
VACUUM TRUCK RENTALS LLC = 1
VAN ZYVERDEN INC = 7
VERMEER MOUNTAIN WEST INC = 10
WESTLAKE SERVICES LLC = 1
WORLD ACCEPTANCE CORPORATION = 4


