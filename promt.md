Hi Geoffrey, update on UAT #29 (RM/PM dropdowns from Data Mart Trial):
Relationship Manager is good to go — OfficerName and OfficerNumber both have data (2,356 entries), so we can populate the dropdown as "Number - Name" and store both fields as you specified.
One blocker on Portfolio Manager: in 01_DATA_01_Data Mart Trial, PMName has data but the PM Number column is entirely empty (all NULL). So we can populate the PM dropdown by name, but there's no number to capture or store.
Could you confirm how you'd like to handle PM:
Is the PM associate number available from another field/table we should use instead?
Or should we proceed with PM name only for now, and add the number once that data is populated?
I'll build Relationship Manager fully in the meantime.
Thanks, Manikant
