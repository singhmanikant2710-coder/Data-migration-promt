-- pehle backup (ek baar, dono bugs ke liye kaafi)
SELECT *
INTO tblMain_Athens_Backup_20260921
FROM tblMain
WHERE strCustomerName = 'ATHENS PAPER COMPANY INC';


DELETE FROM tblMain WHERE strCustomerName = 'ATHENS PAPER COMPANY INC';

INSERT INTO tblMain
SELECT * FROM tblMain_Athens_Backup_20260921;
