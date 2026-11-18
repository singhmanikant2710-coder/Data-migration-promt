UAT #237 – Review Queue: Excel Export button

REQUIREMENT (from Geoff, Issue 237 tab)
1. Move the 'Home' button on the Review Queue screen to the left.
2. Add an 'Excel Export' button where Home currently sits.
3. Export all current Review Queue fields/logic plus the new ones, preferably in this order:
   - Reviews.Sample_type (new)
   - Reviews.Review_type (new)
   - Sample Name
   - Review ID
   - Customer_number
   - Customer_name
   - Reviews.Unit (new)
   - Reviews.Portfolio_sub_segment (new)
   - CRO_name
   - Status (Review Queue logic)
   - SUM(Accounts.Commitment)
   - MAX(Accounts.Bank_PD)
   - MAX(Accounts.CAS_PD)
   - Completed_date
   - Review_approval_date
   - Review_distributed_date
   - Review_finalized_date
   - Reviews.Cancelled_date (new)
   - Reviews.Cancelled_reason (new)
Purpose: offline reconciliation against external Sample files and progress monitoring.

PHASE 1 – READ-ONLY DISCOVERY (no edits)
- Review Queue end to end with file:line: page (frontend/src/app/review-queue/page.tsx), service, API route/controller, repository SQL (SqlReviewRepository.cs ~3425 area), DTO.
- How each current column is computed: Status logic, Exposure (is it SUM of Accounts.Commitment or something else?), Bank PD / CAS PD, the date columns (note #250: Completed = Completed_date only).
- Filters that shape the visible rows: My View (My View/My Team/My Department), Sample Name, the status tiles (Borrowers Sampled, Unopened/Cancelled, In Progress, Draft Completed, Approved, Distributed, Finalized), the "Type to filter grid" box, sorting, paging. Which are applied server-side vs client-side?
- Confirm the exact DB column names for the new fields on dbo.[02_CORE_02_Reviews] (Sample_type, Review_type, Unit, Portfolio_sub_segment, Cancelled_date, Cancelled_reason) and where Sample Name comes from. Report any that do not exist.
- Existing export code in the app (Core Export ExportsController.cs, any XLSX writer, lib/xlsxCell.ts, any xlsx/exceljs package already installed) – reuse, do not add a new package without asking.
- Existing file-name convention for downloads (Proper Case, no hyphens, as used for report PDFs).
- Who can see the Review Queue and which rows (role/view filtering) – the export must never return more rows than the user can see on screen.

PHASE 2 – PLAN (smallest generic change)
- Reuse the SAME query/logic as the queue (no second copy of the Status/Exposure/PD logic). Add the new fields to it only if needed, without changing what the screen shows.
- Export ALL rows matching the current view/filters, not just the current page. Columns in the listed order with readable headers.
- Real dates as Excel dates (MM/DD/YYYY), numbers as numbers (Commitment as currency, PDs as numbers), Customer_number as text so leading zeros are kept, NULL as empty cell.
- Excel formula-injection protection: any text cell starting with = + - @ (or tab/CR) must be written as plain text/escaped.
- File name: "Review Queue Export MM DD YYYY.xlsx" style, following the existing convention (propose it).
- Home button moved left; Excel Export button in its place, same style as existing toolbar buttons; disabled + spinner while exporting; clear error toast on failure (no stack traces).
- Server-side authorization identical to the queue endpoint.
- Do NOT decide silently – list options with a recommendation:
  1. Export the rows of the current view/filters (tile, My View, Sample, text filter) or always all rows the user can see?
  2. Generate the file server-side (.NET) or client-side?
  3. Large row counts: any limit needed?
  4. Should the "Status" text match the screen exactly (including Cancelled) – confirm.
- No schema change. SQL only as idempotent scripts/sql/ if truly needed – ask first.

PHASE 3 – IMPLEMENT (only after I answer the Phase 2 decisions)
- Touch only the needed code. Review Queue screen columns, sorting, paging and tiles must stay exactly as today.

PHASE 4 – VERIFY + REPORT (in chat, not in a file)
- ADDED (file:line), REMOVED/CHANGED (file:line).
- On-screen before/after: button positions; export with default view; with a tile selected; with a Sample selected; with the text filter; My View vs My Team vs My Department.
- Confirm row count in the file = row count on screen (all pages), and give a read-only SQL that returns the same rows/values for me to compare.
- NULL/edge cases: review with no Accounts rows (Commitment/PD empty, not 0 unless the screen shows 0); NULL dates; cancelled reviews with/without reason; Customer_number with leading zeros; text starting with = + - @; special characters in customer names; 0 rows (export gives headers only or a message – say which).
- Confirm other screens, Core Export and the queue itself are unaffected.
- dotnet build, npx tsc --noEmit, npm run build results.
- git status (restore next-env.d.ts / tsconfig.tsbuildinfo if rewritten; leave pre-existing SQL scripts and devin-output/ alone).
- Do not commit or push.
