# Three-fixture shim amendment, 2026-10-05, before any target measurement

This protocol freezes a second author-operated collection. It does not replace
`evidence/paramiko-shim-v1/`, `receipt-paramiko-shim-v1.json`, the one-fixture
runner, or the vendored Paramiko tree. No target request has been sent under
this schedule. The same research process performs the collection. It is not
independent external reproduction.

Harness version: `paramiko-shim-3fix-v1`. A later code correction must use a
new version and a new evidence directory. Unsuccessful attempts stay on disk.

## Question

On fresh fixtures, does submitting the authenticated HOP transport binding,
with forwarding flag 1, before client bytes, change A→DEST_A the way the
one-fixture intervention did, and does removing that submission restore the
unmodified result?

These are hypotheses. A miss is a result.

| Source policy | Unmodified | Binding shim | Reverted |
| --- | --- | --- | --- |
| Direct `DEST_A` | allow | deny | allow |
| `HOP` plus `HOP>DEST_A` | deny | allow | deny |

## Unit

A fixture is one replication. Requests, signatures, and the two policy
sessions inside a fixture are nested observations. Three fixtures are planned.
Repeating a cell inside a fixture does not increase the replication count.

## Fixture

Each fixture regenerates outcome identities A and B, host keys HOP, DEST_A
and DEST_B, the agent, `known_hosts`, transport sessions, and downstream and
upstream agent connections. Both destinations authorize A and B. The HOP
authorizes only the separate transport credential. Host-key checking is
strict. Identity files passed to the destination clients are public keys.
`IdentitiesOnly` is set. Password, keyboard-interactive, and multiplexing
authentication are off. Servers set `PerSourcePenalties no`.

The binding is the live transport triple captured at `Transport._verify_key`:
HOP host key, initial session identifier, and the server signature already
present in the key-exchange reply. The fixture HOP private key is not used to
create the binding. `ForwardShim` from the one-fixture harness submits
`session-bind@openssh.com` with flag 1 on each upstream agent connection and
waits for `SSH_AGENT_SUCCESS` before copying client bytes. Rejection closes
that path and is not a source-policy denial.

Unmodified and reverted phases use the trace socket and do not add that bind.
Each phase opens a new Paramiko transport. Before the native matrix and again
before every phase, A is removed and added again with that session's
constraint, so a previous add is not reused.

## Order

Unconstrained qualification is first in every fixture. Constrained order then
alternates, to reduce order effects:

| Fixture | After qualification |
| --- | --- |
| 1 | direct `DEST_A`, then `HOP` plus `HOP>DEST_A` |
| 2 | `HOP` plus `HOP>DEST_A`, then direct `DEST_A` |
| 3 | direct `DEST_A`, then `HOP` plus `HOP>DEST_A` |

## Qualification

Native gates are the `native_gates` object in `schedule_paramiko.json`.

| Route | Direct policy | HOP policy |
| --- | --- | --- |
| A direct to DEST_A | allow | deny |
| A forwarded through HOP to DEST_A | deny | allow |

A→DEST_B is deny on those constrained routes. Unconstrained B is allow on the
routes in the native-gate table. Unconstrained native forwarding must carry a
verified HOP session-bind. Unconstrained Paramiko authentication for A and B
to both destinations must allow. A bracket authenticates unconstrained B
directly to both destinations on the source agent before the first policy and
after the last phase.

A fixture that fails qualification or a bracket does not contribute an
intervention result. The attempt and the cause remain in the ledger.

## Outcome classes

Transcript allow requires `CAL_AUTH_OK`, public-key authentication, and exit 0.
Transcript deny requires `Permission denied (publickey)` and exit 255 after
the HOP itself authenticated on a forwarded route. Anything else is
inconclusive.

A transcript deny confirms a predicted denial only when the same-phase B
control to that destination allows, and the agent trace for that attempt is
one of:

- identity-enumeration refusal: an identities answer omits the tested key and
  no signing request for that key follows;
- agent-signing refusal: a signing request for that key receives
  `SSH_AGENT_FAILURE`.

These do not confirm a predicted denial:

- server rejection: a cryptographically valid signature for the tested key is
  followed by destination refusal;
- transport failure: no completed agent exchange, or the HOP login itself
  fails;
- parser disconnect: the agent socket ends without a failure reply and
  without an identities answer;
- binding rejection: the shim does not receive `SSH_AGENT_SUCCESS` and does
  not relay client bytes.

For the shim, a predicted direct-policy denial additionally requires the
accepted flag-1 HOP bind before those client requests. A predicted allow
requires a signature that verifies against the request bytes and the enclosed
public key. Every signature the audit reports as successful is checked that
way. Signature totals are checks, not extra fixtures.

## Environment expected from the one-fixture freeze

The live run must record the actual values. The pins to compare are image
`sha256:1be8bdacf6a429620e7a379d88046a444cc5feb3fc242b51c843a5a00b7d95da`,
Paramiko 4.0.0, vendor tree
`e54dafb72b5903ffc0e61d0e4ed45d9fb9a35366a7ad5655db15b8d25ddef1db`, and the
binary hashes in `evidence/paramiko-shim-v1/freeze.json`. A mismatch is
reported. It is not rewritten into a match.

## What a matching result would mean

If every qualified fixture shows both predicted flips, with the binding
accepted before client bytes and the controls held, the collection shows that
the same harness intervention produced the same A→DEST_A pattern on these
three fixtures as on the retained one-fixture run. 
