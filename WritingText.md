SELECT TOP 5 Review_id, Customer_number,
       Covenant_tracking_accuracy, Covenant_validation_accuracy,
       Covenant_breaches_addressed, Stepped_up_servicing
FROM dbo.[02_CORE_02_Reviews]
WHERE Covenant_tracking_accuracy IS NULL
  AND Covenant_validation_accuracy IS NULL
  AND Covenant_breaches_addressed IS NULL
  AND Stepped_up_servicing IS NULL;


  SELECT Covenant_tracking_accuracy, Covenant_validation_accuracy,
       Covenant_breaches_addressed, Stepped_up_servicing, Covenant_information
FROM dbo.[02_CORE_02_Reviews]
WHERE Review_id = <yahan id>;

