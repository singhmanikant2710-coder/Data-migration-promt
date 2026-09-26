Your verbatim quote of line 245 is: m >= start ? y : y + 1
Backend line 2668 is:             m >= start ? y + 1 : y
These are NOT identical — the report page is inverted.
Change line 245 to exactly:
yr = String(start === 1 ? y : (m >= start ? y + 1 : y));
Build, do not commit. Same report format.
