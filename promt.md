Decision: Option 1, plus the CompCallCode guard on EVERY path.

1. Recover via xlsxCellToText on all non-streaming paths (small Data Mart
   xlsx, client-side branch, sample files). Already-done parts unchanged.
2. Streaming path (large xlsx): keep streaming. Apply the CompCallCode
   guard there too: row.values gives raw values, so a numeric value in
   CompCallCode must reject the file with the same "format as Text (row N)"
   message. Same guard on the non-streaming paths. Do NOT add other
   columns to the reject list.
3. Monthly upload page: add a one-line hint near the file input, e.g.
   "For large files, CSV is recommended. In Excel, format code/ID columns
   (e.g. CompCallCode) as Text before saving."
4. CSV behaviour byte-identical on both uploads; numeric columns unchanged.

Report ADDED/REMOVED (file:line); behaviour for CompCallCode as an Excel
number on: small xlsx, large (streaming) xlsx, CSV; "01E0" as Text on
both xlsx paths; and an empty CompCallCode cell (must be allowed → NULL,
not rejected). Build results. Do not commit.

