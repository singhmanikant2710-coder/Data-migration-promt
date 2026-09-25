Remove all [PERF] logging from SqlMainRepository.cs: the _perfSw /
_perfTotal stopwatches, _perfStep, and every [PERF] Console.WriteLine.
Keep all logic and the schema-cache changes exactly as they are.
Confirm grep for "[PERF]", "[TTM DIAG]", and "Console.WriteLine" in
files changed in this session returns 0 matches. Build, run unit tests.
