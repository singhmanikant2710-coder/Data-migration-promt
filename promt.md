Add server-side validation in DistributionPartiesController Create and Update:
Role must be non-empty, digits only, max 5 characters. Return 400 with a
clear message otherwise. Report ADDED/REMOVED (file:line), behaviour for a
valid ID (must be unchanged), the blank/NULL Role case, and the build result.
Do not commit.
