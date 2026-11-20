#243 – Implemented (simplified approach, as you requested). Tested in Dev and pushed to the develop branch. Not yet deployed to QA.

What changed:
- New Excel-only report "PD/LGD Grade Changes (Excel Only)" listing every account where the Bank PD differs from the CAS PD and/or the Bank LGD differs from the CAS LGD (one row per account). Accounts without a CAS grade are not listed.
- Columns, in the order you listed: Sample ID, Review ID, Unit, Market, CCL, Special Assets, Customer Number, Customer Name, Relationship Manager, Portfolio Manager, Facility Number, Account Number, Scorecard ID, Bank PD, CAS PD, Bank LGD, CAS LGD, Balance, Commitment, Credit Review Officer, Finalized Date. A Filters sheet lists the filters applied.
- All standard report filters apply (sample, review status incl. Approved, dates, hierarchy, portfolio, Special Assets, CCL, Credit Review Officer, CAS Manager, Approving Manager). Cancelled reviews are excluded.
- "Generate PDF Report" shows a message that this report is Excel only; use "Export Report".

Action needed in QA/Test and Production: please add the report to the drop-down in the Selections Library (Tab "Reporting", Section "Report Selections") with the exact name "PD/LGD Grade Changes (Excel Only)".

Note: in Dev, Facility Number (CORE_Accounts.Facility_number_calc) is blank for all accounts in this report, so that column is empty – the value is not populated in the data.

Tested in Dev: 281 accounts with no filters and 273 with Review Status = Finalized; both matched a direct database count.

I will confirm here once deployed to QA.


git checkout -- frontend/next-env.d.ts frontend/tsconfig.tsbuildinfo
git add -u backend frontend
git add backend/src/Casrr.Application/Reporting/PdLgdGradeChanges backend/src/Casrr.Infrastructure/SqlServer/SqlPdLgdGradeChangesReportRepository.cs scripts/sql/insert-pd-lgd-grade-changes-selection.sql scripts/sql/rollback-insert-pd-lgd-grade-changes-selection.sql
git status
git commit -m "UAT #243: new PD/LGD Grade Changes (Excel Only) report – accounts where Bank and CAS PD/LGD differ, standard report filters; Dev-only selection seed script"
git push origin develop
