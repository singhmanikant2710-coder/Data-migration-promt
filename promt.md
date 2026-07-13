The guard already includes Scorecard, yet a scorecard-only save still returns 400 "No changes were provided". So dto.Scorecard is arriving as null, or its Change is not binding.

The frontend sends exactly:
{
  "ecif": "id-17836",
  "reviewId": 17836,
  "sampleId": 311,
  "scorecard": { "change": "Upsert", "data": { "comments": "<b>Testing </b>Scorecard Comments" } }
}

Investigate and prove (read-only first):

1. Show the ReviewFormSaveRequest DTO — specifically the Scorecard property declaration and how the OTHER working single-object sections (e.g. RiskRatingJustification) are declared. Are they identical in type and JSON naming? Show both verbatim.

2. Show the SectionChange<T> type and the SectionChangeKind enum. How does "Upsert" (a string) bind to SectionChangeKind? Is there a JsonStringEnumConverter registered, or an attribute? Confirm RiskRatingJustification binds successfully with the same "change": "Upsert" string — if it does, Scorecard must be declared differently.

3. Check the JSON property name binding: does Scorecard need a [JsonPropertyName("scorecard")] attribute, or does the app rely on camelCase policy? Compare with RiskRatingJustification.

4. Add a temporary log at the very start of the Save action that dumps whether dto.Scorecard is null and what dto.Scorecard?.Change is, so we can see it at runtime.

Report the difference between how Scorecard and RiskRatingJustification are declared. That difference is the bug. STOP before editing.
