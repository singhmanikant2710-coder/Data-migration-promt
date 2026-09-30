SELECT strCovenantName, strCovenantReported
FROM tblMainCovenants
WHERE strCustomerName LIKE 'ATHENS PAPER%' AND strMonthKey = '202603';

SELECT strCovenantAName, strCovenantAReported, strCovenantBName, strCovenantBReported,
       strCovenantCName, strCovenantCReported
FROM tblCustomer WHERE strCustomerName LIKE 'ATHENS PAPER%';
