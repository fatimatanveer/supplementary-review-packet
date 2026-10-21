# Results: authorization-sequence pilot and Paramiko 4.0.0 extension

Author-operated measurements on one device. These runs are not
independent reproduction, not a sample of users or products, and not evidence
for a statistical ranking of strategies. 

## Pilot provenance

Study directory `experiments/novelty-sequences-2026-10-05/`. Evidence root
`evidence/pilot-v1/` was left unchanged. Current sources and the three agent
binaries still match `evidence/pilot-v1/freeze.json`. No pilot amendment was
required.

`pilot-v1-receipt.json` and `receipt-pilot-v1.json` are byte-identical
(`sha256:4603a3f45acbdd2cd92c92fc6f32255f50372c38d1b064789275205446b2fbb4`).
The second name is a later copy of the same audit, not a second experiment.
A new saved-evidence replay, `receipt-pilot-v1-recheck-2026-10-05.json`,
passed with 0 errors and 38,662 verified signatures. It is a replay of saved
records, not a fresh live fixture.

The pinned image used for this family of runs remains
`sha256:1be8bdacf6a429620e7a379d88046a444cc5feb3fc242b51c843a5a00b7d95da`.
The corrected agent actually selected by the baseline PATH prefix is
`aa7952eeec9690a95b8c0ee17027ce1438ea6e72ee7bee38dd86a1ef93efa4a4`.

## Pilot denominators

Three fixtures, seeds 51001, 51002 and 51003. Each fixture scheduled 242
programs: 4 smoke, 96 random single-connection, 96 sequence, and 46 fixed
(23 cases in two policy phases). Five comparison arms ran on every program.
Main matched comparisons: 1,210 per fixture and 3,630 overall. Every main
comparison was qualified. Reduction replays are additional and are not part
of the 3,630.

Pooled unique normalized histories are 490 of 726 programs. Within each
fixture the count is 202 of 242. Smoke and the fixed suite repeat across
seeds (4 and 46 unique schedules). All 288 single-connection programs remain
distinct after session-label normalization. The 288 sequence programs
collapse to 152 normalized histories because the 144 template draws repeat
8 skeletons. Program counts are therefore not unique behaviors, and unique
histories are not independent users.

Each arm saw the same target signing requests: 12 smoke, 1,003
single-connection, 943 sequence, and 156 fixed, totaling 2,114 target signing
requests per arm. Paired wire messages recorded for those main comparisons
were 36, 3,604, 2,924 and 588 respectively (7,152 per arm, source replay
included). Paired replay time summed across those comparisons was about 0.33
s, 11 s, 14 s and 3.9 s per arm. Those totals repeat work across arms and
must not be read as five independent samples or as a wall-clock contest.
Evidence timestamps run from 2026-10-04T21:04:56Z through roughly 21:07Z.

## Pilot outcomes

| Arm | Authorization endpoints | Liveness endpoints | Divergent programs | Inconclusive signing requests |
| --- | ---: | ---: | ---: | ---: |
| Familiar stock filter | 0 | 245 | 159 | 6 |
| Repaired filter | 0 | 0 | 0 | 6 |
| Selection-only control | 0 | 0 | 0 | 0 |
| Seeded binding-drop | 0 | 245 | 159 | 0 |
| Seeded signature cache | 106 | 0 | 103 | 0 |

The 245 liveness endpoints are 3 smoke + 27 single-connection + 197 sequence
+ 18 fixed. The cache's 106 authorization endpoints are 100 sequence programs
and 6 fixed-suite endpoints (3 programs, index 44 in each fixture). The cache
found 0 of 288 single-connection programs. Both seeded classes were already
visible in the fixed suite. The pilot does not establish discovery of a new
defect class.

The cache stores a successful reply under the exact request bytes and can
return it after a later source denial. That is not fresh unauthorized signing
and not an exploitable SSH authentication bypass. Cache state resets between
programs. The mutant was not altered so that the fixed suite would miss it.

Nine bounded greedy reductions were saved. Witnesses contain two operations
(binding-drop and stock liveness) or four operations (cache authorization:
bind, sign, policy change, sign). Attempt counts were 5 or 6, under the cap
of 24. These are not proofs of globally minimal sequences.

The six inconclusive signing requests on stock and on the repaired filter are
the fixed suite's `truncated-sign` case in both policy phases, across three
fixtures. Each ended with EOF. EOF is inconclusive, not a policy denial. The
selection and cache adapters answered those malformed requests inside the
harness, so they did not produce the same EOF. That difference is adapter
handling of a truncated frame, not an authorization result. No main program
was excluded for a failed control.

## Paramiko extension

Schedule `AMENDMENT-paramiko-schedule-2026-10-05.md` and
`schedule_paramiko.json` were frozen before the first target request. The
live run used offline image
`sha256:1be8bdacf6a429620e7a379d88046a444cc5feb3fc242b51c843a5a00b7d95da`
with `--network none`. Paramiko 4.0.0 was loaded from extracted Linux arm64
wheels, not from an image `pip` install. The image Python is 3.13.5 and has
no `pip`. Vendored dependencies, which shadow the image's older cryptography
43.0.0 and bcrypt 4.2.0, are listed in `dependency-manifest.json`. The
corrected agent hash above was the process on `PATH`; the image `/usr/bin/ssh-agent`
(`a459a59b06b8be4904ba7cc8345a67d6067bb43482615f28b9e68da869f03c84`) was
recorded and not selected.

Three fresh fixtures each opened three transport sessions: unconstrained
qualification, origin policy `DEST_A`, and HOP policy `HOP` plus
`HOP>DEST_A`. Each session kept two logical connections across a ten-step
remove/restore sequence and then replayed the same public bytes on the source
agent. All three fixtures qualified. `receipt-paramiko-v1.json` recomputed
the records with 0 integrity errors and 66 verified raw signatures. Prediction
tally: 183 confirmed, 0 refuted, 0 inconclusive. A confirmed prediction is
not a new vulnerability.

### Predicted versus observed forwarding

Authentication outcomes below are recomputed from saved client stdout, stderr
and exit status. They were identical in all three fixtures.

| Policy | Endpoint | Native direct | Native OpenSSH forward | Paramiko `AgentRequestHandler` |
| --- | --- | --- | --- | --- |
| Origin `DEST_A` | A → DEST_A | allow | deny | allow |
| Origin `DEST_A` | A → DEST_B | deny | deny | deny |
| HOP `HOP` and `HOP>DEST_A` | A → DEST_A | deny | allow | deny |
| HOP `HOP` and `HOP>DEST_A` | A → DEST_B | deny | deny | deny |
| Either | B → either destination | allow | allow | allow |

Unconstrained qualification allowed A and B to both destinations on the
native direct path, the native forward path, and the Paramiko path. B
remained usable in every constrained session. Removing A never removed B.

### What the traces show

Paramiko's unmodified handler requests `auth-agent-req@openssh.com` and then
copies bytes. On every Paramiko authentication slice the only accepted
`session-bind@openssh.com` host keys were the final destinations, with
forwarding flag 0. No HOP host key appeared. Under origin policy the DEST_A
connection listed A and produced one verified A signature; the DEST_B
connection listed only B and produced none. Under HOP policy every Paramiko
identity answer listed only B, and no A signature was produced.

Native OpenSSH forwarding in the same fixtures accepted HOP host-key bindings
on separate connections with flag 0 and flag 1, then accepted the destination
binding with flag 0. The flag-1 HOP binding sits on the connection that also
carries the destination binding. Under HOP policy that path produced one
verified A signature. Under origin policy it produced none. Native direct
slices contained destination bindings only.

Real-SSH denials on these paths were identity-list withholding followed by
`Permission denied (publickey)`, not a later signature-success after a source
denial. The raw ten-step sequence did send explicit signing requests. After
A was removed, those requests were denied and B still signed. After the same
public A was restored, step 8 matched step 2 on the retained connection,
including the HOP-policy case where both decisions were deny. All 144 live
agent requests matched their source replays in bytes and recomputed decision.
That match is a matched-context result. It does not prove the forwarder
supplied a physical hop. The native-versus-Paramiko traces are what show the
hop binding is absent.

Failed predictions were not rewritten after the run. There was no unexpected
discrepancy to reduce. Setup failures were not encountered, so none were
counted as qualified repetitions. Disposable private keys were not archived.

## What this does and does not support

The pilot shows that the sequence generator can hit seeded faults, and that
the fixed suite already hits both seeded classes. The repaired filter and the
selection control produced no qualified authorization or liveness discrepancy
in this corpus.

The extension shows that Paramiko 4.0.0's documented forwarding API, in this
configuration, preserves final-destination rejection of A at DEST_B and drops
the native direct-versus-HOP distinction. OpenSSH's published destination
constraint and session-bind mechanism already describes that distinction.
The same missing-hop pattern is already reported for nested VS Code sessions
in the current manuscript. A new forwarder row with the predicted outcome is
not a new defect class.

The origin-policy allow through Paramiko is the agent treating the presented
binding as direct use of DEST_A. It is not a signature on a forbidden
destination, not theft of the private key, and not evidence that an attacker
who cannot satisfy DEST_A can obtain a signature.
