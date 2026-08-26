Apply the diff exactly as shown. Only edit backend/src/Bcat.Infrastructure/SqlServer/SqlMainRepository.cs. Make exactly these two changes:

1) In required[]: change "curAverageGrossNR" to "curGrossNRorAR"
2) In the CTE: change AVG(curAverageGrossNR) to AVG(curGrossNRorAR) — keep the alias curAverageGrossNRTTM and the window clause exactly the same.

Nothing else changes. Apply now, then confirm curAverageGrossNR no longer appears as a source anywhere in RecomputeTtmCalculationsAsync (only curGrossNRorAR), and the output alias curAverageGrossNRTTM is unchanged.
