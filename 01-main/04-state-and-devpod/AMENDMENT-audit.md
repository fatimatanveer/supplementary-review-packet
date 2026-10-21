# Audit interpretation amendment — 5 October 2026

All three state-v1 fixtures completed before replay. The first checker receipt,
receipts/state-v1.json, contains six errors: its malformed-frame rule incorrectly
required a native OpenSSH disconnect. The public records show native response 05
in both phases of all fixtures; the repaired filter closes its connection. The
earlier baseline checker did not prescribe native disconnect behavior.

The corrected rule distinguishes native malformed-request rejection from repaired
parser disconnect. Neither is evidence of an authorization decision over a valid
request. Permitted authentication controls follow both outcomes. No raw records,
runner, frozen inputs or historical receipt are changed. The initial checker is
retained in the collection snapshot; the amended checker produces a new receipt.
This is a retrospective interpretation correction, not preregistered confirmation
of the native malformed-frame outcome.
