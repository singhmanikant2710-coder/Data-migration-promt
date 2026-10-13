Create a test file only – no code changes, do not commit.

Goal: test the monthly-upload/save route change (stack removed, correlationId added) through the real UI on Admin > Monthly Upload Process.

1. First confirm from page.tsx (~:349) exactly which uploads go to /api/monthly-upload/save (file type and row threshold), so the test file really reaches route.ts and is not stopped by client-side validation.

2. Generate test-data/monthly-upload-wrong-header.xlsx (test-data/ is gitignored – never stage it):
   - Columns: the real MonthlyUploadHeaders in the correct order, EXCEPT rename one column header (e.g. "DocType" -> "DocTypeX") so the server-side header check (~route.ts:569) fails with "Header mismatch".
   - 1,100 data rows (above the threshold you confirmed in step 1) with simple dummy values; code/ID columns as text.
   - Use whatever library is already available in the repo (e.g. the xlsx/exceljs package the route uses); if you need a script, run it from %TEMP% and delete it afterwards.

3. Also generate test-data/monthly-upload-correct-header.xlsx with the same 1,100 rows and the correct headers, only so I can compare (I will NOT click Save with it).

Report: which upload path these files trigger (file:line), the exact error message the UI should show for the wrong-header file, file paths and sizes, and git status (test-data/ must not appear). Do not commit or push.
