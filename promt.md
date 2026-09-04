Bug 198 — CASRR UAT (Geoff). READ-ONLY diagnosis. NO edits. One pass, answer, STOP.

ISSUE (regression): On Samples > Load Samples, the validation logic marks recent reviews as "Pass" when they should be "Hold". Credits that had a recent review date should be invalidated as "Hold". Example: in Sample ID 354, "American Bancard Processing" and "Cyprexx Services" should have been "Hold" (recent review dates) but passed; "750 Glenwood" in the same sample was correctly held. This previously worked — it's a regressive bug.

Trace the sample validation / hold logic:
1. Grep backend for the Load Samples validation logic (search: "Hold", "Pass", "validate", "invalid", "SampleLoad", "review date", "reviewDate"). Likely in SqlSampleLoadRepository.cs / SampleLoadController / ISampleLoadService / SampleLoadService. List the file(s) + method(s) that decide Hold vs Pass for a credit during sample load.
2. Paste the exact condition/SQL that determines whether a credit is held based on its recent review date. What is the date threshold/window (e.g. "reviewed within last N months")? 
3. Report the exact comparison: is it using >, >=, <, <=? What date field is compared to what cutoff? Is there any timezone/date-only vs datetime issue, or an off-by-one on the window?
4. Check git blame / recent changes on that method if visible — what changed recently that could have flipped correct Holds into Passes (regression)?
5. Report how "American Bancard Processing"/"Cyprexx Services" (recent review) would evaluate vs "750 Glenwood" (correctly held) under the current logic — why do the first two pass?

Report file paths + line numbers + the exact date-comparison logic. Do NOT propose or write a fix yet.
