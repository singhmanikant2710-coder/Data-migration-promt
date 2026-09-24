BCAT P4: stop treating zero YTD PBT as missing (no TTM substitution)

Backend TryMergeTtmIntoSeries and Manufacturing top strip replaced a
stored YTD PBT of 0 with the TTM figure. Verified in SSMS and legacy
Access: zero YTD rows are genuine (monthly sum = 0). Fallback now fires
only when YTD is absent; NULL-path behaviour unchanged.
