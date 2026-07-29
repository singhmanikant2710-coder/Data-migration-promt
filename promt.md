Three files. Do NOT modify or revert any existing logic authored by anyone (including Jothi) — only ADD. Use LIVE DB, ignore columns.csv. Do not plan. Just apply.

UAT #160: On Review History → Review List, add two columns — "Review ID" and "Customer Number" — placed between "SAMPLE / REVIEW NAME" and "BORROWER NAME / LINESHEET".

Important: the correct customer number is dbo.[02_CORE_02_Reviews].[Customer_number] (which has data). The existing eCIF_number column is currently NULL, so do NOT use it for the Customer Number column.

FILE 1 (backend): backend/src/Casrr.Infrastructure/SqlServer/SqlReviewHistoryRepository.cs
- ADD r.[Customer_number] to the SELECT list. Append it at the END of the SELECT (do not reorder existing columns), so existing reader indices stay valid.
- Read it DBNull-safe using a NAME-BASED ordinal lookup:
    var ordCustNum = rdr.GetOrdinal("Customer_number");
    var custNumber = rdr.IsDBNull(ordCustNum) ? null : Convert.ToString(rdr.GetValue(ordCustNum), us);
- ADD CustomerNumber = custNumber to the ReviewHistoryRow being constructed.
- ADD a matching property to the backend ReviewHistoryRow DTO/contract:
    public string? CustomerNumber { get; init; }
- Do not remove or change the existing eCIF_number selection/mapping.

FILE 2 (frontend types): frontend/src/services/api/reviewHistory.ts
- ADD one field to the ReviewHistoryRow type (do not change existing fields):
    customerNumber: string | null;

FILE 3 (frontend grid): frontend/src/app/review-history/page.tsx
- Insert TWO new column definitions immediately AFTER the "SAMPLE / REVIEW NAME" column and BEFORE the "BORROWER NAME / LINESHEET" column, matching the styling of the other simple text columns:
    {
      key: "reviewId",
      header: "REVIEW ID",
      sortable: true,
      cellClassName: "text-slate-600 text-sm tabular-nums",
      render: (r) => (r.reviewId ?? "-") as any,
    },
    {
      key: "customerNumber",
      header: "CUSTOMER NUMBER",
      sortable: true,
      cellClassName: "text-slate-600 text-sm tabular-nums",
      render: (r) => r.customerNumber ?? "-",
    },
- Do not change any other column, ordering, or the Borrower cell's PDF/navigation logic.

Run read-only TypeScript diagnostics on the changed frontend files only.
