Bug 204 — Review Progress screen: in the "Unopened/Cancelled" bucket, a column shows header "Completed" but should show "Cancelled" with the Cancelled date from CORE Reviews. Other buckets already have bucket-specific column logic. READ-ONLY, no edits. One pass, answer, STOP.

Screen: Review Progress (/review-status).

Investigate:
1. Find the Review Progress grid and the column that renders "Completed" (the one Geoff wants to become "Cancelled" for the Unopened/Cancelled bucket). What drives its header text and its cell value? File + line.
2. How is the current "bucket" (Unopened/Cancelled, In Progress, Draft Completed, Approved, Distributed, Finalized) determined in the component? Where is the selected bucket state?
3. Do OTHER buckets already switch this column's header/value based on the bucket (the "similar logic is in place for the other filtered views" Geoff mentions)? If yes, paste that existing bucket→column mapping (header label + which date field it shows per bucket). This is the pattern to extend.
4. Is the "Cancelled date" (Cancelled_date from 02_CORE_02_Reviews) available in the data returned for these rows? Check the backend query/DTO for the Review Progress grid — does it include Cancelled_date, or does it need to be added? File + line.
5. Identify exactly: (a) where the header label is set per bucket, (b) where the cell value/date field is chosen per bucket, and (c) whether Cancelled_date is already in the row data. So we can make the Unopened/Cancelled bucket show header "Cancelled" + Cancelled_date.

Report file paths + line numbers + the existing bucket→column logic + whether Cancelled_date is in the data. Do NOT fix yet.
