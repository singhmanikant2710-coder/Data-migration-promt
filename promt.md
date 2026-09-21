READ-ONLY — do not edit any files. I need a diagnosis, not a fix.

Investigate two BCAT bugs for Athens Paper Company Inc (composite key: MonthKey=202510, CustomerName="ATHENS PAPER COMPANY INC"):

Bug 1 — Edit save failure:
Editing Min Tangible Net Worth on an EXISTING month (202510) throws:
"Violation of PRIMARY KEY constraint 'tblMain$PrimaryKey'. Cannot insert duplicate key in object 'dbo.tblMain'. The duplicate key value is (202510, ATHENS PAPER COMPANY INC)."

Trace the full call path for the "Black Book Edit" save/refresh action:
- Which controller/endpoint handles the save
- Which application service/command handler it calls
- How (or whether) it loads the existing tblMain entity before saving
- Whether the entity's EF Core tracking state is Added or Modified before SaveChangesAsync
- Whether this save path is shared with "Add New Month," and where they diverge

Bug 2 — Missing fiscal months / bad downstream calcs:
For Athens Paper (FY end 9/30), months 202601, 202602, 202603 never got created — the sequence jumps from 202512 to 202604. This corrupts dblAccountsReceivableTurnDays, curInventoryTurn, perCollateralAvailability and other ratios that depend on intElapsedFiscalDays (= intFiscalMonth * 30).

Trace:
- Where "Add New Month" determines the next MonthKey and intFiscalMonth
- Whether intFiscalMonth is derived from true calendar offset from the customer's FY start, or from the position/count of existing rows
- Whether this is the same code path as the known "Add New Month fiscal year" bug affecting all nine fiscal start months

Report back file paths, method names, and your best-supported root-cause hypothesis for each. Do not propose or write a fix yet.
