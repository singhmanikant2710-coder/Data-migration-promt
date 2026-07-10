Plan approved. This reuse of LookupsController/ReportingService is cleaner than a new controller — good.

Proceed step-by-step:
1. IReportingRepository.cs (add method signature)
2. SqlReportingRepository.cs (implement with DISTINCT TOP syntax)
3. ReportingService.cs (add the "fhn-naics-industries" case)
4. Frontend CustomerInfoSection.tsx (SelectField wiring, fetch once on mount)

Pause after each file for my confirmation. Keep binding (section="customerInfo", name="naicsIndustry") unchanged. Confirm the useEffect uses [] dependency and fetches exactly once.
