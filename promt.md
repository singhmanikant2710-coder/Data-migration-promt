Modify ONLY this file:
backend/src/Casrr.Infrastructure/SqlServer/SqlReviewRepository.cs

Add a new method DeleteCrmFindingAsync that deletes ONE finding from 
dbo.[02_CORE_07_Findings] by composite key (Review_id + Finding_code), following 
the same simple-write ADO.NET pattern as SaveKeyRisksAsync:

public async Task DeleteCrmFindingAsync(
    int reviewId,
    string findingCode,
    CancellationToken ct)
{
    const string sql = @"
DELETE FROM dbo.[02_CORE_07_Findings]
WHERE [Review_id] = @id AND [Finding_code] = @code;";

    using var conn = _connFactory.Create();
    await conn.OpenAsync(ct).ConfigureAwait(false);
    using var cmd = new SqlCommand(sql, conn) { CommandType = CommandType.Text };
    cmd.Parameters.Add(new SqlParameter("@id", SqlDbType.Int) { Value = reviewId });
    cmd.Parameters.Add(new SqlParameter("@code", SqlDbType.NVarChar, 50) { Value = findingCode });
    await cmd.ExecuteNonQueryAsync(ct).ConfigureAwait(false);
}

Modify ONLY SqlReviewRepository.cs.

------‐--------------
Modify ONLY this file:
backend/src/Casrr.Application/IReviewRepository.cs

Add a declaration matching SaveKeyRisksAsync style:

// Delete a single CRM finding from dbo.[02_CORE_07_Findings] by Review_id + Finding_code
Task DeleteCrmFindingAsync(
    int reviewId,
    string findingCode,
    CancellationToken ct);

Modify ONLY IReviewRepository.cs.

--------------------
Modify ONLY this file:
backend/src/Casrr.Api/Controllers/ReviewController.cs

Add a new endpoint to delete one CRM finding, matching the style of the Save endpoint. 
The controller base route is [Route("api/v1/reviews")]. Add:

[HttpDelete("crm-finding")]
[ProducesResponseType(StatusCodes.Status200OK)]
[ProducesResponseType(StatusCodes.Status400BadRequest, Type = typeof(ProblemDetails))]
public async Task<ActionResult> DeleteCrmFinding(
    [FromQuery] int reviewId,
    [FromQuery] string findingCode,
    CancellationToken ct)
{
    if (reviewId <= 0 || string.IsNullOrWhiteSpace(findingCode))
        return BadRequest();

    await _svc.DeleteCrmFindingAsync(reviewId, findingCode, ct);
    return Ok();
}

If _svc (the review service) does not yet expose DeleteCrmFindingAsync, you will 
need to add it to the service and its interface too — if that requires editing 
another file, STOP and tell me first so I can guide the service-layer wiring.

Modify ONLY ReviewController.cs
