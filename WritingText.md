git commit -m "UAT #263: changing the Component/Finding Code of a saved CRM finding replaces it instead of creating a duplicate; block duplicate codes, swaps and blank codes on save; unsaved-row delete is local only"
git push origin develop


SELECT f1.Review_id, r.Customer_name, r.Sample_id,
       f1.Finding_code AS Code_1, f2.Finding_code AS Code_2,
       LEFT(CAST(f1.Finding_comments AS nvarchar(max)), 120) AS Comments
FROM dbo.[02_CORE_07_Findings] f1 WITH (NOLOCK)
JOIN dbo.[02_CORE_07_Findings] f2 WITH (NOLOCK)
  ON f2.Review_id = f1.Review_id AND f2.Finding_code > f1.Finding_code
 AND CAST(f2.Finding_comments AS nvarchar(max)) = CAST(f1.Finding_comments AS nvarchar(max))
JOIN dbo.[02_CORE_02_Reviews] r WITH (NOLOCK) ON r.Review_id = f1.Review_id
WHERE LEN(LTRIM(RTRIM(CAST(f1.Finding_comments AS nvarchar(max))))) > 0
ORDER BY f1.Review_id;


#263 – Fixed. Tested in Dev and pushed to the develop branch. Not yet deployed to QA.

What was wrong: When the CRM Component or Finding Code of a previously saved finding was changed, Save added a new row for the new code but never removed the original, so both appeared with the same comments. (This has been the save behaviour since July; it only shows up when a saved finding's code is changed.)

What changed:
- Changing the Component and/or Finding Code of a saved finding now replaces that finding, keeping its comments and other details. Changing it more than once before saving keeps only the final code.
- Save is now stopped with a clear message (and nothing is changed) when: two findings would have the same Finding Code; a Component is changed without selecting a new Finding Code; or two findings' codes are swapped in one save (please save one change at a time).
- Deleting a finding that has not been saved yet no longer affects a saved finding with the same code.

Existing duplicates: this fix prevents new duplicates but does not remove existing ones. I can provide a list of reviews where two different finding codes have identical comments (likely duplicates) from QA/Production for your team to review before deleting.

Tested in Dev: your UW-105 → CS-105 scenario, the blocked cases above, and deleting a changed finding.

I will confirm here once deployed to QA.


Hi John / Ashok,

Could you please run the read-only query below in QA and Production and share the results? It lists reviews where two different CRM finding codes have identical comments – likely duplicates created by the CRM Findings bug (#263). It makes no changes; the business will review the list before anything is deleted.

<paste the duplicate SQL here>

Thanks,
Manikant


In Dev, one review matches this check: 19184 (PALAMAD LLC) – CS-104 and SM-103 with the same comment. This may be intentional, so please confirm before anything is removed. I've asked John/Ashok to run the same check in QA and Production.

