Edit ONLY backend/src/Bcat.Infrastructure/SqlServer/SqlMainRepository.cs, inside method TryMergeTtmIntoSeries. Change only the merge write loop. Show me the final code, do not run build.

Find this exact block (the loop that writes merged TTM values onto the row):

                        foreach (var kv in dict)
                        {
                            var key = kv.Key;
                            var val = kv.Value;
                            if (!val.HasValue) continue;

                            var existingNum = p.Values.TryGetValue(key, out var existingObj) ? CoerceNullableDouble(existingObj) : null;
                            // Treat 0 as missing for TTM components to align with legacy (overwrite stale zeros)
                            if (!existingNum.HasValue || Math.Abs(existingNum.Value) <= 0)
                            {
                                p.Values[key] = val.Value;
                            }
                        }

Replace it with this (always overwrite with the tblMainTTMCalculations value, since that is the correct year-agnostic trailing-12 source that matches legacy; the tblMain columns may hold a stale year-locked value):

                        foreach (var kv in dict)
                        {
                            var key = kv.Key;
                            var val = kv.Value;
                            if (!val.HasValue) continue;

                            // Always overwrite with the tblMainTTMCalculations value: it is the
                            // authoritative year-agnostic trailing-12 TTM that matches legacy.
                            // tblMain's *TTM columns can be stale/year-locked, so we prefer the merge value.
                            p.Values[key] = val.Value;
                        }

The ONLY change: remove the "if (existing is 0 or missing)" condition so the value is always written. Keep the "if (!val.HasValue) continue;" line (we still skip when the source TTM value itself is null). Do NOT change anything else in the method — not the dict assignments, not the YTD fallback, nothing.

After editing, show me the final version of that loop so I can verify. Do not run build.
