Hi Geoff, quick confirmation before making all date fields Date-Only in these 4 exports:
All the date columns are technically datetime2 in the database, and I found that 53 rows in the Samples export currently have a real time-of-day recorded (not just midnight) in the Created_date field. Formatting as Date-Only would drop that recorded time from the export — it wouldn't be visible anywhere afterward.
Two questions:
Is that OK? Or should we keep the full timestamp on fields where a real time was recorded (like Created_date), and only date-only the fields that are genuinely date-only in nature (like Sample_start_date, Effective_date, etc.)?
Also — MM-DD-YYYY can be misread in Excel on non-US regional settings (03-04-2026 could mean March 4 or April 3 depending on the machine). Is MM-DD-YYYY specifically required, or is any unambiguous date-only format fine?
Once confirmed I'll implement — happy to do a blanket date-only if that's what's wanted, just flagging the time-loss so it's a deliberate choice, not an accident.
