# Reviewer revision results — 4 October 2026

These are new author operated experiments, not independent replication.
Canonical live data: `evidence/v3/`. Final evidence audit:
`evidence/verification/final.json`. The earlier attempts remain unchanged.

## Current-source repair

Two binaries were compiled from the retained upstream 0.5.3 inputs: stock and a
connection-preserving research patch. Full input/binary digests and compilation
commands are in `builds/manifest.json`; the source diff is
`source/repair-0.5.3.diff`. This is not a distribution binary or upstream release.

Three corrected fixtures each contain 72 primary cells, 145 authentication/direct
records including qualification and health, and 42 state/protocol records. All
checker-owned primary expectations agree with the saved outcomes. Regenerated
fixtures, rather than individual requests, are the repeatability units.

| Operation | Native | Stock 0.5.3 | Repaired 0.5.3 |
| --- | --- | --- | --- |
| Unconstrained A to DEST_A / DEST_B | allow / allow | allow / allow | allow / allow |
| DEST_A-only A directly to DEST_A / DEST_B | allow / deny | deny / deny | allow / deny |
| Direct known B without enumeration | allow | deny | deny |
| DEST_A-only A through HOP to DEST_A | deny | deny | deny |
| Forwarded-policy A directly to DEST_A | deny | deny | deny |
| Forwarded-policy A through HOP to DEST_A / DEST_B | allow / deny | deny / deny | allow / deny |

Forwarded policy includes both `HOP` and `HOP>DEST_A`. It also permits A at HOP;
the study does not claim otherwise. A separate transport key, absent from the
outcome agent, supplies outer login. Native A topology gates and forwarded B
health qualify each policy phase before the filter comparisons.

In each corrected fixture, the checker decodes four permitted chains: native and
repaired direct paths contain DEST_A/authentication; native and repaired forwarded
paths contain HOP/forwarding followed by DEST_A/authentication. Host signatures
verify, and the final binding/session belongs to the successful signing connection.
This is evidence about the controlled OpenSSH/filter path. It supplies no missing
shipping-IDE hop measurement.

## State and restart boundaries

Seven selected cases compare native/repaired paths under direct DEST_A policy:
valid host-bound A, direct B, missing bind, invalid bind, cross-connection mismatch,
two concurrent session mismatches, and truncated signing. The intended policy
outcomes agree with the independently encoded checker schedule. The malformed
request is refused natively and disconnects on the repaired path; its latter
outcome is not an authorization denial. Real permitted authentication brackets
each case.

Before upstream restart, retained native/repaired paths sign valid requests.
After restart they become inconclusive/closed. New unbound requests refuse, and
new correctly bound requests sign; real authentication controls pass. There is
no transparent retained-connection recovery. The patch does not replace the
upstream agent's destination or lifecycle enforcement.

## Stronger evidence audit

The new checker does not import either runner, read runner-exported expected.json
as its oracle, or derive policy expectations from observation fields. Its own
specification enumerates the required state identifiers and expected signing
relations. It checks encoded key/domain/session/host semantics, binding signatures
and flags, exact-byte signing, controls, uniqueness, completeness and new topology
traces. Cryptographic verification uses the pinned environment's cryptography
library. 

| Audit | Fresh/live fixtures represented | Verified signatures | Errors |
| --- | --- | --- | --- |
| Retrospective audit of historical core | 3 historical fixtures; no new execution | 441 | 0 |
| New 0.5.3 corrected study | 3 new fixtures | 69 | 0 |

All six in-memory evidence mutations are rejected: omitted case, duplicate record,
known-B label carrying known-A bytes, altered embedded expectation, corrupted
signature, and absent bracketing control. Canonical evidence is never mutated.
The old checker and receipts remain available with their original limited scope.

## Attempt accounting

| Attempt | Recorded outcome | Use |
| --- | --- | --- |
| Build | Stock and repaired 0.5.3 targets compile | Provenance only; no full build |
| v1 | Three complete schedules; 134 auth/direct records per fixture; forwarded path ineligible | Retained instrument-confounded attempt, not canonical topology evidence |
| v2 | First fixture stops at added native topology gates; 75 auth/direct records | Forwarded B health detects unusable path; retained |
| v3 | Three qualified complete fixtures; 145 auth/direct and 42 state records each | Canonical new evidence |
| Final checker batch | Historical/new audits and six mutation checks pass | Saved-record validation, not independent live reproduction |

V1 omitted the first policy edge and used an outer `IdentityAgent=none` setting
that prevented usable forwarding. V2 corrected the policy and added enforced
gates; those gates exposed the remaining path problem. V3 changes the selected
outer agent endpoint while retaining independent transport-key login. No expected
scientific endpoint was revised to turn a failed observation into a success.


