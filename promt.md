Apply the diff exactly as shown. Only edit the perCashCollections block in backend/src/Bcat.Infrastructure/SqlServer/SqlMainRepository.cs. Change only the denominator columns from current-month to prior-month:
- curPrincipalNR -> curPrincipalNRPriorMonth (in Has checks and m.[...] references)
- curGrossNRorAR -> curGrossNRorARPriorMonth (in Has checks and m.[...] references)
Keep the numerator m.[curCashCollections], the CASE/selCash logic, NULLIF, CONVERT, and sets.Add exactly the same. Apply now, then confirm no curPrincipalNR or curGrossNRorAR (non-PriorMonth) remains in this block.
