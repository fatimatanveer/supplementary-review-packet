# Reviewer revision: current-source repair and forwarding-path discrimination

This is a new, bounded, author operated study. 

## Question and arms

Does the connection-preserving repair extend to the retained upstream 0.5.3
source, and can the repaired path distinguish direct authorization from permission
through a named forwarding host? Compare native OpenSSH, source-built stock 0.5.3
and repaired 0.5.3. The patch changes only request routing/context retention;
identity selection and cryptographic/policy decisions remain with their existing
owners. 

Use three fresh Linux arm64 fixtures with regenerated A/B, host and transport
keys. Each fixture uses the cached core image identified in launch.py, network
disabled, two CPUs, 768 MiB RAM, 256 PIDs, and loopback-only disposable sshd
processes. All outcome clients use public-only keys, fixed enrollment and host
keys, no multiplexing, no alternate authentication, and source penalties disabled.
Compile only the two filter targets. Do not benchmark performance in this study.

## Primary schedule (per fixture)

1. Qualify native A/B at both destinations, unconstrained A through every arm,
   and native constrained A allow/deny. Existing native-health controls qualify
   negative A authentication. Rotate the three arms between fixtures.
2. In unconstrained, DEST_A-only, restored-unconstrained phases: A/B authentication
   to both destinations plus unbound direct A/B signing through all three arms.
   This is 54 primary cells. Stock constrained A fails at both destinations;
   native/repaired constrained A allows DEST_A and refuses DEST_B. Both filters
   refuse B; native B works. Raw constrained A signing refuses everywhere.
3. Start HOP with a separate transport key held outside every outcome agent.
   Forward each agent through real OpenSSH using the transport key to enter HOP.
   Load A first with only direct DEST_A permission, then only HOP>DEST_A permission.
   For each policy and arm, test direct DEST_A and forwarded DEST_A/DEST_B: 18 cells.
   Native/repaired allow only direct DEST_A in the first phase and only forwarded
   DEST_A in the second. Stock constrained A refuses throughout. Native B through
   HOP to both destinations controls path health after each arm's triplet.
   Decode full public bindings: allowed direct requests have DEST_A/authentication;
   allowed forwarded requests have HOP/forwarding then DEST_A/authentication on
   the signing connection. Do not infer analogous IDE-product hop behavior.
4. Restore direct DEST_A policy. On native/repaired paths, exercise valid bound A,
   excluded B, unbound A, invalid bind, cross-connection mismatch, two concurrent
   session mismatches and malformed sign. Bracket each case with allowed A auth.
   Malformed outcomes are descriptive; no parser-hardening claim is made.
5. With native/repaired connections held after successful binding/signing, restart
   only the disposable upstream agent and reload A/B. Retained connections must
   not produce a signature; fresh unbound A refuses and fresh correctly bound A
   signs. Post-restart real authentication qualifies recovery. This does not claim
   transparent reconnection of retained paths.

## Verification and accounting

The checker owns its expected schedule; it must not load expected.json or trust
per-record expected fields. Validate exact identifiers, duplicates, cardinality,
request grammar, public keys, binding signatures/flags, signatures over exact
bytes, SSH command evidence, health controls and decoded chain order. 

One focused final verification batch includes adversarial mutations of copied
in-memory records: omitted case, duplicate case, relabeled request, altered
expectation, corrupted signature and missing control. 
