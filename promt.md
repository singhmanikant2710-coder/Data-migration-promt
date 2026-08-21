READ-ONLY. Do NOT edit. Find the generation path only.

Confirmed: InitialMemoPDF.tsx and FinalMemoPDF.tsx use fontFamily "Helvetica"
and neither calls ensureFontsRegistered(). So Initial/Final memos render ≥/≤ as
e/d.

Tell me (no edits):
1. Which file generates the Initial Memo and Final Memo PDFs for download/
   preview? Search where InitialMemoPDF and FinalMemoPDF are imported and where
   pdf(<InitialMemoPDF/>).toBlob() (or toBuffer, or an API) is called. Give the
   exact file path(s) and show that generation code.
2. Is it CLIENT-side (browser pdf().toBlob()) or SERVER-side (API returns PDF)?
3. Does that generator file already call ensureFontsRegistered() + prefetch the
   .ttf (like ReviewPDFModal does for the linesheet), or is it completely
   missing?
4. State the exact minimal fix: which file gets ensureFontsRegistered()+prefetch,
   and confirm InitialMemoPDF/FinalMemoPDF Page fontFamily must become
   "DejaVuSans".

Output: generator file path + client/server + what's missing + minimal fix. No edits.
