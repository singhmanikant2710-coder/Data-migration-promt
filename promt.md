Non-Compliant Covenants report shows NO data after rebuild. Diagnose where data is lost. READ-ONLY, no edits. Answer, STOP.

1. Frontend routing: when "Non-Compliant Covenants" is selected, confirm it now hits the non-compliant-covenants handler (not covenants-summary). Add a quick check of what id it resolves to.

2. Caller shape mismatch (most likely cause): the PDF was changed to read a new DTO shape (data.details, data.monitoringTotals, data.totalBorrowers, etc.), but does the caller in reports/page.tsx actually pass that shape? Paste the caller (~L969-999) and confirm it passes `data` = the full typed response, NOT the old `items` unwrap. If the PDF expects data.details but receives {items: [...]}, everything renders blank.

3. PDF field names: confirm the PDF component reads the EXACT field names the DTO/response provides (e.g. does the response use `details` or `Details`? camelCase vs PascalCase — JSON serialization casing mismatch is a common blank-data cause). Paste the response JSON field casing vs what the PDF reads.

4. Backend: for a sample with known non-compliant covenants, does the repository actually return rows? Or is TotalBorrowers=0 / Details empty because the sample has no matching covenants?

Report exactly where data is lost: routing, caller shape, JSON casing mismatch, or genuinely-empty backend result. Do NOT fix yet.
