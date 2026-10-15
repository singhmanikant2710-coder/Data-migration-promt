UPDATE dbo.[02_CORE_02_Reviews]
SET Covenant_tracking_accuracy = NULL,
    Covenant_validation_accuracy = NULL,
    Covenant_breaches_addressed = NULL,
    Stepped_up_servicing = NULL
WHERE Review_id = <id>;   -- 1 row affected aana chahiye
