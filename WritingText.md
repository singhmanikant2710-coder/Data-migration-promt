#257 – Fixed. Tested in Dev and pushed to the develop branch. Not yet deployed to QA.

What was wrong: Comment fields in the CRM Summary and CRM Findings & Observations Excel exports were written with their HTML/rich-text formatting codes.

What changed:
- CRM Summary: Risk Rating Justification (column R) and Finding Comments (column AC) are now clean plain text.
- CRM Findings & Observations: Comments are now clean plain text.
- The same clean-up applies to the new #256 exports (CRM Summary for Management – Risk Rating Justification and Review Comments; CRM Findings for Management – Comments). The other #256 exports have no rich-text fields.
- Paragraphs and line breaks are kept, bulleted lists show as "•" lines, tables show their cells separated by "|", images appear as "[Image]", and these columns wrap so the text is readable.
- PDFs are unchanged.

Note: in CRM Summary, each finding is a separate row, so the Risk Rating Justification is repeated on every finding row of the same review (as before).

I will confirm here once deployed to QA.


git checkout -- frontend/next-env.d.ts frontend/tsconfig.tsbuildinfo
git add frontend/src/app/reports/page.tsx frontend/src/lib/reportExcelExport.ts frontend/src/lib/reportExcelSheets.ts
git status
git commit -m "UAT #257: Report Excel exports write rich-text comments as clean plain text (no HTML), keeping paragraphs and bullets"
git push origin develop
