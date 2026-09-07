Before I accept, show me the EXACT diff of ExportsController.cs — every changed line with - (removed) and + (added) prefixes. Specifically I need to see:

1. The full StripHtml method as it is NOW (after your change) — paste it completely.
2. Does StripHtml have a fast-path guard at the very top that returns the string unchanged when it contains no '<' and no '&'? i.e. is this line present:
   if (s.IndexOf('<') < 0 && s.IndexOf('&') < 0) return s;
   If NOT present, that is the main performance fix and it must be added.
3. Where exactly is StripHtml called in the row loop — paste those lines. Confirm it's applied only to string cells.
4. List every other line changed in this diff (the BOM change, the NOLOCK removal, the ORDER BY, and anything else), each with - / + prefix.

Just show me the diff and the current StripHtml body. Do NOT make new changes yet — I want to review what's already there first.
