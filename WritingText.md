#246 – Implemented. Tested in Dev and pushed to the develop branch. Not yet deployed to QA.

What changed:
- CASRR now shows a warning 15 minutes and again 5 minutes before the automatic sign-out that happens 4 hours after sign-in. The warning appears on every screen and shows the exact time the session will end, with a countdown.
- On the Review Form, if there are unsaved changes, the warning says so and offers a "Save now" button, which runs the normal Save (with all the usual checks). "OK" closes the warning without changing anything.
- The 4-hour limit itself is unchanged and cannot be extended from the warning; it simply gives users time to save their work.
- If the session still ends with unsaved changes, they are kept as a local draft and the "Restore unsaved changes?" prompt appears when the review is opened again after signing in.

While testing this, we also found and fixed an existing issue where, in some cases (for example switching browser tabs while a review was still loading), an unsaved draft could be removed before the "Restore unsaved changes?" prompt was shown.

Tested in Dev: both warnings, Save now with unsaved changes, OK, the automatic sign-out, and restoring the draft after signing in again.

I will confirm here once deployed to QA.
