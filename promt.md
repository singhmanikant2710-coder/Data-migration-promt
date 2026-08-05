READ-ONLY. Diagnostics only. Do not change anything.

The client (Geoff) wants to confirm which column is used as the "From PD" 
in the CRM PD Grade Migration report. It should be [Bank_PD], NOT [System_PD].

In the CRM PD Grade Migration data path:
1. In SqlCrmPdGradeMigrationReportRepository.cs — show the SQL SELECT, 
   specifically which column populates the "From PD" / PdInitial value. Is it 
   a.[Bank_PD], a.[System_PD], or something else? Show the exact column 
   reference and how it's aliased/mapped.

2. Also show which column populates the "To PD" / PdFinal value (should be 
   [CAS_PD] based on earlier findings — confirm).

3. In CrmPdGradeMigrationReportService.cs — confirm how PdInitial (From PD / 
   Bank PD) and PdFinal (To PD / CAS PD) are read from the raw query result 
   and mapped. Show the mapping (which raw field -> PdInitial, which -> 
   PdFinal).

4. Confirm the WHERE clause's PD range filter: earlier it was 
   "a.[Bank_PD] BETWEEN 1 AND 16 AND a.[CAS_PD] BETWEEN 1 AND 16" — confirm 
   it uses Bank_PD (not System_PD) for the From side.

5. Check if [System_PD] exists as a column in the Accounts table 
   (dbo.[02_CORE_04_Accounts]) and whether it's referenced ANYWHERE in this 
   report's query — so I can confirm we're NOT accidentally using it for 
   "From PD".

Report: exactly which column feeds "From PD" (Bank_PD vs System_PD), which 
feeds "To PD", and confirm no System_PD is used where Bank_PD should be. 
Do not edit anything. Findings only.
