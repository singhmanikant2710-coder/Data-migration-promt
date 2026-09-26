BEGIN TRAN;
DELETE FROM tblMainCovenants WHERE strCustomerName LIKE 'ATHENS PAPER%' AND strMonthKey IN ('202604','202605');
DELETE FROM tblMainDisplayCovenants WHERE strCustomerName LIKE 'ATHENS PAPER%' AND strMonthKey IN ('202604','202605');
DELETE FROM tblMainTTMCalculations WHERE strCustomerName LIKE 'ATHENS PAPER%' AND strMonthKey IN ('202604','202605');
DELETE FROM tblMain WHERE strCustomerName LIKE 'ATHENS PAPER%' AND strMonthKey IN ('202604','202605');
