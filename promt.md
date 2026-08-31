Bug #27 — Internal Notes: special symbols show as HTML codes (&quot;)
Reported by: John Halsrud

STATUS: Resolved.

ROOT CAUSE: Legacy/migrated notes in tblCustomer.strInternalNotes
(and strExternalNotes) were stored with HTML entities (&quot;,
&amp;, &lt;, &gt;) instead of raw characters. New saves store raw
characters correctly, but existing/migrated records displayed the
encoded form literally.

FIX: Added an HTML-entity decode step applied when displaying
Internal/External Notes, so entities render as the intended
characters (&quot; → ", &amp; → &, etc.). Applied consistently
across the notes view, edit, and PDF.

VERIFIED: First Financial Credit INC — notes with &quot; now
display correctly as " in the view, edit box, and blackbook PDF.
New saves continue to store raw characters. DB is untouched
(display-side decode), so no data migration was required.
