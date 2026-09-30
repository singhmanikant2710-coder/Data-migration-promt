One defect before I accept: all-zero IDs are not treated as "no ID".

CustomerInfoSection.tsx:330 uses /^\d+$/.test(id), and '00000' passes
that check, so the one PM row with '00000' + a real name still renders
"00000 - NAME". Per Option A it must render the name only.

Fix it generically, in every place, so it can't come back:
1. Frontend toNumberName: show "ID - NAME" only when the ID is all digits
   AND not all zeros (e.g. add !/^0+$/.test(id)). An all-zero ID with a
   name renders the name alone; with no name, it renders empty.
2. ReviewService.SplitNumberName: an all-zero number part is treated as
   no ID (number = null), so saving never writes '00000' back.
3. PadEmployeeIdSql: an all-zero / '0' OfficerNumber or PM Number must not
   produce a "00000 - NAME" dropdown option. Pass it through unpadded, or
   exclude it, whichever keeps the existing WHERE/ORDER BY logic intact.
Valid IDs ('17436', '08784') must behave exactly as in your last change.

Report ADDED/REMOVED (file:line), the on-screen result for: '17436',
'08784', '00000' + name, '00000' + NULL name, and a NULL ID, plus the
build results. Do not commit.
