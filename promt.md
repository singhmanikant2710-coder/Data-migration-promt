Modify ONLY this file:
backend/src/Casrr.Infrastructure/SqlServer/SqlReviewRepository.cs

Add a new method SaveCrmRatingsAsync that saves the 5 per-component CRM UNSAT flags 
and their comments into dbo.[02_CORE_02_Reviews], following the same simple-write 
ADO.NET pattern as SaveKeyRisksAsync. (Use the LIVE database column names below; 
ignore any columns.csv.)

public async Task SaveCrmRatingsAsync(
    int reviewId,
    bool riskRecognitionUnsat, string? riskRecognitionComments,
    bool scorecardMgmtUnsat, string? scorecardMgmtComments,
    bool underwritingUnsat, string? underwritingComments,
    bool creditServicingUnsat, string? creditServicingComments,
    bool loanAdminUnsat, string? loanAdminComments,
    CancellationToken ct)
{
    const string sql = @"
UPDATE dbo.[02_CORE_02_Reviews] SET
    [Risk_recognition_UNSAT] = @rr,  [Risk_recognition_comments] = @rrc,
    [Scorecard_mgmt_UNSAT]   = @sm,  [Scorecard_mgmt_comments]   = @smc,
    [Underwriting_UNSAT]     = @uw,  [Underwriting_comments]     = @uwc,
    [Credit_servicing_UNSAT] = @cs,  [Credit_servicing_comments] = @csc,
    [Loan_admin_UNSAT]       = @la,  [Loan_admin_comments]       = @lac
WHERE [Review_id] = @id;";

    using var conn = _connFactory.Create();
    await conn.OpenAsync(ct).ConfigureAwait(false);
    using var cmd = new SqlCommand(sql, conn) { CommandType = CommandType.Text };
    cmd.Parameters.Add(new SqlParameter("@id", SqlDbType.Int) { Value = reviewId });
    cmd.Parameters.Add(new SqlParameter("@rr", SqlDbType.Bit) { Value = riskRecognitionUnsat });
    cmd.Parameters.Add(new SqlParameter("@rrc", SqlDbType.NVarChar, -1) { Value = (object?)riskRecognitionComments ?? DBNull.Value });
    cmd.Parameters.Add(new SqlParameter("@sm", SqlDbType.Bit) { Value = scorecardMgmtUnsat });
    cmd.Parameters.Add(new SqlParameter("@smc", SqlDbType.NVarChar, -1) { Value = (object?)scorecardMgmtComments ?? DBNull.Value });
    cmd.Parameters.Add(new SqlParameter("@uw", SqlDbType.Bit) { Value = underwritingUnsat });
    cmd.Parameters.Add(new SqlParameter("@uwc", SqlDbType.NVarChar, -1) { Value = (object?)underwritingComments ?? DBNull.Value });
    cmd.Parameters.Add(new SqlParameter("@cs", SqlDbType.Bit) { Value = creditServicingUnsat });
    cmd.Parameters.Add(new SqlParameter("@csc", SqlDbType.NVarChar, -1) { Value = (object?)creditServicingComments ?? DBNull.Value });
    cmd.Parameters.Add(new SqlParameter("@la", SqlDbType.Bit) { Value = loanAdminUnsat });
    cmd.Parameters.Add(new SqlParameter("@lac", SqlDbType.NVarChar, -1) { Value = (object?)loanAdminComments ?? DBNull.Value });
    await cmd.ExecuteNonQueryAsync(ct).ConfigureAwait(false);
}

Modify ONLY SqlReviewRepository.cs.
