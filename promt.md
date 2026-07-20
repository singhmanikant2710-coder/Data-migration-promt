Backend only. Add the Customer Background narrative to the Initial Memo and Final Memo API responses. Use LIVE DB, ignore columns.csv. Single file per edit. Do NOT modify or revert any existing logic authored by anyone (including Jothi) — only ADD. Every existing field must keep behaving exactly as it does.

Context: The Review Form's Customer Background comments are stored in dbo.[02_CORE_02_Reviews].[Borrower_information] (NVARCHAR, holds HTML). The CAS Linesheet already renders it. The memo endpoints do not return it at all.

1) Locate the Initial Memo and Final Memo backend flow: the controller endpoints, the service, and the repository method(s) that build the memo response (search for "InitialMemo" / "FinalMemo" / the endpoints the frontend calls via frontend/src/services/api/reporting.ts). Report the file paths first.

2) In the memo response contract(s) — the DTO(s) that serialise to what the frontend receives as InitialMemoResponse / FinalMemoResponse — ADD one nullable property that serialises as:
     customerBackground
   Do not rename, remove, or reorder any existing property.

3) In the repository query that builds the memo data, ADD [Borrower_information] to the SELECT list and read it DBNull-safe into the new property. Do not change any existing column or WHERE clause.

4) In the service/mapping layer, map it through to the response DTO in the same place the other narrative fields (riskRatingJustification, scorecardNarrative) are mapped.

Return the raw HTML exactly as stored — do NOT strip tags, the frontend will render it.

Do not touch the frontend in this step. Report the files changed and the exact new SELECT column line.
