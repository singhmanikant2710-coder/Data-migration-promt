Bug 218 — CRM Policy Exceptions report. Apply ALL fixes below in one pass. Do NOT break existing functionality. Match the confirmed house standard used by other CRM reports (Non-Compliant Covenants). Show all diffs, rebuild (dotnet build + npm run build), do NOT commit. Follow AGENTS.md.

REFERENCE: prototype "08_Policy Exceptions.pdf" (Image 1) shows the target layout. Current report (Image 2) shows the problems.

FRONTEND FILE: frontend/src/components/pdf/PolicyExceptionsPDF.tsx (the CRM Policy Exceptions report PDF component)
BACKEND (only if data is missing): SqlPolicyExceptionsReportRepository.cs + PolicyExceptions models — check first whether the needed data (per-exception-type totals, per-borrower commitment, an "identified/has-exception" flag) is already returned; extend only if missing.

FIXES:

1. HEADER top-right: currently shows the sample caption ("353 - 8/31/2026... - Professional CRE"). Replace with the DOWNLOAD DATE/TIME only, matching other CRM reports (mirror how NonCompliantCovenantsPDF / CrmSummaryTablePDF / ScorecardResultsPDF render today's date-time in the header-right — use that exact format). Remove the sample caption from the header.

2. FOOTER: use the house standard — "CRM Policy Exceptions • Page X of Y" (report name + page number), centered, NO logo. If the current footer has a First Horizon / CAS RiskReview logo image, remove it and its styles (mirror the footer already used in NonCompliantCovenantsPDF).

3. POLICY EXCEPTION TOTALS table: must show totals grouped BY policy exception description/type (like the prototype: "E44 - Minimum Post-Owner's Fixed Charge Coverage | COUNT | EXPOSURE", "E43 - Minimum Pre-Owner's Fixed Charge Coverage | ...", then a "Totals (N borrowers)" row). Currently it shows an empty/single blank row. Columns: EXCEPTION DESCRIPTION | COUNT | EXPOSURE. COUNT = distinct borrowers with that exception type; EXPOSURE = SUM(Commitment) for those. Bottom "Totals (N borrowers)" row = total distinct borrowers + total exposure. Exposure basis = SUM(Accounts.Commitment) (house standard — reuse the same commitment-loading approach used by NonCompliantCovenants / CrmSummaryTable; there is a shared caption helper SqlReportCaptionHelper you can follow the pattern of).

4. POLICY EXCEPTION DETAILS table: 
   a. Include ONLY borrowers that have one or more policy exceptions. Currently it lists rows with "No Policy Exceptions" / IDENTIFIED = N/A (e.g. AHC MCKINNEY RANCH, ARLINGTON ST AUGUSTINE, ARRIS GWINNETT, BISCAYNE SHORES) — those must be EXCLUDED. Keep only rows that represent an actual policy exception (like the prototype: CLEARPATH HOLDINGS, LENNY'S MIDCO, SOUTHERN BREW CORP).
   b. Include the customer COMMITMENT amount column (SUM Commitment per borrower/review). The prototype shows real commitment values ($11,806,735 etc.); currently it shows $0 — ensure commitment is populated from SUM(Accounts.Commitment).
   Columns (match prototype, left to right): CUSTOMER NAME (REVIEW ID) | IDENTIFIED | EXCEPTION DESCRIPTION | COMMITMENT.

5. FONT SIZE: the section headers "POLICY EXCEPTION TOTALS" and "POLICY EXCEPTION DETAILS" must be font size 11 (currently 10). Change only these two section headers to 11.

REQUIREMENTS:
- Use fixed pixel column widths (getPageWidthPts - margins), NOT flexBasis percentages (mirror ScorecardResults/NonCompliantCovenants).
- Add wrap={false} on each detail data row <View> so rows don't split across page boundaries (this report spans multiple pages — page 1 of 7).
- Use pageSetup tokens (colors, crmTypography), formatCurrency, formatDate, ensureHyphenationDisabled().
- Do NOT change the routing, other reports, or the backend beyond what's needed for the totals-by-type + commitment data.
- If the backend already returns per-type grouping and per-borrower commitment, do it all frontend-side; if not, extend the repository/DTO minimally (reuse LoadReviewCommitmentsAsync-style commitment loading and SqlReportCaptionHelper for the caption).

After applying, list every file + line changed, grouped by fix. Show diffs. Rebuild both. Do NOT commit.
