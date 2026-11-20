UAT #243 (revised) – New report "PD/LGD Grade Changes (Excel Only)"

REQUIREMENT (Geoff – replaces the earlier reconciliation design; do NOT build buckets/Data Mart comparison)
An Excel-only report listing every review/account where Bank PD ≠ CAS PD and/or Bank LGD ≠ CAS LGD. Geoff will add it to the report drop-down himself as "PD/LGD Grade Changes (Excel Only)". One row per account. Columns, in this order:
Reviews: Sample_id, Review_id, Unit, Market, CCL, Special_assets, Customer_number, Customer_name, Relationship_mgr_name, Portfolio_mgr_name
Accounts: Facility_number_calc, Account_number, Scorecard_id_bank, Bank_PD, CAS_PD, Bank_LGD, CAS_LGD, Balance, Commitment
Reviews: CRO_name, Review_finalized_date

PHASE 1 – READ-ONLY DISCOVERY (no edits)
- Confirm the exact column names/types for every field above (dbo.[02_CORE_02_Reviews], dbo.[02_CORE_04_Accounts]); flag any that don't exist (e.g. Balance vs Outstanding).
- Report framework: how a report name from 03_LIBRARY_09_Selections (Tab "Reporting", Section "Report Selections") maps to an id in reports/page.tsx (toReportId), and the substring routing guards – confirm "PD/LGD Grade Changes (Excel Only)" does not collide with any existing guard (e.g. "pd grade migration", "summary", "scorecard").
- Reuse: the #256 Excel builder (lib/reportExcelExport.ts / reportExcelSheets.ts, Filters sheet, reportFileName), and the standard ReportFilters + SqlReviewStatusHelper + #262 people filters, to copy from an existing simple report repository (name it).
- Dev data: read-only SQL counting accounts where PD differs, LGD differs, both, and rows where CAS_PD/CAS_LGD is NULL.

PHASE 2 – PLAN (smallest generic change)
- New backend endpoint/repository (same pattern as other reports, raw ADO.NET, parameterised): returns the rows above, applying the standard Reports Home filters (Sample, Review Status + Start/End incl. Approved, hierarchy, portfolio, Special Assets, CCL, CRO, CAS Manager, Approving Manager), excluding cancelled reviews.
- Difference rule: include an account when (CAS_PD IS NOT NULL AND CAS_PD <> Bank_PD) OR (CAS_LGD IS NOT NULL AND CAS_LGD <> Bank_LGD). Sort: Sample_id, Customer_name, Review_id, Account_number.
- Excel via the #256 builder: one sheet "PD-LGD Grade Changes" + Filters sheet; IDs/customer/account/facility/scorecard as text, PD/LGD as numbers, Balance/Commitment as currency, Finalized date as a real date, CCL/Special Assets as Yes/No; file name "PD-LGD Grade Changes.xlsx" (per reportFileName rule – "/" is not allowed in file names).
- "Excel Only": Export Report works; Generate PDF Report shows a clear info toast ("This report is available as Excel only – use Export Report") and does nothing else. No PDF component.
- Dev testing: idempotent DEV-ONLY scripts/sql/insert-pd-lgd-grade-changes-selection.sql (+ rollback), same pattern as the CAS KRI script, header "DEV ONLY – Geoff adds this report in QA/Prod". Do not touch other Selections.
- No change to other reports. No schema change.
- Do NOT decide silently – list as decisions with a recommendation:
  1. CAS PD/LGD NULL (reviewer never entered a CAS grade): exclude (recommended) or include?
  2. Highlight the differing PD/LGD cells (e.g. bold) – yes/no? (recommend: no extra formatting, keep it simple)
  3. Access: same as other reports (recommended) or Admin only?

STOP after Phase 2 and wait for my answers. Report in chat only.



Thanks Geoff – that works well. I'll build "PD/LGD Grade Changes (Excel Only)" with the fields you listed, one row per account where Bank and CAS PD and/or LGD differ, using the standard report filters. I'll let you know when it's ready for testing.
