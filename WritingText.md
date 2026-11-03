#234 – Implemented. Tested in Dev and pushed to the develop branch. Not yet deployed to QA.
What changed: The Covenant Type dropdown in both Performance Covenants and Monitoring Covenants now has a search box at the top of the list. Typing any part of a covenant type (not case-sensitive) narrows the list; "No matches" is shown when nothing fits. The list can also be used with the keyboard (arrow keys, Enter, Esc). Saved values and the list of covenant types are unchanged.
I will confirm here once deployed to QA.

#236 – Implemented. Tested in Dev and pushed to the develop branch. Not yet deployed to QA.
What changed: The Policy Exception Description dropdown now has the same search box as #234. You can type part of the description or the code (e.g. "E01") to narrow the 52 options. Exception types already used on the review remain greyed out and cannot be selected. If a review has a saved exception code that is no longer in the list, the code is now shown instead of a blank "Select Exception Description".
I will confirm here once deployed to QA.


git add frontend/src/components/ui/FilterableSelect.tsx frontend/src/models/FilterableSelect.ts "frontend/src/app/review/[ecif]/review-info/components/sections/CovenantsSection.tsx" "frontend/src/app/review/[ecif]/review-info/components/sections/PolicyExceptionsSection.tsx"
git status
git commit -m "UAT #234, #236: type-to-filter search for Covenant Type and Policy Exception Description dropdowns (new FilterableSelect component)"
git push origin develop
