# H1 follow-up results — 4 October 2026

Core receipt: `evidence/v1/verification.json`; lifecycle receipt:
`evidence/lifecycle-v2/verification.json`; resource receipt:
`evidence/resources-v2/verification-v2.json`.

## Factorial and source-version result

Three fresh qualified fixtures each completed 216 scheduled authentication/direct
signing endpoints and 541 total authentication/direct records including qualification
and health controls. Each also contains 383 state/protocol records, including 150
registered signing expectations. The checker verified 147 signatures per fixture
(441 total across the combined core records) and found no registered mismatch.
Requests are nested observations, not independent samples.

| Path | Constrained A: DEST_A / DEST_B | Unconstrained and restored A | Direct known B |
|---|---|---|---|
| Native / transparent | allow / deny | allow / allow | allow |
| Exact package / original / reverted 0.5.2 | deny / deny | allow / allow | deny |
| Normalized neither (B=0,R=0) | deny / deny | allow / allow | deny |
| Binding only (B=1,R=0) | deny / deny | allow / allow | deny |
| Connection retention only (B=0,R=1) | deny / deny | allow / allow | deny |
| Combined normalized (B=1,R=1) / historical repair | allow / deny | allow / allow | deny |
| Source-built upstream 0.5.3 | deny / deny | allow / allow | deny |
| Dedicated A-only agent | allow / deny | allow / allow | deny |

The four normalized arms share source, wrappers and build flags except the two
macros. Stock original/revert are separate controls. Both changes are needed in
this implementation under the selected fixture. This is not a universal design
theorem. Original/reverted binaries are identical. A fresh source-built 0.5.3
executable confirms functionality loss; no 0.5.3 repair was tested.

## Direct validation of repaired H1

Matched state sequences were sent to native, historical repaired and normalized
combined paths in constrained and unconstrained phases. Qualified controls bracket
each of 23 case labels. The corpus includes cross-connection and concurrent session
mismatches, invalid/missing/repeated/reordered bindings, forwarding states, signed-data
variants, direct excluded B, malformed signing and unknown extension. H1 rejects B;
native restrictions reject invalid constrained contexts. Unconstrained A remains
capable of raw signing, as expected for an identity filter rather than P4.

The malformed-sign case returns a native failure but disconnects on repaired H1;
it is not counted as an authorization denial. Local management requests through
repaired H1 are refused in the tested sequence; no broader parser robustness or
management-policy completeness is inferred. After restarting the original agent,
retained downstream paths become inconclusive/closed. New unbound paths refuse,
and new correctly bound paths sign and authenticate again. There is no demonstrated
automatic retained-connection recovery.

The separate checker does not import the runner. It validates stored signatures,
authentication logs, hashes, registered schedule and accepted bind/sign connection
association. 

## Upstream locking boundary

V1's descriptive diagnostic yielded a valid signature after a refused forwarding
binding while locked. The same result occurred natively and through both repairs
in all three fixtures. The follow-up uses the recorded system agent and a separately
built OpenSSH 10.5p1 agent, with three new qualified fixtures per version and native
versus historical-repaired paths. Each fixture retains real SSH auth qualification
and final permitted/forbidden controls.

| Agent | Forwarding binding while locked | Later DEST_A-shaped signing after unlock | Paths / fixtures |
|---|---|---|---|
| Debian 1:10.0p1-7+deb13u4 (banner 10.0p2) | refused | verified signature | native and repaired, 3 |
| Source-built OpenSSH 10.5p1 | accepted | refused | native and repaired, 3 |

The key allows direct DEST_A, not the attempted extra forwarding hop. The result
concerns omitted path context and reproduces a known upstream lifecycle issue;
it does not demonstrate authentication to forbidden DEST_B or a new filter CVE.
This materially limits any blanket security claim for a functioning repair. The
agent version comparison uses the same new container runtime (including its
documented OpenSSL update), not a one-commit OpenSSH ablation. Historical OS agents'
backport status is not inferred from their version strings.

## Operational alternative and bounded cost

The dedicated A-only agent meets the three endpoint requirements in all three
fixtures. It uses an additional credential source and enrollment of A there;
the repair instead filters an existing multi-identity source. Neither is declared
universally preferable; no human usability or field deployment was measured.

One qualified resource fixture completed 20 sequential plus 8 concurrent permitted
authentications per native/transparent/repaired/stock arm (112 total), with 1/8
held clients and 50 connect/list/disconnect cycles. Stock used unconstrained A and
is a separate diagnostic, not a matched constrained-performance baseline.

| Filter | Baseline FD / threads | 8 held clients | After close | After 50 cycles |
|---|---|---|---|---|
| Repaired H1 | 4 / 1 | 20 / 9 | 4 / 1 | 4 / 1 |
| Stock H1 (unconstrained diagnostic) | 4 / 1 | 12 / 9 | 4 / 1 | 4 / 1 |

Repaired RSS was 5104 KiB initially and 5348 KiB after the cycles; retained allocator
memory is not automatically a leak. Native/transparent agent process samples were
empty and are reported unavailable, not zero. Sequential medians including tracer
overhead were 90.2 ms native, 91.3 ms transparent and 92.6 ms repaired in this single
fixture. These are descriptive timings, not statistical performance comparisons.
No long-idle, sustained load, heap-leak or field-reliability claim follows.

## Attempts and corrections

| Attempt | Status | Treatment |
|---|---|---|
| Cached Docker readiness | Initial daemon unavailable; started local Docker | Readiness only, no outcome collected |
| Upstream source tar endpoint | HTTP 400 for filter snapshot URL | Retrieved tagged source files with hashes instead; not a live exclusion |
| Core v1 | 3 complete qualified fixtures | Canonical new factorial/state evidence |
| Agent build | Targeted 10.5p1 agent compiled successfully | No full build or installation; source and dependency provenance retained |
| Lifecycle v2 | 3 complete fixtures per agent version | Canonical version/lifecycle contrast |
| Resources v1 | 112 completed authentications while separate compilation ran | Retained; timing-confounded, excluded from main resource summary |
| Resources v2 | 112 completed authentications after compilation ended | Canonical bounded workload |
| Resource checker v1 | Empty agent samples summed to zero | Original receipt preserved; v2 checker reports unavailable/null; raw data unchanged |

Every core and lifecycle verification receipt reports zero errors within its stated
checks. 
