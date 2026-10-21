SELECT Policy_exception_information, Policy_exception_mitigation
FROM dbo.[02_CORE_02_Reviews] WHERE Review_id = <RID>;

SELECT * FROM dbo.[02_CORE_06_Policy Exceptions] WHERE Review_id = <RID>;
