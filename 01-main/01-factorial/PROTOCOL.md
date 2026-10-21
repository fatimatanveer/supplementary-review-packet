# H1 follow-up v1 — frozen factorial, state, lifecycle and resource study

Date: 4 October 2026. New author operated evidence, not independent replication.
Historical experiments are read-only. Freeze this file, specification, all runner
and verifier sources, dependencies, generated filter source, build manifest and
binary hashes before the first outcome. Use append-only attempt directories.

## Hypotheses and units

H-F: binding forwarding and per-downstream upstream-connection preservation are
jointly required for useful constrained authentication in the tested H1 design.
Four builds of the same normalized 0.5.2 source differ only in H1_BIND/H1_REUSE
macros. Separate flushed input/output wrappers are common to all four. Compare
stock original, byte-identical revert, historical repaired source, exact packaged
binary, native agent, transparent relay, and source-built upstream 0.5.3 as controls.
Do not describe normalized neither as the stock implementation.

Three fresh fixtures are the units. A/B and destination host keys are regenerated.
Each fixture loads A+B before filter startup, uses only its own agents and two
loopback sshd destinations, disables connection multiplexing/alternate auth and
server source penalties, and uses public-only outcome identity files. All live
outcomes run in the cached, recorded Linux image with network disabled, 2 CPUs,
768 MiB RAM, and 256 PIDs. Compile only the required small filter binaries, with
sequential builds. No user agents, user keys, remote production hosts or IDE grants.

## Factorial and workflow endpoints

Qualify native unconstrained A/B at both destinations, constrained native A
allow/deny, and unconstrained direct A through every filter. Rotate arm order by
fixture. Run unconstrained / constrained / restored phases; A/B authentication at
both destinations and direct known A/B requests without enumeration in each arm.
Expected constrained A: native, transparent, historical repair and combined
normalized repair allow/deny; package, stock/revert, neither, bind-only,
reuse-only and stock 0.5.3 deny/deny. All filters exclude direct B. Native B remains
usable after negative A authentications. A dedicated agent with only constrained
A is a separate operational alternative, expected allow/deny and no B. It changes
the source and is not a claim about original-source continuity.

## Repaired-H1 state and lifecycle endpoints

Use the specification in specifications/requirements.md, not the historical P4
oracle. Matched public byte sequences compare native, historical repair and
combined repair in constrained and unconstrained phases. Valid signatures are
verified over exact bytes. Test missing/invalid/repeated/reordered binding,
forward-only, mismatched session/host/inner key, ordinary/host-bound form,
cross-connection, two interleaved clients, close/new connection, raw data, excluded
B, unknown extensions, malformed frames, and management requests. P4 grammar
restrictions are not imposed on H1. Malformed disconnects remain descriptive.
For refused signing, bracket cases with permitted native and repaired controls.
Record full public request/reply frames and upstream connection identifiers.

Lock regression: lock a disposable original agent through a separate local
management connection, issue forwarding binding via native or repaired endpoint,
unlock locally, then attempt a final allowed-host bind and destination-shaped
signing on the retained connection. The policy permits direct DEST_A only, not
the additional forwarding hop. Register this as a descriptive diagnostic because
package backport state must be established separately; never mislabel inherited
agent behavior as a filter-specific vulnerability. Record lock/bind/unlock
responses and post-test health. A separate restart experiment kills only the
fixture's original agent, starts a new agent at that path, reloads the same policy,
then compares retained/new downstream connections. No automatic reconnect is
assumed; a dead path is not a policy denial.

## Bounded operational measurements

Measure native, stock, transparent and repaired processes in one additional
disposable fixture: 1 then 8 held clients; 20 sequential authentication cycles;
8 concurrent authentications; and 50 connect/list/disconnect cycles. Observe
/proc file descriptors, threads, RSS, socket identities and cleanup after clients
close. Record raw timings (monotonic clock); summaries are descriptive, not
performance superiority estimates. The tracer adds overhead. Idle-held observation
lasts at most 1 second; this is not a long-idle or denial-of-service evaluation.
Native/transparent/repaired authenticate using constrained A. Stock uses a
separately identified unconstrained diagnostic phase so its latency is not a
matched constrained-workflow comparison.

## Accounting and stopping

Qualification failures are excluded but retained. After qualification, every
scheduled endpoint including failure/inconclusive remains countable. Stop on
unrecoverable setup error or unsafe unintended resource growth; preserve logs.
Never repeat an endpoint to replace an unwanted outcome. An instrument correction
requires an amendment and a new attempt; retain the prior source freeze/results.
Historical source-derived corpus results are not relabeled independent. One
focused offline verifier checks hashes, signatures, auth logs, schedule coverage,
controls and connection associations. 


