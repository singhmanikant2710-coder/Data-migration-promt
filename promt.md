Apply the FY-2 gating now, but make sure it REPLACES the line, does not duplicate it.

The line that currently reads exactly:
{historyYear2Only.length > 0 && (

must become exactly:
{false && historyYear2Only.length > 0 && (

Replace that single line in-place. Do NOT add a new line — the old {historyYear2Only.length > 0 && ( must be GONE, replaced by {false && historyYear2Only.length > 0 && (. There must be only ONE opening line for this block after the change.

Everything else stays identical. Apply now, then confirm the file has exactly one "{false && historyYear2Only.length > 0 && (" and no leftover "{historyYear2Only.length > 0 && (" for this FY-2 block.
