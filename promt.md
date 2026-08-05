Confirmed — we're using [Bank_PD] for the "From PD" in all cases, exactly as you want. I traced it through the full data path:
From PD is sourced from a.[Bank_PD] → mapped to PdInitial (the "From" / row axis of the matrices).
To PD is sourced from a.[CAS_PD] → mapped to PdFinal (the "To" / column axis).
The PD range filter also uses Bank_PD and CAS_PD (not System_PD).
I also checked specifically for System_PD — it does exist as a column on the Accounts table, but it's not referenced anywhere in this report's query, so there's no accidental use of it. Good call double-checking, but you're all set here — it's Bank_PD throughout.
