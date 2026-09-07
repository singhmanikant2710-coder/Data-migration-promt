SELECT r.Sample_id, c.Review_id, r.Customer_name, r.Completed_date,
       c.Covenant_type, c.Covenant_category,
       c.Covenant_last_eval_status AS Status,
       c.Covenant_financial_result AS Result
FROM [02_CORE_05_Covenants] AS c
INNER JOIN [02_CORE_02_Reviews] AS r ON r.Review_id = c.Review_id
WHERE r.Sample_id = 357
ORDER BY c.Review_id;

SELECT r.Sample_id, c.Review_id, r.Customer_name, r.Completed_date,
       c.Covenant_type, c.Covenant_category,
       c.Covenant_last_eval_status AS Status,
       c.Covenant_financial_result AS Result
FROM [02_CORE_05_Covenants] AS c
INNER JOIN [02_CORE_02_Reviews] AS r ON r.Review_id = c.Review_id
WHERE r.Sample_id = 357
  AND (
    UCase(Trim(Nz(c.Covenant_last_eval_status,''))) IN ('NON-COMPLIANT','NOT COMPLIANT','NOT-COMPLIANT','PAST DUE','PAST-DUE')
    OR UCase(Trim(Nz(c.Covenant_financial_result,''))) IN ('NON-COMPLIANT','NOT COMPLIANT','NOT-COMPLIANT')
  );

  SELECT r.Sample_id, c.Review_id, r.Customer_name, r.Completed_date,
       c.Covenant_type, c.Covenant_category,
       c.Covenant_last_eval_status AS Status,
       c.Covenant_financial_result AS Result,
       c.Covenant_threshold, c.Covenant_last_eval_date
FROM [02_CORE_05_Covenants] AS c
INNER JOIN [02_CORE_02_Reviews] AS r ON r.Review_id = c.Review_id
WHERE r.Customer_name LIKE '*COTTI*' OR r.Customer_name LIKE '*SOUTHERN BREW*';
