Hi Geoff, thanks for flagging. The XLSX upload works for smaller files, but
there's a size safeguard for very large Excel files (yours is 61 MB),
because large XLSX files are much heavier for the server to process than
CSV. For the full monthly Data Mart file, please save it as CSV and upload
that. It handles all ~86k rows. If uploading large XLSX files directly is
important to you, let us know and we'll look at whether that limit can be
safely raised.
