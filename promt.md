Create FILE 2:
backend/src/Casrr.Application/Interfaces/
INaicsRepository.cs

Follow exact same pattern as:
ISelectionRepository.cs

Methods needed:
- GetAllAsync(string? sector = null)
- GetByKeyAsync(string naicsIndustryKey)
- GetDistinctSectorsAsync()
- GetDistinctDivisionsAsync()
- CreateAsync(Naics item)
- UpdateAsync(Naics item)
- DeleteAsync(string naicsIndustryKey)

Confirm file path after creation.
Wait for approval before FILE 3.
