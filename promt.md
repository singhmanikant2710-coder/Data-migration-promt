
**#184 — CRM Findings and Observations Excel Export — Findings**

Hi Geoff,

I reviewed the CRM Findings and Observations Excel export. The export logic itself is working correctly — Columns L (CRM Component), M (Code), and N (Severity) each write the raw values coming from the finding records, with correct column mapping and no data loss.

On the three points:

- CRM Component ("00-CRM Admin") and Code ("CRM-00"): the export already displays whatever value the finding record holds for these fields. If a record's component/code is "00-CRM Admin" / "CRM-00", it will show as-is. These values come from the underlying data, not from export formatting.

- Severity ("N/A" default): the export currently writes the severity value as stored; when a record has no severity, the cell is blank. Introducing "N/A" as a new default value is a data/default-value change (as you noted, this will settle with the final data migration), rather than an export-side fix.

In short, the export is faithfully reflecting the source data. The three items look like they'll be resolved by the data migration / default-value setup rather than a change to the export code. Happy to make an export-side adjustment if you'd prefer any specific display mapping in the meantime — just let me know.

Thanks,
Manikant
