Create FILE 4:
backend/src/Casrr.Api/Controllers/
NaicsController.cs

Follow exact same pattern as:
SelectionsController.cs

Requirements:
- Route: "api/v1/naics"
- Authorize policy: "RequireActiveUser"
- Inherit: BaseTemplateController
- Inject: INaicsRepository, ILogger,
  TelemetryClient, IGraphUserInfoProvider
- Same error handling pattern as 
  SelectionsController

ENDPOINTS:

1. GET api/v1/naics/library
   - Optional ?sector= filter
   - Returns list of Naics items

2. GET api/v1/naics/library/{key}
   - Returns single item
   - 404 if missing

3. POST api/v1/naics/library
   - Validates NaicsIndustryKey unique
   - 201 on success
   - 409 on duplicate key

4. PUT api/v1/naics/library/{key}
   - Updates item
   - 404 if missing

5. DELETE api/v1/naics/library/{key}
   - Deletes item
   - 404 if missing

6. GET api/v1/naics/sectors
   - Returns distinct NAICS_sector values

7. GET api/v1/naics/divisions
   - Returns distinct NAICS_division values

Same error codes as SelectionsController:
INVALID_LIBRARY_INPUT
LIBRARY_NOT_FOUND
LIBRARY_DUPLICATE
UNEXPECTED_ERROR

Confirm file path after creation.
Wait for approval before FILE 5.
