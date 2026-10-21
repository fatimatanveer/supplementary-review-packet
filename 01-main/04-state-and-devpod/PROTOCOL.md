# Current-source coverage consolidation — 5 October 2026

Frozen before collection. Author operated; not external reproduction.

## Question and inputs

Do the distinct signing/context transitions previously exercised in 0.5.2 also
hold for the retained repaired 0.5.3 with the corrected OpenSSH 10.5p1 agent?
Use the existing hashed stock/repaired executables and cached runtime; no rebuild.
The copied baseline is identified by BASELINE.json. Preserve preceding studies.

## Schedule

Three fresh fixtures, disposable Ed25519 identities and destination/transport host
keys, qualified native gates. First execute the baseline direct/topology/restart
schedule unchanged. Then exercise all 23 historical state-case labels on native
and repaired sources in both unconstrained and constrained phases (92 case groups
per fixture). These are nested observations, not 92 independent replications.

Cases: known-a, known-b, bound-b, ordinary, hostbound, destination-b, missing-bind,
inner-key, session-mismatch, host-mismatch, algorithm, bound-raw, bound-sshsig,
sign-before-bind, repeat-bind, rebind-auth, forward-only, forward-chain,
invalid-bind, cross-connection, concurrent, truncated-sign, unknown-extension.

Record public requests/replies, connection identities, interval timestamps and
bracketing real permitted authentication. Native unconstrained A signs arbitrary
data; the repaired filter adds identity selection, not the reference P4 grammar.
Constrained expectations follow the earlier checker-owned specification and
OpenSSH's declared binding/destination semantics. Codes 5 and 28 are refusals;
code 6 alone accepts a binding. Rejected repeated binding retains earlier accepted
context. A malformed disconnect is classified separately from authorization.

Add a repaired-source-only management sequence: remove B, remove all, lock, unlock,
and an unknown extension. Expect refusal and unchanged source usability; bracket
each operation with permitted A and original-source B authentication. Do not send
private-key-bearing add requests. Native destructive management is outside this
identity-filter test. Existing historical management evidence retains its scope.

## Independence, controls and stopping

The checker does not import the runner. It checks complete schedules, exact public
bytes, signatures, accepted bindings, per-connection order, distinct concurrent
contexts and pre/post authentication intervals. Two concurrently live clients
synchronize after binding before they sign. Agent restarts retain explicit events.

Run with network disabled, 2 CPUs, 768 MiB, 256 PIDs, a 10-minute container deadline,
and no personal-agent/key mounts. Stop on native qualification failure or runner
error; retain the attempt. An unexpected policy result is evidence, not a reason
to change expectations. An instrument repair requires a dated amendment and a new
attempt. Do not erase or overwrite collected data.

## Scope

This consolidates finite-state coverage and metadata, not exhaustive assurance.
Malformed-input fuzzing, full management semantics, hardware keys and external
human validation remain outside the measured claims. All results must be reported
before editorial conclusions are updated.
