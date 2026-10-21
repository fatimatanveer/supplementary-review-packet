# Prospective authorization-sequence pilot, version 1

This is a new, author-operated study on one device. It does not
alter the previous publication evidence or establish independent reproduction.
Freeze this protocol, generator, checker, dependency sources, binaries, and
generated programs before the first live experiment. Preserve failed attempts.

## Question and falsifiable claims

Does retaining connection context across source-policy transitions expose
authorization discrepancies missed by isolated requests and the existing fixed
23-case suite? A result against an intentionally defective adapter establishes
sensitivity only. A result against the familiar stock filter is rediscovery.
The repaired filter is a regression target, not a held-out implementation.
No result from these targets alone establishes a novel vulnerability.

For a signing request q in history H, define R(H,q) as the cryptographically
verified source-agent decision under the same ordered policy updates and
connection events. For a filter selecting A, the expected decision is R(H,q)
for A and deny for B. Compare signatures, not identity-list visibility.
Source-policy changes are trusted local administrative operations, not attacker
capabilities. An accepted signature after source denial is an authorization
discrepancy; rejection after source acceptance is a liveness discrepancy.
EOF, malformed responses, failed controls, and setup errors are inconclusive,
not policy denials. Binding return codes are descriptive.

## Bounded design

Three fresh fixtures and seeds 51001, 51002, 51003; disposable keys only; pinned
offline Docker image. Two logical connections, valid or invalid signed bindings,
two destinations, raw and hostbound requests, allowed/excluded identities,
close/reopen, and local replacement/removal/restoration of A. No external hosts.
Policies: unconstrained, DEST_A, DEST_B, absent. Replacements retain public A.
Source-agent restart and actual parallel racing are outside this pilot.

Strategies:
1. Four smoke programs (raw and bound A under unconstrained and DEST_A policy).
2. Exact existing 23-case implementation, both policy phases, for comparison.
3. 96 random single-connection programs, no within-program policy changes or
   close/reopen; same request vocabulary as sequence generation.
4. 96 sequence programs: 48 sampled temporal templates and 48 random histories.
   Templates deliberately exercise sign/update/re-sign and close/reopen.

The sequence and stateless strategies share a program budget, not a wire-message
or runtime budget. Report actual costs, unique normalized programs, and first
detection indices. Do not claim statistical superiority from unequal budgets.
Three fixtures are reproducibility checks, not independent users or products.

Arms: original source oracle; familiar stock and repaired filter; selection-only
transparent adapter as negative control; two explicitly seeded adapters:
drop all binding messages, and cache successful signatures by complete request
bytes across policy changes within a program. Adapters run in the harness and
are not external implementations. The cache can replay an already obtained
signature; this is not proof of fresh unauthorized signing or an exploitable
authentication bypass. Cache state resets between programs. They select only A.

Every replay has pre/post functional A and source-B controls under restored
unconstrained policy, in addition to fixture real-SSH qualification. Reset policy
before each replay. Use identical public requests for matched arms. Exclude
unqualified replays. Rotate target ordering across fixtures.

## Retention, reduction, and acceptance

Save all public request/reply bytes and cryptographically verify every claimed
signature. Never save private keys. Record policy commands, fixture and program
identifiers, elapsed time, source/target decisions, and source hashes. Recompute
results from saved evidence with a separate checker that does not trust runtime
classification fields. Freeze generated programs before any live request.

Greedily minimize the first sequence discrepancy per arm and discrepancy class,
with at most 24 candidate replays each. Retain qualification and discrepancy
class, record all attempts, and call the result bounded reduction, not globally
minimal. Report known defects and seeded faults separately from new findings.

Advance to unfamiliar implementations only after this pilot passes its controls
and evidence audit. Before extension, freeze target selection, capability gates,
budgets, and predictions in an amendment. 
