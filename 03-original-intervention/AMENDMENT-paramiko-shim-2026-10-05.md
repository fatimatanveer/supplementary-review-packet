# Shim amendment, 2026-10-05, before any shim target measurement

This amendment freezes one bounded causal experiment. It does not change the
pilot, `evidence/paramiko-v1/`, the vendor tree, or
`paper/revised-manuscript.md`. No target request has been sent under this
schedule.

The shim is harness code. It is not Paramiko behavior. Paramiko 4.0.0 remains
the byte-copy forwarder measured in the earlier extension.

## Binding source

OpenSSH `PROTOCOL.agent` defines `session-bind@openssh.com` as a binding to
the session identifier from the initial key exchange (RFC 4253 section 7.2)
and the host key used for that exchange. The signature is the server's
signature of that session identifier, as carried in the initial
`SSH2_MSG_KEXDH_REPLY` / `SSH2_MSG_KEX_ECDH_REPLY`. `is_forwarding` selects
forwarding rather than a direct authentication attempt.

The runner captures that triple from the live Paramiko transport:

- the session identifier is `Transport.session_id` at `_verify_key`, which
  `_set_K_H` has set from the exchange hash H;
- the host key and signature are the arguments of `_verify_key`;
- those signature bytes must occur inside a parsed server packet that
  precedes `SSH_MSG_NEWKEYS` in the socket receive buffer.

A signature produced with the fixture host private key is not used. The
earlier harness loads signer objects for a raw sequence this experiment does
not run. This runner clears those objects before the first session. The
ten-step raw sequence is omitted because its destination bindings would be
signed by the harness. The endpoint is real OpenSSH authentication through
the forwarded socket. The destination client builds its own destination
binding from its own key exchange.

Each Paramiko session has its own key exchange. The shim uses only that
session's captured triple.

## Per-connection forwarding bind

Binding state belongs to one agent connection. One bind for the whole
transport does not cover the next connection. On every accepted upstream
connection during the shim phase, the harness sends one
`session-bind@openssh.com` with the forwarding flag set, waits for
`SSH_AGENT_SUCCESS`, and only then copies client bytes. The success reply is
consumed by the shim and is not written to the client, because the client
has its own request stream. If the bind is rejected, the shim closes the
connection and does not copy client bytes.

The same HOP session identifier may be reused on a new agent connection.
OpenSSH's duplicate-session check is on that connection's socket entry.

## Comparison

One fresh fixture. Sessions, in order: unconstrained qualification, origin
policy `DEST_A`, HOP policy `HOP` plus `HOP>DEST_A`. Origin and HOP each run
three Paramiko phases: unmodified, shim, reverted. Unmodified and reverted
point `SSH_AUTH_SOCK` at the trace socket. The shim phase points it at the
harness listener, which binds and then forwards into that trace.

Native direct and native OpenSSH-forwarded results must match the gates
already frozen in `schedule_paramiko.json`. Unconstrained Paramiko
authentication for A and B to both destinations must allow. In every phase,
B to both destinations must allow, and A to DEST_B must deny. The
unconstrained native forward trace must contain a verified HOP session-bind.
A missing socket, timeout, or EOF is inconclusive, not a denial.

## Predictions

These were fixed before measurement. A miss is a result.

- Unmodified and reverted origin A to DEST_A allow. The shim phase denies.
- Unmodified and reverted HOP-policy A to DEST_A deny. The shim phase allows.
- A to DEST_B stays deny. B stays allow. Those are gates.

If a predicted flip does not occur, the audit first checks that the shim
placed a valid, accepted forwarding bind for this session's captured triple
on every client-bearing agent connection, before that connection's client
bytes. Without that, the run does not test causal sufficiency.

If both flips occur, the only supplement wording this result supports is
that a controlled intervention restores the predicted forwarding-path
behavior in a second implementation, and that this strengthens the causal
argument modestly.