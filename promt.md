Apply this edit now. Single file only.

File: backend/src/Casrr.Infrastructure/SqlServer/SqlReviewRepository.cs

Find the existing SQL query that selects [Risk_rating_justification] from dbo.[02_CORE_02_Reviews]. Add [Scorecard_information] to that SAME query (do not create a new query), and map the value into form.Scorecard.Comments.

Do this in both GetReviewByEcifAsync and GetReviewByKeysAsync.

Note: the ScorecardSection with a Comments property already exists on ReviewForm in ReviewQueue.cs — use it, do not re-create it.

Apply the edit and show me the diff. Do not touch any other file.
