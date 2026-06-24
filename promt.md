READ-ONLY. Do NOT edit. Report only.

When the Review Summary PDF modal (ReviewPDFModal) opens for a borrower, trace 
where its data comes from:

1. Frontend: which service/API call does ReviewPDFModal make, and what 
   endpoint + params (reviewId? ecif?) does it hit to get the review data?
2. Backend: which controller -> service -> repository method serves that 
   endpoint? Name the repository method that fetches the single review's full 
   data.
3. In that repository method, which DB table(s) does it read from, and which 
   columns map to each PDF field: eCIF, Review ID, Review Type, Sample Type, 
   Sample Date, Start Date, Completed Date, Segment, Unit, Market, Line of 
   Business, NAICS Industry, Relationship Manager, Portfolio Manager, CRO, 
   Review Approver, Bank PD, CAS PD, Collateral Rating, Risk Recognition Rating, 
   Scorecard Mgmt Rating, Underwriting Rating, Credit Servicing Rating, Loan 
   Admin Rating?
4. Paste the SQL/query and the column-to-field mapping (especially the ordinal 
   GetValue(n) indices if it uses a DataReader), so I can verify the mapping is 
   aligned correctly.

Report only. No edits.

SELECT * FROM dbo.[02_CORE_02_Reviews] WHERE Review_id = 1058;
