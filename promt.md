Read-only. No edits. No plan. Just report with file paths + exact code.

The info (ℹ️) "Review Tip" popup on the Review Form shows "No help tip available", but the data exists in dbo.[03_LIBRARY_06_Help Tips] (verified: Help_tip_id 12, Help_tip_topic 'Unsatisfactory Ratings'). Trace the full path and report:

1) Frontend: the component rendering the info button + "Review Tip" dialog (CRM Ratings section, "Unsatisfactory Ratings"). Show the exact topic/key string it passes on click, and how it reads the tip from the API response.

2) Backend: the controller + service + repository method serving help tips. Show the exact query against dbo.[03_LIBRARY_06_Help Tips], its WHERE filter, and the exact column names used.

3) State clearly WHERE the match fails — one of:
   (a) frontend passes a topic string that doesn't exactly equal DB Help_tip_topic (case/spacing/trailing space),
   (b) API filters by Help_tip_id but frontend sends topic (or vice-versa),
   (c) API returns the row but frontend reads the wrong field name,
   (d) the help-tips endpoint isn't called at all for this button.

Use LIVE DB, ignore columns.csv. Output findings only. Change nothing.
