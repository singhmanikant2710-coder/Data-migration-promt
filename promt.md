The Console.WriteLine diagnostic log is not appearing in the terminal when I hit Save. 

Show me exactly WHERE in ReviewController.cs you placed the log — paste the surrounding code. Confirm it is inside the Save action method body, before the postedSections guard.

Also: this app uses Serilog (I see [INF] structured logs in the terminal). Console.WriteLine may not surface. Replace it with the app's ILogger instead — use _logger.LogInformation(...) the same way other logs in this controller do.

Apply and show the diff.
