Formula confirmed: use Variant B —
fy = (start = 1) ? y : ((m >= start) ? y + 1 : y)
fm = ((m - start + 12) % 12) + 1

Before proceeding to the write-path fix, one more check on the validation
results: most of the 255 remaining Variant-B mismatches (start months 2,
4, 5, 6, 10, 11) show a specific signature — StoredFM values that
increment by exactly 1 with NO wraparound (e.g. Alan Wire Company:
4,5,6,7,8 instead of the correct 12,1,2,3,4; B W I Companies: 1,2,3,4,5
instead of 2,3,4,5,6). This matches the exact corruption pattern already
confirmed on Athens Paper's test DB from the prevRow.intFiscalMonth+1
defect — i.e., these are pre-existing corrupted rows from the ACTIVE bug,
not formula errors.

Run this check: for every mismatched row (Variant B), determine whether
its StoredFM sequence for that customer is a simple monotonic increment
with no 12-to-1 wraparound anywhere in its history (corruption signature)
versus a genuine formula disagreement. Report:
- Count of mismatches matching the corruption signature (expected: most
  or all of the 255)
- Any mismatches that do NOT match this signature — these would indicate
  a real remaining formula problem and need individual review before
  I approve Prompt 2

Do not modify any files. Report only.
