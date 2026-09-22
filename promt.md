Hi John, need your help on a data question for BCAT.

We've confirmed the fiscal-year labeling convention for most customers: for a customer whose fiscal year starts in month X (from tblCustomer.intFiscalYearMonthStart), the fiscal year is named for the year the cycle ENDS in. E.g. October 2025 to September 2026 = "FY2026" for an October-start customer. This is validated against 5+ customers' full legacy history in Access.

But 4 customers do the OPPOSITE consistently across their entire history — their fiscal year is named for the year the cycle STARTS in:
- Bankers Healthcare Group LLC (start month 10)
- Keystone Private Income Fund (start month 10)
- Vermeer Mountain West Inc (start month 10)
- Nationwide Specialty Finance Inc (start month 6)

We've ruled out industry, lender/syndication structure, and start-month as the differentiator — none of those explain why just these 4 are different. Nothing in tblCustomer or tblCustomerAdditional seems to flag it either.

Do you know of any per-customer setting, historical data-fix, or migration note that would explain why these specific 4 use the opposite fiscal-year-labeling convention? Or is there another table/column we haven't checked that might hold this?

Happy to hop on a call if easier — trying to close this out before finalizing a fiscal-year fix that's otherwise ready to ship.
