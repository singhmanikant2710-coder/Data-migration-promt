Register the new Review History service and repository in 
dependency injection, and verify the build.

Open this file ONLY:
backend/src/Casrr.Api/Extensions/StartupExtensions.cs

Find the section where IReviewRepository and IReviewService are 
registered (search for "IReviewRepository" or "IReviewService").

ADD these two lines in the exact same pattern, right after the 
existing Review registrations:

services.AddScoped<IReviewHistoryRepository, SqlReviewHistoryRepository>();
services.AddScoped<IReviewHistoryService, ReviewHistoryService>();

Make sure to add the correct using statements at the top of 
StartupExtensions.cs if needed (for the namespaces where 
IReviewHistoryRepository, SqlReviewHistoryRepository, 
IReviewHistoryService, and ReviewHistoryService are defined).

Do not change anything else in this file — do not touch any 
other existing registration.

After making this change, run a build check:
dotnet build backend/src/Casrr.Api/Casrr.Api.csproj

Show me:
1. Exactly which lines you added (with 5 lines of context before/after)
2. The full build output — confirm "Build succeeded" with no errors

If the build fails with a file lock error (DLL in use by another 
process), tell me and I will stop the running process first.
