Investigated the fiscal-year-change scenario you flagged. Two things worth knowing:

1. Currently, changing a customer's fiscal start month in the edit screen updates only the customer record — it does NOT cascade to that customer's existing historical rows. Those keep their old fiscal year/month/dates until each one is individually re-saved.

2. We found something interesting in the legacy code: Access had this field LOCKED once initially set ("Disable selection once initially setup") — so in legacy, this scenario could never actually happen. That lock appears to have been dropped during migration.

Given that, there are two ways to make this "consistent" per your ask:
(a) Restore the legacy behavior — lock the field after it's first set, so it simply can't be changed once a customer has history. Matches original design, low effort/risk.
(b) Build a full cascade — when fiscal start month changes, automatically recompute fiscal year/month/dates and downstream metrics (AR turn days, inventory turn, YTD aggregates) for every existing month for that customer. This is a meaningfully bigger build — need to think through transaction scope and performance for customers with 100+ months of history.

Which direction do you want? Also still waiting on your read on the scope question from earlier (legacy-only fix vs. also SQL Server dev/prod) whenever you get a chance.
