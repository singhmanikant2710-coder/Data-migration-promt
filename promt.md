Plan approved — the dependency arrays (segments []; units [segment]; markets [segment, unit]) are correct and will prevent re-fetch loops. Good.

Proceed step-by-step:
1. IReportingRepository.cs (3 method signatures)
2. SqlReportingRepository.cs (implement, DISTINCT TOP + case-insensitive)
3. ReportingService.cs (3 new keys/methods)
4. LookupsController.cs (3 new routes)
5. Frontend CustomerInfoSection.tsx (cascade wiring)

Pause after each file for my confirmation. Keep bindings (section="customerInfo", names segment/unit/market) unchanged.

One check: confirm the frontend
