Key clue: saving works when I ALSO change a scorecard grid field (which stages the "transactions" section), but fails with 400 "No changes were provided" when I change ONLY the Scorecard Comments rich text.

This proves the backend guard is not counting dto.Scorecard — it is arriving null or with Change=None.

Apply the temporary diagnostic log at the very top of the Save action in ReviewController.cs (before the guard):

try {
    var sc = dto?.Scorecard == null ? "Scorecard NULL" : $"Scorecard.Change={dto.Scorecard.Change}, DataKind={dto.Scorecard.Data.ValueKind}";
    Console.WriteLine($"[Save] {sc}");
    Console.WriteLine($"[Save] RAW BODY CHECK - dto null? {dto == null}");
} catch (Exception ex) { Console.WriteLine($"[Save] log error: {ex.Message}"); }

Apply it. STOP.
