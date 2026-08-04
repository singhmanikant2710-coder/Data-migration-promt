READ-ONLY. Diagnostics only. Do not change anything.

In CrmPdGradeMigrationPDF.tsx, the DETAIL table has a "DIRECTION" column that 
currently shows "Up"/"Down" but with WRONG logic. Geoff's correct rule:
- If CAS PD > Bank PD  -> "Downgrade"
- If CAS PD < Bank PD  -> "Upgrade"
- Else (equal)         -> "No Change"

Show me:
1. Where the DIRECTION column value is computed/rendered in the detail table. 
   Show the exact code — is it computed in the frontend from bankPd/casPd 
   fields on the row, or does it come pre-computed from the backend/data?
2. The exact field names for Bank PD and CAS PD on the detail row (e.g. 
   r.bankPd, r.casPd, r.pdInitial, r.pdFinal).
3. The current logic/labels being used (which produces the wrong "Up").
4. Confirm whether this direction is computed ONLY for display, or if it's 
   used elsewhere (so I know the change is display-only).

Do not edit anything. Findings only.
