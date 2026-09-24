On the leading-zero question — yes, we can absolutely do this without changing
the column type at all. Padding for display can be done at fetch time:

- In SQL: RIGHT('00000' + CAST(Relationship_mgr_number AS varchar(10)), 5)
  or FORMAT(Relationship_mgr_number, '00000')
- In .NET: value.ToString("D5") or value.ToString().PadLeft(5, '0')

This avoids touching the schema entirely, so no ALTER TABLE needed, no
dependent-constraint issue, and no reload risk.

One thing we need confirmed before doing this: is the Employee ID ALWAYS a
fixed width (e.g. always 5 digits in the source HR system, with shorter IDs
padded with leading zeros)? If yes, padding to 5 digits on fetch will exactly
recreate the correct value. If the width varies by employee, padding blindly
could produce an incorrect-looking ID. Can you confirm the fixed width from
the source system?

Given this works without a schema change, our suggestion is to hold off on
the ALTER TABLE for now — no need to fight the constraint error below unless
there's another reason to actually change the column type.

---

On the ALTER TABLE error — this is happening because a default constraint
(DF__02_CORE__0__Relat__7795AESF) is bound to Relationship_mgr_number. SQL
Server won't let you change a column's type while a constraint depends on
it. You'd need to drop the constraint first, alter the column, then
re-add the constraint if still needed:

-- 1. Confirm the exact constraint name (in case it's different per
--    environment)
SELECT dc.name AS ConstraintName, c.name AS ColumnName
FROM sys.default_constraints dc
INNER JOIN sys.columns c
    ON dc.parent_object_id = c.object_id
    AND dc.parent_column_id = c.column_id
WHERE dc.parent_object_id = OBJECT_ID('dbo.[02_CORE_02_Reviews]')
  AND c.name = 'Relationship_mgr_number';

-- 2. Drop the default constraint
ALTER TABLE dbo.[02_CORE_02_Reviews]
DROP CONSTRAINT [DF__02_CORE__0__Relat__7795AESF];

-- 3. Then alter the column
ALTER TABLE dbo.[02_CORE_02_Reviews]
ALTER COLUMN Relationship_mgr_number varchar(10) NULL;

But again — given the fetch-time formatting approach above works without any
of this, we'd suggest confirming with Geoff first whether the schema change
is even necessary before running it.
