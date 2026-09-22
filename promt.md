Wanted to lay out exactly what we tried before landing on "these 4 are the only exceptions," since you asked us to dig deeper first.

We tested 5 different hypotheses for what might explain the convention difference, each checked against real data — not assumptions:

1. Is the formula itself wrong? Verified 5 customers' FULL history directly against legacy Access (Athens Paper, Imperial Trading, Charter Pipe, 1st Franklin, Colonial Auto Finance) — different fiscal start months, all matched our "ending-year" formula exactly. Formula itself isn't the problem.

2. Is there a flag on tblCustomer we're missing? Pulled all 200+ columns on that table looking for anything fiscal/convention-related. Found datFiscalYearStart, strFiscalYearMonthStart — checked both for all 4 exception customers, both empty/uninformative. No flag there.

3. Is it industry-based? All 4 exceptions had different industries at first glance except 2 DirectAuto. Pulled all 22 DirectAuto customers, checked a same-industry customer's (Colonial Auto Finance) full history — it followed the NORMAL convention. Ruled out.

4. Is it tied to fiscal start month? Imperial Trading and Nationwide are both June-start, but Imperial Trading is normal and Nationwide is an exception. Ruled out.

5. Is it about loan structure — single lender vs. syndicated? Pulled lender counts from tblCustomerAdditional for every customer. Syndicated customers split 10 normal / 2 exception — no correlation. Ruled out.

After all 5 came back negative, we ran it against the ENTIRE customer base — 521 customers in tblCustomer, 212 with actual fiscal data to check (rest are calendar-year or have no tblMain rows). Result: exactly 4 customers, 100% consistent across their whole history, follow the opposite convention. No 5th, no partial cases beyond those 4 — that's a full population scan, not a sample.

So: we can't find a structural/queryable reason for it in the data we have access to. At this point it looks like it has to be institutional knowledge — a deliberate decision made for these 4 customers at some point that isn't recorded in any field we can query. That's why we brought it to you rather than guessing further.
