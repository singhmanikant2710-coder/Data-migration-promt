Run this to export legacy Access objects to text so they can be read. This is READ-ONLY on the .accdb (opens, exports definitions, closes). Do NOT modify the .accdb. Run it, then tell me the output folder path. Do not loop.

Save as export-legacy.ps1 in the repo root and run it:

$ErrorActionPreference = "Stop"
$accdb = "legacy\FileUpload\BCAT2016_VM.accdb"   # adjust if the TTM logic lives in the other .accdb
$outDir = "legacy\_exported"
New-Item -ItemType Directory -Force -Path $outDir | Out-Null

$access = New-Object -ComObject Access.Application
$access.OpenCurrentDatabase((Resolve-Path $accdb).Path)
$db = $access.CurrentDb()

# Export all Queries (SQL)
$qsql = ""
foreach ($q in $db.QueryDefs) {
    if ($q.Name -notlike "~*") {
        $qsql += "`n`n-- ===== QUERY: $($q.Name) =====`n" + $q.SQL
    }
}
Set-Content -Path "$outDir\queries.sql" -Value $qsql -Encoding UTF8

# Export all modules + form/report code-behind (VBA)
foreach ($comp in $access.VBE.ActiveVBProject.VBComponents) {
    $lines = $comp.CodeModule.CountOfLines
    if ($lines -gt 0) {
        $code = $comp.CodeModule.Lines(1, $lines)
        Set-Content -Path "$outDir\VBA_$($comp.Name).txt" -Value $code -Encoding UTF8
    }
}

$access.CloseCurrentDatabase()
$access.Quit()
Write-Host "Exported to $outDir"

After it runs, list the files created in legacy\_exported and STOP. Do not analyze yet.


READ-ONLY. Read only files under legacy\_exported\ (queries.sql and VBA_*.txt). Do NOT read any .accdb (binary). Do NOT open the current app's backend/frontend. Search these exported text files ONCE for the TTM logic. No loop — find, quote, stop.

Find and quote VERBATIM (with the source filename for each):

1) The exact formula/expression for Interest Expense TTM. Search for: "curInterestExpenseTTM", "InterestExpenseTTM", "InterestTTM", "Interest Coverage TTM". Quote the full SQL SELECT or VBA expression that computes it. I need to see: is it a trailing-12-month SUM of monthly interest, and over what window/date filter?

2) The exact formula for Interest Coverage TTM (e.g. is it EBIT TTM / Interest Expense TTM, or CAFC / Interest, or something else?). Quote it verbatim.

3) The exact formula for EBIT TTM (search "EBITTTM", "curEBITTTM", "EBIT"). Confirm whether EBIT TTM = PBT TTM + Interest Expense TTM, or a different composition. Quote it.

4) How the 12-month window is defined in legacy: is it a fixed count of 12 rows, a date-range (monthKey between X and Y), or a running SUM? Quote the WHERE/JOIN/window that bounds it to 12 months.

5) Whether the TTM sum resets or is affected by fiscal year boundary (relevant to Sept-30 year-end customers). Quote any intFiscalYear / fiscal filter used in the TTM computation.

OUTPUT:
- For each of 1-5: the source filename + verbatim quoted formula.
- One-line plain statement per item of what the formula does.
- Then STOP. Do not compare to the new app yet, do not propose changes.


$ErrorActionPreference = "Stop"
$accdb = "legacy\BCAT2016_VM.accdb"
$outDir = "legacy\_exported"
New-Item -ItemType Directory -Force -Path $outDir | Out-Null
$access = New-Object -ComObject Access.Application
$access.OpenCurrentDatabase((Resolve-Path $accdb).Path)
$db = $access.CurrentDb()
$qsql = ""
foreach ($q in $db.QueryDefs) { if ($q.Name -notlike "~*") { $qsql += "`r`n-- ===== QUERY: $($q.Name) =====`r`n" + $q.SQL } }
Set-Content -Path "$outDir\queries.sql" -Value $qsql -Encoding UTF8
foreach ($comp in $access.VBE.ActiveVBProject.VBComponents) { $lines = $comp.CodeModule.CountOfLines; if ($lines -gt 0) { $code = $comp.CodeModule.Lines(1, $lines); Set-Content -Path "$outDir\VBA_$($comp.Name).txt" -Value $code -Encoding UTF8 } }
$access.CloseCurrentDatabase(); $access.Quit()
Write-Host "Exported to $outDir"
