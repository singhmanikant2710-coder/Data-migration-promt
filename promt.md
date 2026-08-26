Edit ONLY the file backend/src/Bcat.Infrastructure/SqlServer/SqlMainRepository.cs, inside the method TryMergeTtmIntoSeries. Do not touch any other file or method.

Find this exact block of 10 lines:

                    dict["curProfitBeforeTaxesTTM"] = Read("curProfitBeforeTaxesTTM", "PBTTTM", "curPBTTTM");
                    dict["InterestExpenseTTM"] = Read("curInterestExpenseTTM", "InterestExpenseTTM", "InterestTTM");
                    dict["DepreciationTTM"] = Read("curDepreciationTTM", "DepreciationTTM");
                    dict["AmortizationTTM"] = Read("curAmortizationTTM", "AmortizationTTM");
                    dict["DistributionsTTM"] = Read("curDistributionsTTM", "DistributionsTTM");
                    dict["curCPLTDTTM"] = Read("curCPLTDTTM", "CPLTDTTM");
                    dict["curFixedChargesTTM"] = Read("curFixedChargesTTM", "FixedChargesTTM", "FixedChargeTTM");
                    dict["curNetChargeOffTTM"] = Read("curNetChargeOffTTM", "NetChargeOffTTM", "NetCOTTM");
                    dict["curAveragePrincipalNRTTM"] = Read("curAveragePrincipalNRTTM", "AveragePrincipalNRTTM", "AvgPrincipalNRTTM");
                    dict["curAverageGrossNRTTM"] = Read("curAverageGrossNRTTM", "AverageGrossNRTTM", "AvgGrossNRTTM");

Replace it with this exact block (only the first 4 dictionary KEYS change to add the "cur" prefix; every other line stays byte-for-byte identical):

                    dict["curProfitBeforeTaxesTTM"] = Read("curProfitBeforeTaxesTTM", "PBTTTM", "curPBTTTM");
                    dict["curInterestExpenseTTM"] = Read("curInterestExpenseTTM", "InterestExpenseTTM", "InterestTTM");
                    dict["curDepreciationTTM"] = Read("curDepreciationTTM", "DepreciationTTM");
                    dict["curAmortizationTTM"] = Read("curAmortizationTTM", "AmortizationTTM");
                    dict["curDistributionsTTM"] = Read("curDistributionsTTM", "DistributionsTTM");
                    dict["curCPLTDTTM"] = Read("curCPLTDTTM", "CPLTDTTM");
                    dict["curFixedChargesTTM"] = Read("curFixedChargesTTM", "FixedChargesTTM", "FixedChargeTTM");
                    dict["curNetChargeOffTTM"] = Read("curNetChargeOffTTM", "NetChargeOffTTM", "NetCOTTM");
                    dict["curAveragePrincipalNRTTM"] = Read("curAveragePrincipalNRTTM", "AveragePrincipalNRTTM", "AvgPrincipalNRTTM");
                    dict["curAverageGrossNRTTM"] = Read("curAverageGrossNRTTM", "AverageGrossNRTTM", "AvgGrossNRTTM");

The ONLY differences between the old and new block are these 4 dictionary keys:
- "InterestExpenseTTM"  becomes  "curInterestExpenseTTM"
- "DepreciationTTM"     becomes  "curDepreciationTTM"
- "AmortizationTTM"     becomes  "curAmortizationTTM"
- "DistributionsTTM"    becomes  "curDistributionsTTM"

CRITICAL RULES:
- Do NOT change any Read(...) arguments — they stay exactly as written.
- Do NOT change the line dict["curCPLTDTTM"] = Read("curCPLTDTTM", "CPLTDTTM"); — it must stay spelled "curCPLTDTTM" with "TTM". Never write "TMM".
- Do NOT change the other 5 lines (curProfitBeforeTaxesTTM, curFixedChargesTTM, curNetChargeOffTTM, curAveragePrincipalNRTTM, curAverageGrossNRTTM).
- Do NOT modify the write loop, the YTD fallback, or anything else in the method.

After editing, show me the final 10 lines exactly as they now appear in the file so I can verify. Do not run build.
