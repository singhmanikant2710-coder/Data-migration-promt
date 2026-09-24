READ-ONLY — this is a single fact-check, not a fix or a report.

Run this exact query against legacy MS Access and report ONLY the raw
result, nothing else:

SELECT datFiscalYearStart FROM tblMain
WHERE strCustomerName = 'Athens Paper' AND strMonthKey = '202510'
