Hi Geoff,
1. Delinquent_status timeline:
This will take about 30 minutes — implementation, a build, and a quick test to confirm "1-30", "30-59", etc. all display correctly in Excel afterward, without affecting any other column or export. I'll get started and let you know once it's verified.
2. Restricting CORE Exports to Administrators:
Happy to scope this out for you. Here's a quick breakdown of what it would involve:
The app already has a concept of user roles/permissions (I'd need to check exactly how Admin status is determined in the current system — likely tied to the Users table you mentioned, or an existing "Administrator" role flag).
The change itself would be adding a permission check on the CORE Exports screen/page — so it only renders (or the menu item only appears) for users flagged as Administrator, and non-admins either don't see the option at all, or get a "not authorized" message if they try to access it directly by URL.
This is a moderate-sized change, not a quick one — because it touches:
Checking how the existing role system works (may already have what we need, or may need a small addition)
The frontend (hiding the menu/page for non-admins)
The backend (blocking the API endpoint itself for non-admins, not just hiding the button — this matters for real security, not just UI hiding)
I'd estimate this at half a day to a full day of work once I understand how the existing admin/role system is structured, since I want to do it properly (backend-enforced, not just a hidden button that a non-admin could still access via a direct link).
Let me know if you'd like me to move forward with this, and I'll start by investigating how admin roles currently work in the app so I can give you a tighter estimate.
