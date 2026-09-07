Bug 220 — REBUILD the Non-Compliant Covenants report to match the confirmed spec. This is a full rebuild: routing fix + backend data changes + PDF component rewrite. Geoff has confirmed all decisions. Show all diffs before applying, in logical groups. Do NOT commit. Follow AGENTS.md.

CONFIRMED SPEC (from Geoff):
- Report name / banner title: "Non-Compliant Covenants" (NOT "Covenant Violations" — no separate report; this is just the existing Non-Compliant Covenants report rebuilt).
- Footer: HOUSE STANDARD — "Non-Compliant Covenants • Page X of Y", centered, NO logo (match CrmSummaryTablePDF / ScorecardResultsPDF footer). Remove the wide FHB logo currently used.
- Exposure basis: SUM(Accounts.Commitment) per Review_id (house standard; matches prototype).
- Include statuses: 'Not Compliant' AND 'Past Due' (both in summary totals and detail table). Widen the current strict 'NON-COMPLIANT'-only predicate accordingly.
- Layout (single continuous flowing document, NOT one page per borrower):
  1. Header banner: title "Non-Compliant Covenants" left, sample caption right (format "357 - 5/1/2026 - Examination - Franchise Finance").
  2. "Total Borrowers with Covenant Violations" = distinct borrower count.
  3. "Total Exposure with Covenant Violations" = total Commitment $.
  4. "Monitoring Covenant Violation Totals" table: COVENANT TYPE | COUNT | EXPOSURE, with a "Totals (N borrowers)" row. COUNT = distinct borrowers; EXPOSURE = SUM(Commitment).
  5. "Performance Covenant Violation Totals" table: same columns + totals row.
  6. "Covenant Violation Details" table with these EXACT columns (Geoff-confirmed, left to right):
     - CUSTOMER NAME (REVIEW ID)  ← Customer_name + Review_id
     - STATUS                      ← Covenant_last_eval_status
     - CATEGORY                    ← Covenant_category
     - COVENANT TYPE               ← Covenant_type
     - THRESHOLD                   ← Covenant_threshold (NULL/blank for Monitoring covenants)
     - EVAL DATE                   ← Covenant_last_eval_date
     - RESULT                      ← Covenant_financial_result (NULL/blank for Monitoring covenants)
     - COMMITMENT                  ← SUM(Commitment)
  7. Final page: "APPLIED REPORT FILTERS" block.

CHANGES:

A. ROUTING FIX (frontend/src/app/reports/page.tsx):
- The non-compliant-covenants branch is currently dead because s.includes("covenants") (the covenants-summary test, ~L163) fires first for "Non-Compliant Covenants". Reorder/tighten so "Non-Compliant Covenants" routes to the non-compliant-covenants id, and "Covenants Summary" still routes correctly. Also fix the isCovenantsSummary fallback (~L450-456) similarly. Verify against the live catalog names.

B. BACKEND — extend the Non-Compliant Covenants report data to produce the sections above:
- Repository (SqlNonCompliantCovenantsReportRepository.cs): 
  * Join 02_CORE_04_Accounts to compute SUM(Commitment) per Review_id.
  * Widen the covenant predicate to include 'Not Compliant' and 'Past Due' (not just NON-COMPLIANT).
  * Return: distinct borrower count, total exposure (SUM Commitment), monitoring vs performance grouping by covenant type (distinct-borrower COUNT + SUM Commitment per type, plus totals), and per-violation detail rows with all 8 columns above.
  * Sample caption: reuse the ResolveCaptionAsync logic from SqlCrmSummaryTableReportRepository.cs (extract to a shared helper, e.g. SqlReportCaptionHelper, and have both call it — do not copy-paste).
- DTO (NonCompliantCovenantsModels.cs): add TotalBorrowers, TotalExposure, ReportingCaption, the monitoring/performance total rows (CovenantType, Count, Exposure), and extend detail rows with Status, Category, CovenantType, Threshold, EvalDate, FinancialResult, Commitment.
- Keep the API route /non-compliant-covenants/execute stable.

C. FRONTEND PDF (NonCompliantCovenantsPDF.tsx) — rewrite the render tree to the layout above:
- Header: title "Non-Compliant Covenants" + reportingCaption (replace the generatedOn timestamp).
- Remove per-borrower page loop; render one continuous document.
- Add the two headline lines, the two totals tables, the details table (8 columns), and the APPLIED REPORT FILTERS page.
- Footer: house standard "<Name> • Page X of Y", centered, no logo (mirror CrmSummaryTablePDF/ScorecardResultsPDF). Remove FHB logo image + footerCenter/Left/Right styles.
- Use FIXED pixel column widths computed from getPageWidthPts(PAGE_ORIENTATION) - margins (like ScorecardResultsPDF ~L243-261) — NOT flexBasis percentages.
- Use pageSetup tokens (colors, crmTypography), formatCurrency, formatDate, ensureHyphenationDisabled().
- THRESHOLD and RESULT blank for Monitoring covenants.

D. SERVICE/CALLER:
- reporting.ts: give Covenant... / NonCompliantCovenantsResponse a real typed interface matching the new DTO (replace `any`).
- reports/page.tsx caller: pass the new data shape (data + meta) to the PDF, mirroring how other CRM report PDFs are invoked.

Do NOT touch Covenant Violations / its feature flags (Geoff doesn't want that report). Do NOT change other reports' exposure basis. 
Report every file + line changed, grouped A/B/C/D. Show diffs. Do NOT commit.
