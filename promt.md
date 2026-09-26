SELECT 'tblMain' t, COUNT(*) n FROM tblMain
 WHERE strCustomerName LIKE 'ATHENS PAPER%' AND strMonthKey IN ('202604','202605')
UNION ALL SELECT 'tblMainCovenants', COUNT(*) FROM tblMainCovenants
 WHERE strCustomerName LIKE 'ATHENS PAPER%' AND strMonthKey IN ('202604','202605')
UNION ALL SELECT 'tblMainDisplayCovenants', COUNT(*) FROM tblMainDisplayCovenants
 WHERE strCustomerName LIKE 'ATHENS PAPER%' AND strMonthKey IN ('202604','202605')
UNION ALL SELECT 'tblMainTTMCalculations', COUNT(*) FROM tblMainTTMCalculations
 WHERE strCustomerName LIKE 'ATHENS PAPER%' AND strMonthKey IN ('202604','202605');
