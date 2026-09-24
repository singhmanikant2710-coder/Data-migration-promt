Sub CheckBCAT()
  Dim f As DAO.Field, e As String, q As DAO.QueryDef
  Debug.Print "=== Calculated fields ==="
  For Each f In CurrentDb.TableDefs("tblMain").Fields
    e = ""
    On Error Resume Next
    e = f.Properties("Expression")
    On Error GoTo 0
    If e <> "" Then Debug.Print f.Name & " := " & e
  Next
  Debug.Print "curEBIT type: " & CurrentDb.TableDefs("tblMain").Fields("curEBIT").Type
  Debug.Print "=== Queries using curEBIT ==="
  For Each q In CurrentDb.QueryDefs
    If InStr(q.SQL, "curEBIT") > 0 Then Debug.Print q.Name
  Next
End Sub

SELECT COUNT(*) AS total,
  SUM(IIf(Abs(curEBIT - (Nz(curProfitBeforeTaxes,0)+Nz(curInterestExpense,0)))<0.01,1,0)) AS ebit_eq_pbt_plus_int,
  SUM(IIf(curInterestExpense Is Null,1,0)) AS null_int,
  SUM(IIf(curDepreciation Is Null,1,0)) AS null_dep,
  SUM(IIf(curAmortization Is Null,1,0)) AS null_amort,
  SUM(IIf(curDistributions Is Null,1,0)) AS null_dist
FROM tblMain WHERE curEBIT Is Not Null;

SELECT m.strCustomerName, c.intFiscalYearMonthStart AS st, COUNT(*) AS rows_,
  SUM(CASE WHEN m.intFiscalYear = CASE WHEN c.intFiscalYearMonthStart = 1 THEN x.y
       WHEN x.mo >= c.intFiscalYearMonthStart THEN x.y + 1 ELSE x.y END THEN 1 ELSE 0 END) AS ending_year,
  SUM(CASE WHEN m.intFiscalYear = CASE WHEN x.mo >= c.intFiscalYearMonthStart THEN x.y
       ELSE x.y - 1 END THEN 1 ELSE 0 END) AS starting_year
FROM tblMain m
JOIN tblCustomer c ON c.strCustomerName = m.strCustomerName
CROSS APPLY (SELECT CAST(LEFT(m.strMonthKey,4) AS int) AS y,
                    CAST(RIGHT(m.strMonthKey,2) AS int) AS mo) x
WHERE m.strCustomerName IN ('Keystone Private Income Fund','Nationwide Specialty Finance Inc',
  'Bankers Healthcare Group LLC','Vermeer Mountain West Inc')
GROUP BY m.strCustomerName, c.intFiscalYearMonthStart;

READ-ONLY — do not propose or apply any fix.

Your last report had garbled/duplicated passages, so I need verbatim
proof before anything goes to the client. For each, quote the exact code
with file:line — no paraphrase:

1. dblFixedChargeCoverage SQL UPDATE (~4480-4484): does fixed charges
   include curDistributions?
2. perReserveCoverage SQL UPDATE (~4694-4700): exact formula.
3. perIneligiblePercent SQL UPDATE (~4724-4727): exact denominator.
4. Math.Abs on divisors (~1731-1737, 1753, 1805, 1867): quote each.
5. Does ANY code write to the Access database? Check
   AccessMainRepository and any OleDb/ODBC INSERT/UPDATE/DELETE.
   Quote each write, or state none exist.
6. In the extracted legacy VBA (Discovery/extraction_output), find
   procedures that loop over months to recalculate TTM or YTD, and any
   code that writes curEBIT or curEBITTTM. Quote names and lines.

Report only.
